
# API Gateway

API Gateway একটা অত্যন্ত গুরুত্বপূর্ণ architectural component। চলো ধাপে ধাপে বুঝি — প্রথমে overview, তারপর ভেতরের flow।**API Gateway কি?**

<img width="708" height="460" alt="image" src="https://github.com/user-attachments/assets/d0863cd3-2b12-41cb-a005-515df3cdf17b" />


সহজ ভাষায় বলতে গেলে — API Gateway হলো তোমার পুরো backend system-এর **একটাই দরজা**। সব client (web, mobile, IoT) এই একটা জায়গায় request পাঠায়, Gateway সেটা সঠিক service-এ পৌঁছে দেয়।

এখন দেখো একটা request ভেতরে কি কি ধাপ পার করে:**API Gateway কেন দরকার?**

<img width="826" height="595" alt="image" src="https://github.com/user-attachments/assets/6e23cd5b-138b-4a4c-a231-fe6a0fb3a7ee" />


Gateway ছাড়া প্রতিটা client-কে প্রতিটা service-এর address আলাদাভাবে জানতে হতো, auth বারবার করতে হতো, এবং কোনো central control থাকতো না। Gateway এই সমস্যাগুলো একটা জায়গা থেকে solve করে।

**Gateway-এর মূল কাজগুলো এক নজরে:**

**Auth & Authorization** — JWT token বা API key দেখে verify করে। ভুল হলে request আর ভেতরে যায় না।

**Rate Limiting** — একজন user প্রতি মিনিটে সীমার বেশি request করলে `429 Too Many Requests` ফেরত দেয়। DDoS attack আটকায়।

**Load Balancing** — একই service যদি ৩টা instance চলে, gateway সমানভাবে request ভাগ করে দেয়।

**Request Routing** — `/users/*` গেলে User Service-এ, `/orders/*` গেলে Order Service-এ — path দেখে ঠিক করে।

**Logging & Monitoring** — কোন endpoint কতবার hit হলো, latency কত, error কোথায় — সব রেকর্ড রাখে।

**কোথায় ব্যবহার হয়?**

Netflix, Amazon, Uber — সবাই API Gateway ব্যবহার করে। জনপ্রিয় tools হলো AWS API Gateway, Kong, Nginx, Traefik, এবং Apigee।

কোনো specific বিষয় আরো গভীরে জানতে চাইলে বলো — যেমন rate limiting algorithm, auth flow, বা কিভাবে নিজে একটা gateway বানাবে।
