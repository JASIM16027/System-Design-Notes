## OAuth2 — বিস্তারিত সহজ ভাষায়

আগে একটা real-life analogy দিয়ে শুরু করি, তারপর technical details।

---

### Hotel Key Card Analogy

তুমি একটি hotel-এ গেলে। Reception তোমাকে একটি key card দিলো। এই card দিয়ে তুমি শুধু তোমার room খুলতে পারবে, gym যেতে পারবে — কিন্তু hotel-এর cash register বা manager-এর office খুলতে পারবে না। Card-এ তোমার password নেই, শুধু একটি limited permission আছে। Card expire হলে কাজ করবে না।

OAuth2 ঠিক এভাবেই কাজ করে। Password না দিয়ে, limited permission-এর একটি temporary card (Access Token) দেওয়া হয়।

---

### চারটি Actor — এদের চেনো আগে

OAuth2-এ মোট চারটি party থাকে। এদের না চিনলে পুরো flow বোঝা কঠিন।

**Resource Owner** — তুমি, মানে user। তোমার Google account তোমার, তুমিই সিদ্ধান্ত নেবে কাকে access দেবে।

**Client** — যে website বা app তোমার Google info চাইছে। যেমন Canva, Notion, বা যেকোনো "Continue with Google" button আছে এমন site।

**Authorization Server** — Google-এর সেই server যে login করায় এবং permission দেয়। এটা শুধু "তুমি কে এবং তুমি কী allow করছ" সেটা দেখে।

**Resource Server** — Google-এর সেই server যেখানে তোমার actual data আছে — নাম, email, profile picture। Authorization Server আলাদা, Resource Server আলাদা।

---

### Step-by-Step Flow — প্রতিটি ধাপ বিস্তারিত

তুমি Canva-তে গিয়ে "Continue with Google" click করলে কী হয় দেখো।

**ধাপ ১ — Client Authorization Request পাঠায়**

Canva তোমাকে সরাসরি Google-এ redirect করে। এই redirect URL-এ কিছু গুরুত্বপূর্ণ parameter থাকে। `client_id` দিয়ে Google জানে কোন app request করছে। `redirect_uri` দিয়ে বলে "কাজ শেষে user-কে কোথায় ফেরত পাঠাবে।" `scope` দিয়ে বলে ঠিক কোন কোন permission চাই — যেমন শুধু `email` আর `profile`, পুরো Google Drive না। `state` হলো একটি random string যা CSRF attack থেকে রক্ষা করে।

```
https://accounts.google.com/o/oauth2/auth
  ?client_id=canva_app_id
  &redirect_uri=https://canva.com/callback
  &scope=email profile
  &response_type=code
  &state=xK9mP2qR
```

**ধাপ ২ — তুমি Google-এ Login করো**

এখন তুমি Google-এর নিজের page-এ আছ। Canva এই page দেখতে পাচ্ছে না, তোমার password Canva জানছে না। Google তোমাকে জিজ্ঞেস করে "Canva কি তোমার নাম আর email দেখতে পারবে?" তুমি Allow করো।

**ধাপ ৩ — Authorization Code আসে**

তুমি Allow করার পর Google তোমাকে Canva-র redirect URI-তে পাঠায়, সাথে একটি short-lived Authorization Code।

```
https://canva.com/callback
  ?code=4/0AX4XfWj8K2mN...
  &state=xK9mP2qR
```

এই code-এর lifetime মাত্র কয়েক মিনিট। এটা দিয়ে সরাসরি কিছু করা যায় না — এটা শুধু একটি intermediate step। `state` parameter check করা হয় প্রথমে — যদি match না করে, request reject।

**ধাপ ৪ — Code দিয়ে Token চাওয়া হয়**

এই step টা browser-এ হয় না, হয় Canva-র backend server-এ, user দেখতে পায় না। Canva সেই code নিয়ে Google-কে বলে "এই code দিলাম, এখন আমাকে Access Token দাও।" এখানে Canva নিজের `client_secret`ও পাঠায় — এটা শুধু Canva আর Google জানে, কোনো user জানে না।

```
POST https://oauth2.googleapis.com/token
  code=4/0AX4XfWj8K2mN...
  client_id=canva_app_id
  client_secret=CANVA_SECRET
  redirect_uri=https://canva.com/callback
  grant_type=authorization_code
```

**ধাপ ৫ — Access Token ও ID Token আসে**

Google verify করে সব ঠিক আছে কিনা, তারপর দুটো জিনিস পাঠায়।

`access_token` — এটা দিয়ে Resource Server থেকে data আনা যাবে। এটার একটি expiry আছে, সাধারণত ১ ঘণ্টা।

`refresh_token` — access token expire হলে এটা দিয়ে user-কে আবার login করাতে না হয়েই নতুন access token নেওয়া যায়।

`id_token` — এটা একটি JWT যেখানে user-এর basic info আছে। এটা OpenID Connect (OIDC)-এর অংশ, OAuth2-এর extension।

**ধাপ ৬ — Access Token দিয়ে Data আনা**

Canva এখন Access Token দিয়ে Google-এর Resource Server-কে জিজ্ঞেস করে user-এর তথ্য।

```
GET https://www.googleapis.com/oauth2/v2/userinfo
Authorization: Bearer ya29.A0ARrdaM...
```

Google দেখে token valid কিনা, expired কিনা, scope match করছে কিনা। সব ঠিক থাকলে নাম আর email পাঠিয়ে দেয়।

