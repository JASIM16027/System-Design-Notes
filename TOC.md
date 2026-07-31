# 📚 Table of Contents — System Design Notes

## 🟢 Fundamentals

| # | Topic | কী আছে |
|---|-------|--------|
| 1 | [REST API](1.%20REST%20API%20-%20Complete%20Notes%20with%20Diagrams.md) | Resource, State, Representation, Transfer · HTTP methods & status codes · URL design · REST vs SOAP · Auth · E-commerce example |
| 2 | [HTTP Versions](2.%20HTTP%20Versions%20-%20Complete%20Details.md) | HTTP/1.0 → 1.1 → 2 · Keep-alive, pipelining, multiplexing · তুলনা টেবিল ও migration |
| A1 | [Type `www.google.com`](A1.What%20Happens%20When%20You%20Type%20%60www.google.com%60%20in%20a%20Browser%3F.md) | DNS resolution-এর ৮ ধাপ — browser cache থেকে actual connection পর্যন্ত |

## 🔐 State & Authentication

| # | Topic | কী আছে |
|---|-------|--------|
| 3 | [Stateful vs Stateless](3.%20Stateful%20vs%20Stateless%20Architecture.md) | দুই architecture-এর পার্থক্য · Scaling problem · JWT কেন statelessness-এর ভিত্তি |
| 4 | [Token & JWT Auth](4.%20Token-Based%20and%20JWT%20Authentication.md) | Login → token → verify পুরো ৭ ধাপ · Token-based auth · JWT structure |
| 5 | [OAuth2](5.%20How%20OAuth2%20works.md) | OAuth2 flow সহজ ভাষায় · যেসব technical detail সবাই miss করে |

## 🏗️ Infrastructure

| # | Topic | কী আছে |
|---|-------|--------|
| 6 | [API Gateway](6.%20API%20Gateway%20Details.md) | Single entry point · Request flow · Load Balancer vs Gateway · Service discovery · Full architecture |
| 7 | [Proxy Types](7.%20Proxy%20,%20Forward%20Proxy%20এবং%20Reverse%20Proxy%20.md) | Forward vs Reverse proxy · কে কার পক্ষে কাজ করে · HTTP header-এ কী দেখায় |
| 10 | [Scalability](10.%20Scalability%20-%20Vertical%20vs%20Horizontal%20Scaling.md) | Scale up vs scale out · Stateless কেন জরুরি · System-এর ৭ ধাপের evolution |
| 11 | [Load Balancer](11.%20Load%20Balancer%20-%20Complete%20Guide.md) | ৬টা algorithm · Health check · L4 vs L7 · Sticky session · SSL termination |
| 12 | [Caching & Redis](12.%20Caching%20-%20Strategies,%20Redis%20and%20Eviction.md) | ৫টা caching strategy · LRU/LFU eviction · Stampede, penetration, avalanche · Redis vs Memcached |
| 13 | [CDN](13.%20CDN%20-%20Content%20Delivery%20Network.md) | Edge server · Push vs Pull · Cache-Control header · Cache busting · Edge computing |

## 🗄️ Data Layer

| # | Topic | কী আছে |
|---|-------|--------|
| 14 | [Database Basics](14.%20Database%20-%20SQL%20vs%20NoSQL,%20Indexing%20and%20Transactions.md) | SQL vs NoSQL-এর ৪ ধরন · ACID vs BASE · B+ Tree indexing · Isolation level · N+1 problem |
| 15 | [Database Scaling](15.%20Database%20Scaling%20-%20Replication,%20Sharding%20and%20Partitioning.md) | Scaling-এর ৭ ধাপ · Replication lag · Failover · Sharding strategy · Shard key বাছা |
| 19 | [Consistent Hashing](19.%20Consistent%20Hashing.md) | `hash % N`-এর সমস্যা · Hash ring · Virtual node · Replication · কোড সহ |

## 🌐 Distributed Systems

| # | Topic | কী আছে |
|---|-------|--------|
| 16 | [CAP & Consistency](16.%20CAP%20Theorem%20and%20Consistency%20Models.md) | CP vs AP · PACELC · ৬টা consistency model · Quorum (W+R>N) · কোন DB কোন দিকে |
| 17 | [Message Queue](17.%20Message%20Queue%20and%20Event-Driven%20Architecture.md) | Kafka vs RabbitMQ · Delivery guarantee · Idempotency · DLQ · Event sourcing · Saga |
| 18 | [Microservices](18.%20Monolith%20vs%20Microservices.md) | কখন কোনটা · Distributed monolith · Circuit breaker · Observability · Strangler Fig |
| 21 | [Real-time Communication](21.%20Real-time%20Communication%20-%20Polling,%20SSE,%20WebSocket.md) | Polling vs Long polling vs SSE vs WebSocket · WebSocket scaling · Redis pub/sub backplane |

## 📐 Design Skills

| # | Topic | কী আছে |
|---|-------|--------|
| 8 | [Functional vs Non-Functional](8.%20Functional%20vs%20Non-Functional%20Requirements.md) | FR vs NFR · Availability-র 9 হিসাব · Consistency vs Availability · Latency vs Throughput |
| 9 | [Back of the Envelope](9.%20Back%20of%20the%20Envelope%20Estimation.md) | Zeros গোনার trick · Magic table · Storage ও QPS formula · Facebook, YouTube, WhatsApp walkthrough |
| 20 | [Rate Limiting](20.%20Rate%20Limiting%20-%20Algorithms%20and%20Design.md) | ৫টা algorithm · Token bucket vs leaky bucket · Distributed rate limiting · Redis + Lua |

## 🎯 Interview Preparation

| # | Topic | কী আছে |
|---|-------|--------|
| 22 | [Interview Framework & Case Studies](22.%20System%20Design%20Interview%20Framework%20and%20Case%20Studies.md) | ৪৫ মিনিটের framework · ৬টা full case study — URL shortener, WhatsApp, News feed, YouTube, Uber, Notification |
| 23 | [Question Bank](23.%20Interview%20Question%20Bank%20-%20120+%20Q&A.md) | ১১০+ প্রশ্ন-উত্তর, ১২টা বিভাগে সাজানো · সবচেয়ে বেশি জিজ্ঞাসিত ১৫টার দ্রুত পুনরাবৃত্তি |

---

## 📖 পড়ার ক্রম

**নতুন হলে:** 1 → 2 → A1 → 3 → 4 → 8 → 9 → 10 → 11

**মাঝারি:** 12 → 13 → 14 → 15 → 16 → 6 → 7 → 5

**Advanced:** 17 → 18 → 19 → 20 → 21

**Interview-এর আগে:** 22 → 23 (আর 9 নম্বরের formula গুলো মুখস্থ)
