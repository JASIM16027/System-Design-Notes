
## 🚪 API Gateway কি?

<img width="797" height="340" alt="image" src="https://github.com/user-attachments/assets/7ac982d2-8842-4875-9a7a-ac3fbff1e77d" />

সহজভাবে বলতে গেলে,
👉 **API Gateway হলো এমন একটি system** যেটা client থেকে আসা API request receive করে এবং সেটা **সঠিক backend service-এ পাঠিয়ে দেয়** (endpoint অনুযায়ী)।

---

## 🔄 Request Flow (Step-by-step)

1️⃣ Client → API Gateway
👉 Client একটা request পাঠায় (যেমন: `/api/invoice`)

2️⃣ Gateway Decision
👉 Gateway দেখে endpoint কি
👉 decide করে কোন service-এ যাবে

3️⃣ Routing

* `/api/invoice` → Invoice Microservice
* `/api/order` → Order Microservice
* `/api/sales` → Sales Microservice

4️⃣ Gateway → Microservice
👉 Gateway request সঠিক service-এ forward করে

---

## 🎯 Key Insight

👉 Client কখনো microservice-এর actual address জানে না ❌

👉 সবকিছু handle করে শুধু Gateway

👉 তাই Gateway হলো:

## **Single Entry Point**

---

## 🧠 Simple Diagram (Text)

```
Client
   ↓
API Gateway
   ↓
-------------------------
|  Invoice Service      |
|  Order Service        |
|  Sales Service        |
-------------------------
```

---

## 🧾 Summary

* Client → শুধু 1টা request করে
* Gateway → decide করে কোথায় যাবে
* Service addresses → hidden থাকে
* System → simple + scalable হয়

---


---

## ⚖️ Load Balancer vs API Gateway


<img width="803" height="274" alt="image" src="https://github.com/user-attachments/assets/655db73d-bd2f-4490-adb0-017c1e89bb5e" />

---

## ❓ প্রশ্ন: Load Balancer কি Gateway-এর মতোই কাজ করে?

👉 **না — দুটো আলাদা কাজ করে।**

---

## 🔹 Load Balancer কি করে?

Load Balancer-এর কাজ খুব specific 👇

👉 একই microservice-এর **multiple instance-এর মধ্যে traffic distribute করা**

👉 এটা দেখে:

* কোন instance healthy
* কোনটা কম busy

👉 তারপর decide করে:
👉 **"এই service-এর কোন instance-এ request যাবে?"**

📌 Example:
Invoice Service-এর 3টা instance আছে → LB decide করবে

* Invoice-1
* Invoice-2
* Invoice-3

---

## 🔹 API Gateway কি করে?

API Gateway পুরো system-এর **brain** 👇

👉 এটা দেখে API endpoint
👉 তারপর decide করে:
👉 **"কোন service-এ request যাবে?"**

📌 Example:

* `/api/invoice` → Invoice Service
* `/api/order` → Order Service

---

## 🔄 তারা একসাথে কিভাবে কাজ করে?

1️⃣ Client → API Gateway
👉 request পাঠায় (`/api/invoice`)

2️⃣ Gateway Decision
👉 decide করে → Invoice Service

3️⃣ Gateway → Load Balancer
👉 request পাঠায় Invoice service-এর LB-এ

4️⃣ Load Balancer Decision
👉 decide করে → কোন instance কম busy

5️⃣ Load Balancer → Instance
👉 request forward করে

---

## 🎯 Key Insight

👉 API Gateway কখনো instance choose করে না ❌
👉 Load Balancer কখনো service choose করে না ❌

👉 তাদের কাজ আলাদা:

* Gateway → **Which service?**
* Load Balancer → **Which instance?**

---

## 🧠 Simple Diagram (Text)

```id="u97gdf"
Client
   ↓
API Gateway  →  (decides service)
   ↓
Load Balancer → (decides instance)
   ↓
-------------------------
| Invoice-1 | Invoice-2 |
| Invoice-3              |
-------------------------
```

---

## 🧾 Summary

* Gateway → routing (service level)
* Load Balancer → distribution (instance level)
* দুটো একসাথে কাজ করে system scalable করে

---



## 🔐 Authentication (OAuth 2.0 Flow)

<img width="760" height="313" alt="image" src="https://github.com/user-attachments/assets/dbc2f230-2fb5-48ea-9789-0e216dda2ae1" />

Authentication পুরো system-এ **API Gateway-এ centralized থাকে**।
👉 Microservices-গুলো নিজেরা authentication handle করে না ❌

---

## 🔄 Step-by-step Flow

1️⃣ Client → Authorization Server
👉 প্রথমে client access token নিতে যায়