---

### Authorization Code কেন? সরাসরি Token কেন না?

এটা অনেকেরই মাথায় আসে। Google সরাসরি Token দিলেই তো হতো, মাঝখানে এই Code-এর ঝামেলা কেন?

কারণ security। যদি Google সরাসরি Access Token browser redirect-এ পাঠাত, সেই token URL-এ দেখা যেত। Browser history-তে থাকত, log-এ থাকত, অন্য কেউ দেখতে পেত। Authorization Code হলো একটি one-time-use intermediate step যেটা browser-এ আসে, কিন্তু actual token exchange হয় server-to-server, browser bypass করে, encrypted HTTPS-এ। Token কখনো browser-এ আসে না।

---

### Scope — Limited Permission-এর মূল concept

OAuth2-এর সবচেয়ে গুরুত্বপূর্ণ idea হলো scope। তুমি পুরো Google account দিচ্ছ না, শুধু specific permission দিচ্ছ।

Canva চাইতে পারে শুধু `email` আর `profile`। Google Calendar app চাইতে পারে `calendar.read`। Google Drive backup app চাইতে পারে `drive.readonly`। কিন্তু কোনো app যদি `drive.delete` scope চায় এবং তার দরকার না থাকে, তুমি বুঝতে পারবে কিছু একটা গড়বড় আছে।

এই কারণেই OAuth2 secure — password না দিয়ে, শুধু দরকারি permission দেওয়া হয়, এবং সেটাও revoke করা যায় যেকোনো সময়।

---

### Authentication vs Authorization — এই দুটো এক জিনিস না

OAuth2 মূলত Authorization-এর জন্য — অর্থাৎ "তুমি কী করতে পারবে।" Authentication — "তুমি কে" — এটার জন্য OAuth2-এর উপরে OpenID Connect (OIDC) layer যোগ হয়েছে, যেটা ID Token দেয়। যখন তুমি "Continue with Google" দিয়ে login করো, technically OAuth2 + OIDC দুটোই কাজ করছে।


চলো step by step পুরো flow টা technically দেখি — প্রতিটি HTTP request/response সহ।OAuth2-এর পুরো technical flow টা একটা interactive stepper-এ দেখাই — প্রতিটা step-এ actual HTTP request/response সহ।এখন পুরো flow-এর একটা bird's-eye view দেখি — সব actor একসাথে কীভাবে interact করে:---


<img width="745" height="421" alt="image" src="https://github.com/user-attachments/assets/08b527bb-3467-41f5-a8e5-e5f77851f10b" />


<img width="776" height="479" alt="image" src="https://github.com/user-attachments/assets/8b3a5255-023f-4142-a890-8e5076838b95" />


<img width="751" height="443" alt="image" src="https://github.com/user-attachments/assets/e11ce822-d122-417f-9334-3393fa509349" />


<img width="739" height="398" alt="image" src="https://github.com/user-attachments/assets/bc5f5d62-bfbb-4b40-8363-04f1a56c40cf" />


<img width="752" height="403" alt="image" src="https://github.com/user-attachments/assets/cff6c0bd-6851-48d4-8ca4-962067483925" />


<img width="764" height="523" alt="image" src="https://github.com/user-attachments/assets/f818ba84-eab0-4fd7-8b5c-adac3ac707de" />

<img width="750" height="513" alt="image" src="https://github.com/user-attachments/assets/1c729dd5-4d60-4c4a-8a48-a0d6fa8d3955" />


<img width="755" height="483" alt="image" src="https://github.com/user-attachments/assets/713304d0-8445-4e26-9837-d66f0d4a72e3" />


<img width="763" height="401" alt="image" src="https://github.com/user-attachments/assets/c2fb8440-bac7-46fa-8c58-0d85e8df9360" />



<img width="838" height="712" alt="image" src="https://github.com/user-attachments/assets/7c664216-dabb-4bde-b68d-6a0db52b222d" />


## কয়েকটা গুরুত্বপূর্ণ technical detail যা সবাই miss করে

**Authorization Code কেন দরকার, সরাসরি Token কেন না?** যদি Token সরাসরি browser redirect-এ আসত, সেটা URL-এ দেখা যেত — browser history, server log, referrer header সব জায়গায় leak হতে পারত। Authorization Code ইচ্ছাকৃতভাবে worthless রাখা হয়েছে `client_secret` ছাড়া — তাই URL-এ আসলেও ক্ষতি নেই।

**PKCE কী এবং কেন?** Public client যেমন mobile app বা SPA-তে `client_secret` securely রাখা যায় না (যে কেউ app unpack করে দেখতে পারে)। PKCE (Proof Key for Code Exchange) এই সমস্যা solve করে — request-এর আগে একটি random `code_verifier` তৈরি হয়, তার hash (`code_challenge`) Auth Server-এ পাঠানো হয়, আর Token exchange-এ original verifier পাঠিয়ে prove করা হয় যে একই party request করছে।

**`state` parameter CSRF কীভাবে আটকায়?** Attacker যদি তোমাকে একটি malicious link-এ click করায় যেটা Google-কে তার নিজের callback URL-এ redirect করে, তাহলে `state` না থাকলে attacker-এর account-এ তোমার token চলে যেত। `state` একটি random নম্বর যা Canva generate করে আর callback-এ verify করে — attacker জানে না তাই match করাতে পারে না।




