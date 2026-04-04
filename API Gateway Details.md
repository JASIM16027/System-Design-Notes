
# API Gateway

[Distributed_Systems_Notebook (2).docx](https://github.com/user-attachments/files/26476636/Distributed_Systems_Notebook.2.docx)


API Gateway একটা অত্যন্ত গুরুত্বপূর্ণ architectural component। চলো ধাপে ধাপে বুঝি — প্রথমে overview, তারপর ভেতরের flow।**API Gateway কি?**

<img width="708" height="460" alt="image" src="https://github.com/user-attachments/assets/d0863cd3-2b12-41cb-a005-515df3cdf17b" />


সহজ ভাষায় বলতে গেলে — API Gateway হলো তোমার পুরো backend system-এর **একটাই দরজা**। সব client (web, mobile, IoT) এই একটা জায়গায় request পাঠায়, Gateway সেটা সঠিক service-এ পৌঁছে দেয়।

এখন দেখো একটা request ভেতরে কি কি ধাপ পার করে:**API Gateway কেন দরকার?**

<img width="826" height="595" alt="image" src="https://github.com/user-attachments/assets/6e23cd5b-138b-4a4c-a231-fe6a0fb3a7ee" />


# DNS • API Gateway • Load Balancer • Service Discovery • High Availability

API Gateway কি?
 API Gateway হলো তোমার পুরো backend-এর single entry point. Client কখনো directly microservice-এ request করে না — সব request আসে gateway-এর মধ্য দিয়ে।

Definition
 API Gateway accepts incoming API requests from clients and routes them to the correct backend microservice based on the API endpoint.
 সহজ কথায়: Client একটা request পাঠায়, Gateway সেটা সঠিক service-এ deliver করে।

Gateway কি কি কাজ করে?
    • SSL Termination — HTTPS decrypt করে, backend plain HTTP-তে পায়
    • JWT Verification — প্রতিটা request-এর token verify করে
    • Rate Limiting — অতিরিক্ত request block করে
    • Routing — endpoint দেখে সঠিক microservice-এ পাঠায়
    • Cache Check — আগের response cache থেকে দিতে পারলে দেয়

Load Balancer vs API Gateway পার্থক্যটা মনে রাখো!

Load Balancer: একই service-এর multiple instances-এ traffic distribute করে। Health, traffic load দেখে সিদ্ধান্ত নেয়।
API Gateway: API endpoint দেখে কোন service-এ যাবে সেটা decide করে। এটা API বোঝে, Load Balancer বোঝে না।
Example: /api/invoice → Invoice service (Gateway decides)  |  Invoice instance 1/2/3 (LB decides)

## API Composition  

Problem — My Orders Page
ধরো তুমি "My Orders" page visit করলে। এই page-এ অনেক data দরকার:
    • Product details (Product Service থেকে)
    • Invoice details (Invoice Service থেকে)
    • Ratings & Reviews (Ratings Service থেকে)
    • Recommendations (Recommendation Service থেকে)

Problem
Mobile client: শুধু 2টা call দরকার (Product + Invoice)
PC client: 4টা call দরকার (সব)
Gateway ছাড়া, প্রতিটা client আলাদাভাবে সব call করতো — এটা complex এবং wasteful।

## Solution — API Composition

API Gateway API Composition দিয়ে এই সমস্যা সমাধান করে:
    1. Client একটাই request পাঠায়: /api/myOrder
    2. Gateway জানে কোন client-এর কি কি data দরকার
    3. Gateway সব relevant services-এ parallel-এ call করে
    4. সব responses একসাথে join করে
    5. Client-কে একটাই unified response দেয়

Key Benefit
Client: 4 calls 1 call. সব orchestration complexity চলে যায় Gateway-এ।


## Authentication  

OAuth 2.0 Flow — Gateway-এ
API Gateway authentication centralize করে। প্রতিটা microservice-কে আলাদাভাবে auth handle করতে হয় না।

Step-by-step flow:
    6. Client Authorization Server-এ গিয়ে access token নেয়
    7. Client প্রতিটা API request-এ সেই token পাঠায় Gateway-এ
    8. Gateway token Authorization Server-এ validate করে
    9. Token valid হলে, Gateway request microservice-এ forward করে
    10. Token invalid হলে, Gateway 401 Unauthorized return করে

গুরুত্বপূর্ণ
Microservices নিজেরা কখনো token verify করে না। এই কাজটা Gateway একাই করে। এই কারণে বলা হয় authentication is centralized at the gateway.


  Rate Limiting  

API Gateway 4 ধরনের rate limiting rule set করতে পারে:

1. Burst Limit
Gateway একসাথে maximum কতগুলো concurrent request handle করবে সেটা define করে। এর বেশি request আসলে HTTP 429 — Too Many Requests return হয়।

2. API Throttling
একটা specific client বা application কতগুলো request করতে পারবে সেটা limit করে। Limit cross করলে সেই client-কে temporarily block করে। অন্য clients-রা impact হয় না।

3. IP-Based Blocking
Known malicious বা abusive IP address থেকে আসা সব request deny করে। Request কোনো service পর্যন্ত পৌঁছায় না।

4. API Queues
যেসব request immediately process করা যাচ্ছে না সেগুলো queue-এ রাখে। Thundering Herd Problem solve করে — যেমন flash sale-এর সময় হঠাৎ massive traffic spike আসলে সব request reject না করে queue-এ ধরে রাখে।


  Service Discovery  

কেন দরকার?
Microservices dynamically scale হয় — নতুন instance আসে, পুরনো চলে যায়, IP address বদলায়। তাই IP hardcode করা সম্ভব না। Service Discovery একটা live registry maintain করে যেখানে প্রতিটা service-এর current location (IP:Port) থাকে।

দুটো Approach আছে
Approach 1 — Self Registration
প্রতিটা microservice startup-এ registry-তে নিজেকে register করে।
Regular heartbeat পাঠায় (প্রতি 30s): 'আমি এখনও alive'
Shutdown-এ নিজেকে deregister করে।

Approach 2 — Health Check Based
Service Discovery system (যেমন Consul, Eureka) নিজেই সব registered services-কে poll করে।
Healthy instances-কেই শুধু registry-তে রাখে।
Down হয়ে যাওয়া instances automatically remove হয়।

Request Flow — Service Discovery সহ
    11. Client → Gateway: /api/order request পাঠায়
    12. Gateway → Service Discovery: "Order Service কোথায়?" জিজ্ঞেস করে
    13. Service Discovery → Gateway: current healthy IP:Port return করে
    14. Gateway → Microservice: সঠিক instance-এ request forward করে

Tools
Popular Service Discovery tools: Eureka, Consul, Zuul


  Region & Availability Zones  

Region কি?
Region হলো একটা geographic area — যেমন ap-southeast-1 মানে Singapore। প্রতিটা Region সম্পূর্ণ independent — নিজের power grid, network, cooling সব আলাদা। একটা Region পুরো down হলেও অন্য Region চলতে থাকে।

Availability Zone (AZ) কি?
AZ হলো একটা Region-এর মধ্যে আলাদা physical data center। যেমন ap-southeast-1a এবং ap-southeast-1b দুটো আলাদা building, আলাদা power supply — কিন্তু low-latency fiber দিয়ে connected।

Rule of Thumb
Production system-এ সবসময় minimum 2টো AZ-এ deploy করতে হয়। একটা AZ down হলে অন্যটা automatically সব traffic handle করে।

Full Architecture — Layer by Layer
Layer 1: DNS
    • Client সবার আগে DNS-এ যায় (AWS Route 53, Azure Traffic Manager)
    • GeoDNS দেখে কোন region সবচেয়ে কাছে — Bangladesh থেকে Singapore, Europe থেকে Frankfurt
    • TTL 60-300s — এই সময় পর্যন্ত IP cache থাকে

Layer 2: API Gateway
    • DNS resolve করে Gateway-এর IP দেয়
    • Gateway SSL decrypt, JWT verify, rate limit, routing — সব করে
    • Service Discovery-কে জিজ্ঞেস করে কোথায় পাঠাবে

Layer 3: Load Balancer
    • Gateway-এর request পেয়ে সেই service-এর instances-এর মধ্যে traffic distribute করে
    • Least-connection algorithm: সবচেয়ে কম busy instance-এ পাঠায়
    • Cross-AZ aware: কোন AZ healthy সেটা জানে

Layer 4: Microservices (AZ1 & AZ2)
    • প্রতিটা service multiple instances-এ চলে (stacked boxes in diagram)
    • AZ1 এবং AZ2 দুটোতেই identical copy deploy থাকে
    • Database: Primary AZ1-এ, Replica AZ2-এ — continuous replication চলে

Failure Scenario — AZ1 পুরো down হলে কি হয়?
    15. Load Balancer-এর health check AZ1-এর সব instances fail detect করে
    16. সব নতুন request AZ2-তে redirect হয়
    17. AZ2-র PostgreSQL replica automatically promote হয়ে primary হয়
    18. Service Registry থেকে AZ1-এর সব instances automatically remove হয়
    19. User মাত্র 5-10 seconds extra latency দেখে — কোনো error নয়

এটাই High Availability (HA)
প্রতিটা layer-এ redundancy — যেকোনো একটা component fail করলে service চলতে থাকে। User কিছুই বোঝে না।


  Full Request Journey  

একটা request-এর পুরো যাত্রা — Client থেকে Database পর্যন্ত:

Time | Layer | কি হয়
0 ms | Client | Browser POST /api/orders পাঠায়। Header-এ JWT token।
1 ms | DNS / GeoDNS | Bangladesh থেকে request → Singapore Load Balancer-এর IP return।
2 ms | CDN / WAF | Static asset হলে এখানেই শেষ। API call হলে WAF attack check করে।
5 ms | API Gateway | SSL decrypt, JWT verify, rate limit check, cache miss, routing decision।
6 ms | Service Discovery | Gateway Consul-কে জিজ্ঞেস করে, healthy instance list পায়।
7 ms | Load Balancer | Least-connection দিয়ে একটা instance বেছে নেয়।
10 ms | Microservice | Business logic চলে। দরকার হলে অন্য service call করে।
12 ms | Redis Cache | Cache hit → 1ms। Cache miss → PostgreSQL → 15-50ms।


মনে রাখো
DNS → CDN/WAF → API Gateway → Service Discovery → Load Balancer → Microservice → Cache/DB
প্রতিটা layer আলাদা কাজ করে। একটা বাদ দিলেও system ঠিকমতো কাজ করে না।

