## Stateful Architecture

Stateful architecture বলতে বোঝায় এমন একটি system যেখানে **server নিজেই client-এর পূর্ববর্তী interaction-এর তথ্য মনে রাখে।** "State" মানে হলো একটি session-এর current condition — কে connected আছে, সে কী করেছে, তার কাছে কী data আছে।

ধরো তুমি একটি bank-এর ATM-এ card ঢোকালে। তুমি PIN দিলে, ATM সেটা verify করলো এবং তোমাকে "authenticated" হিসেবে চিনে রাখলো। এরপর তুমি balance check করলে, তারপর withdraw করলে — প্রতিটি step-এ ATM জানে তুমি কে, কারণ সে state maintain করছে। তুমি কিছু না বললেও সে বোঝে।

Technical ভাবে বলতে গেলে, stateful system-এ server memory বা database-এ একটি session object রাখে। Client প্রথমবার connect করলে server এই session তৈরি করে এবং client-কে একটি session ID দেয়। এরপর প্রতিটি request-এ client শুধু সেই ID পাঠায় — server সেই ID দিয়ে পুরো context খুঁজে বের করে। Client নিজে কোনো information বহন করে না, server-ই সব জানে।

এই approach-এর সবচেয়ে বড় সুবিধা হলো real-time interaction। TCP connection, WebSocket, SSH session, online multiplayer game — এগুলো সবই stateful কারণ এখানে continuous, দ্বিমুখী যোগাযোগ দরকার। Server জানে কে কখন connected, কার কাছে কী packet পাঠানো হয়েছে, connection-এর current state কী।

সমস্যা হলো scalability। যদি তোমার application তিনটি server-এ চলে এবং Rahim-এর session শুধু Server A-তে থাকে, তাহলে Load Balancer যদি Rahim-এর পরের request Server B-তে পাঠায়, সে "তোমাকে চিনি না" বলে error দেবে। এটা solve করতে হয় হয় sticky session দিয়ে (একই user সবসময় একই server-এ যাবে), অথবা Redis-এর মতো shared session store দিয়ে যেখানে সব server একই session data দেখতে পাবে। দুটোই complexity বাড়ায়।

---

## Stateless Architecture

Stateless architecture-এ server কিছুই মনে রাখে না। প্রতিটি request সম্পূর্ণ নিজের মধ্যে — যতটুকু information দরকার সব request-এর সাথেই পাঠাতে হয়। Server একটি request দেখে, process করে, response দেয় এবং সম্পূর্ণ ভুলে যায়।

Vending machine-এর কথা মনে করো। তুমি প্রতিবার পুরো instruction দাও — কোন item, কত টাকা। Machine তোমাকে চেনে না, আগে কী কিনেছ মনে নেই, কিন্তু যেকোনো machine-এ যাও একই কাজ করবে।

আধুনিক web-এ statelessness সাধারণত JWT (JSON Web Token) দিয়ে implement হয়। User login করলে server একটি signed token তৈরি করে দেয়। এই token-এর ভেতরে user ID, role, expiry time সহ দরকারি সব information encoded থাকে। Server এরপর কিছু সংরক্ষণ করে না। Client প্রতিটি request-এ এই পুরো token পাঠায়। Server শুধু cryptographic signature verify করে বুঝতে পারে token টি valid কিনা এবং tamper করা হয়েছে কিনা। কোনো database lookup লাগে না।

এর সবচেয়ে বড় সুবিধা scaling-এ। যেকোনো server যেকোনো request handle করতে পারে কারণ token-এই সব আছে, কোনো shared state নেই। নতুন server যোগ করা মানেই capacity বাড়া। একটি server crash করলে অন্য server seamlessly request নেয়। এই কারণেই REST API, microservices, এবং cloud-native application সব stateless।

একটি গুরুত্বপূর্ণ সমস্যা আছে — token revoke করা কঠিন। JWT একবার issue হলে সেটার expiry আসার আগে server সেটা invalid করতে পারে না, কারণ server কিছুই store করেনি। User logout করলেও token technically valid থাকে। এটা solve করতে Redis-এ একটি blacklist রাখতে হয়, যা আবার partially stateful হয়ে যায়।

---

## মূল পার্থক্য এক কথায়

Stateful মানে — **server মনে রাখে, client ভুলে যায়।** Stateless মানে — **client মনে রাখে, server ভুলে যায়।**

আর modern production system-এ সাধারণত দুটোই থাকে। HTTP layer stateless (JWT দিয়ে), WebSocket layer stateful (real-time connection), আর database layer সবসময় stateful — কারণ actual data তো কোথাও না কোথাও থাকতেই হবে।


---

## ১. HTTP-এর মূল nature: Stateless by design

HTTP protocol জন্মগতভাবে stateless। প্রতিটা HTTP request সম্পূর্ণ independent। এটা বোঝার জন্য দেখো request-response cycle টা কীভাবে কাজ করে:---

<img width="730" height="619" alt="image" src="https://github.com/user-attachments/assets/fdd8e332-dc95-4224-b36e-6f7e3f7d3ce5" />


## ২. Scaling problem — এখানেই stateful ভেঙে পড়ে

৩ টা user request আসছে। Stateful system-এ কী হয় দেখো:---


<img width="704" height="601" alt="image" src="https://github.com/user-attachments/assets/fee6678d-fe47-48c4-85c9-6d9062bc4536" />


## ৩. JWT — Statelessness-এর technical backbone

Stateless authentication-এর সবচেয়ে popular implementation হলো JWT (JSON Web Token)। এটা তিনটা part নিয়ে তৈরি:---

<img width="769" height="491" alt="image" src="https://github.com/user-attachments/assets/eadf2fb2-6719-48f0-9d30-b35582303fc4" />


## ৪. Real-world architecture — Modern hybrid approach

Production-grade system-এ usually দুটোর combination ব্যবহার হয়:---

<img width="754" height="634" alt="image" src="https://github.com/user-attachments/assets/11ab3f6b-ecda-4f82-a4f2-be8b8bf6e306" />


## Technical summary যা মনে রাখা দরকার

**Stateless-এর গভীর সমস্যা:** Token revoke করা কঠিন। JWT একবার issue হলে expire হওয়া পর্যন্ত valid থাকে — user logout করলেও server জানে না। এটা solve করতে Redis-এ token blacklist রাখতে হয়, যা partially stateful হয়ে যায়।

**Stateful-এর গভীর সমস্যা:** WebSocket connection যদি একটা নির্দিষ্ট server-এ থাকে, সেই server crash করলে সব active connection শেষ। তাই production-এ session replication বা shared state store (Redis Cluster) দরকার।

**Protocol-level পার্থক্য:**
- TCP — stateful, handshake করে connection establish করে (SYN→SYN-ACK→ACK), তারপর state track করে
- UDP — stateless, প্রতিটা packet independent, delivery guarantee নেই
- HTTP/1.1 — stateless, তবে `Connection: keep-alive` দিয়ে TCP connection reuse করে
- HTTP/2 — stateless request কিন্তু stateful connection (multiplexing, server push)