2️⃣ Authorization Server → Client
👉 valid credentials হলে access token দেয়

3️⃣ Client → API Gateway
👉 প্রতিটা API request-এর সাথে token পাঠায়

4️⃣ Gateway → Authorization Server
👉 token verify করার জন্য check করে

5️⃣ Validation Result

* ✅ Token valid → Gateway request microservice-এ forward করে
* ❌ Token invalid → Gateway `401 Unauthorized` return করে

---




## 🔍 Service Discovery

<img width="851" height="499" alt="image" src="https://github.com/user-attachments/assets/acdb7e7e-4fa7-4843-a8d2-4ebcd8ccf0af" />

Microservices যখন dynamically scale করে (increase/decrease হয়), তখন তাদের **IP address এবং port বারবার change হয়**।

👉 তাই কোনো service-এর address hardcode করে রাখা সম্ভব না ❌

এই সমস্যার সমাধানই হলো **Service Discovery**

👉 এটা একটা **live registry (directory)** রাখে যেখানে সব service instance-এর current location (IP:Port) থাকে

---

## 🧩 Registry update করার 2টা approach

---

### **Approach 1 — Self-registration**

এই ক্ষেত্রে microservice নিজেই responsible

👉 Flow:

* Service startup হলে → registry-তে নিজেকে register করে
* Running অবস্থায় → নিয়মিত heartbeat পাঠায়
  👉 "আমি এখনও alive"
* Shutdown হলে → নিজেকে deregister করে

📌 মানে service নিজেই বলে:
👉 আমি আছি / আমি নেই

---

### **Approach 2 — Health-check-based**

এখানে Service Discovery system নিজেই control করে

👉 Example: Eureka, Consul

👉 Flow:

* Discovery system সব service-কে regularly poll করে
* যারা healthy → registry-তে রাখে
* যারা response দেয় না → remove করে

📌 মানে:
👉 system নিজেই check করে কে alive

---

## 🔄 Request Flow (Step-by-step)

একটা request কিভাবে flow করে দেখো:

1️⃣ Client → Gateway
👉 `/api/order` request পাঠায়

2️⃣ Gateway → Service Discovery
👉 জিজ্ঞেস করে:
**"Order service এখন কোথায় চলছে?"**

3️⃣ Service Discovery → Gateway
👉 reply দেয়:
**current healthy IP:Port**

4️⃣ Gateway → Microservice
👉 সেই correct instance-এ request পাঠায়

---

## 🎯 Key Insight

👉 Gateway কখনো hardcoded IP ব্যবহার করে না ❌

👉 সবসময় Service Discovery-কে জিজ্ঞেস করে

👉 তাই registry-ই হলো:

## **single source of truth**

---

## 🧠 Quick Summary

* Service Discovery = live service registry
* IP change হলেও system break হয় না
* Self-registration vs Health-check → দুইটা approach
* Gateway সবসময় dynamic info use করে

---


---

## 🧭 Full Architecture — Layer by Layer

<img width="801" height="622" alt="image" src="https://github.com/user-attachments/assets/6c665f8c-65e0-46b1-9cc9-0f08a6dacbde" />


---

### **Client → DNS**

Client কখনো সরাসরি তোমার backend/server-এ যায় না।
প্রথমে সে DNS-এ request পাঠায় (যেমন: AWS Route 53, Azure Traffic Manager)।

👉 DNS শুধু domain → IP map করে না, এর আরো কাজ আছে:

* **GeoDNS routing** → user কোন region-এর কাছাকাছি সেটা দেখে
* **Health check** → কোন region healthy সেটা check করে
* **Failover** → একটা region down হলে অন্যটায় redirect করে

📌 Example:
Bangladesh থেকে request → Singapore region
Region 1 down → automatically Region 2

---

### **DNS → API Gateway**

DNS resolve করে API Gateway-এর IP দেয়।

👉 API Gateway হলো পুরো system-এর **main entry point (front door)**

এখানে request আসার পর:

* SSL decrypt হয়
* JWT authentication verify হয়
* Rate limiting check হয়
* Routing decide হয়
* API composition করা হয়

📌 গুরুত্বপূর্ণ:
👉 Microservice-এ যাওয়ার আগেই সব control এখানে হয়ে যায়

---

### **API Gateway ↔ Service Discovery**

Gateway কোনো service-এর IP hardcode করে রাখে না ❌

👉 request forward করার আগে Gateway জিজ্ঞেস করে:

> "এই service এখন কোথায় চলছে?"

Service Discovery (Eureka / Consul) reply দেয়:
👉 current healthy IP:Port

📌 এর সুবিধা:

* নতুন instance add হলে automatically detect
* crash হলে remove
* scaling dynamic হয়

👉 তাই Gateway সবসময় **latest system state জানে**

---

### **Gateway → Load Balancer**

Gateway জানার পর কোন service লাগবে, সে request পাঠায় ওই service-এর **Load Balancer**-এ

👉 প্রতিটা microservice-এর আলাদা load balancer থাকে

📌 Gateway decide করে:
👉 কোন service

📌 Load Balancer decide করে:
👉 কোন instance

---

### **Load Balancer → Microservice Instances**

Load Balancer request distribute করে multiple instances-এ

👉 Algorithms:

* Least connection
* Round robin
* Health check

📌 Diagram-এর stacked boxes = multiple running instances

👉 এক instance crash করলে অন্যগুলো handle করে

---

### **AZ1 & AZ2 — কেন দুইটা Zone?**

AZ (Availability Zone) = একই region-এর ভিতরে আলাদা data center

👉 AZ1 এবং AZ2:

* আলাদা building
* আলাদা power supply
* আলাদা network

📌 দুটোতেই একই services deploy থাকে (identical copy)

---

### **Traffic Flow across AZ**

Gateway একসাথে AZ1 + AZ2-তে traffic পাঠায়

👉 যদি AZ1 down হয়:

* Gateway instantly সব traffic AZ2-তে পাঠায়
* User শুধু একটু latency notice করে
* কোনো downtime হয় না

📌 এটাকেই বলে:

## **High Availability (HA)**

---

### **Extra Protection (Inside AZ)**

একটা AZ-এর ভিতরেও:

* multiple instances থাকে
* একটা instance crash → service down হয় না

👉 তাই protection দুই layer-এ:

1. Instance level
2. AZ level

---

## 🧠 Summary (Shortcut)

👉 Flow:
**Client → DNS → API Gateway → Service Discovery → Load Balancer → Microservice (AZ1 & AZ2)**

👉 Key ideas:

* DNS = smart traffic director
* Gateway = brain (control + routing)
* Service Discovery = live registry
* Load Balancer = traffic distributor
* AZ = fault isolation

---



# API Gateway


[Distributed_Systems_Notebook (2).docx](https://github.com/user-attachments/files/26476636/Distributed_Systems_Notebook.2.docx)


API Gateway একটা অত্যন্ত গুরুত্বপূর্ণ architectural component। চলো ধাপে ধাপে বুঝি — প্রথমে overview, তারপর ভেতরের flow।**API Gateway কি?**

<img width="708" height="460" alt="image" src="https://github.com/user-attachments/assets/d0863cd3-2b12-41cb-a005-515df3cdf17b" />


সহজ ভাষায় বলতে গেলে — API Gateway হলো তোমার পুরো backend system-এর **একটাই দরজা**। সব client (web, mobile, IoT) এই একটা জায়গায় request পাঠায়, Gateway সেটা সঠিক service-এ পৌঁছে দেয়।

এখন দেখো একটা request ভেতরে কি কি ধাপ পার করে:**API Gateway কেন দরকার?**

<img width="826" height="595" alt="image" src="https://github.com/user-attachments/assets/6e23cd5b-138b-4a4c-a231-fe6a0fb3a7ee" />

<img width="1061" height="895" alt="image" src="https://github.com/user-attachments/assets/d186afbd-fbf7-4ebb-a2c9-01659a34b3b0" />
<img width="940" height="744" alt="image" src="https://github.com/user-attachments/assets/5807b3d7-d24f-4194-a023-1447dc430f28" />

<img width="1012" height="736" alt="image" src="https://github.com/user-attachments/assets/183108ad-c4ac-4acb-87c6-84186eda8565" />

<img width="1007" height="741" alt="image" src="https://github.com/user-attachments/assets/a5d9d4b7-dba1-438e-846b-7e7a35a05949" />

<img width="1012" height="882" alt="image" src="https://github.com/user-attachments/assets/972efeb0-ffe0-42af-a2fb-792e3aca63de" />

<img width="1054" height="875" alt="image" src="https://github.com/user-attachments/assets/535c6559-d93e-4253-a955-660c93260728" />

<img width="1049" height="886" alt="image" src="https://github.com/user-attachments/assets/60ff37f6-a461-4014-a480-87df1e90596a" />


<img width="986" height="761" alt="image" src="https://github.com/user-attachments/assets/3470aff9-38c8-49ad-8e4d-154a3387f3a7" />


মনে রাখো
DNS → CDN/WAF → API Gateway → Service Discovery → Load Balancer → Microservice → Cache/DB
প্রতিটা layer আলাদা কাজ করে। একটা বাদ দিলেও system ঠিকমতো কাজ করে না।

