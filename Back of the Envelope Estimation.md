---

# Back of the Envelope Estimation — সম্পূর্ণ গাইড

## এটা আসলে কী?

ধরো তুমি একটা interview-তে বসে আছো। Interviewer জিজ্ঞেস করলো — "YouTube-এর জন্য system design করো।" তুমি কি সাথে সাথে architecture আঁকতে শুরু করবে? না। আগে বুঝতে হবে system কত বড়। কত user? কত data? কত request per second?

এই হিসাবটাই হলো Back of the Envelope Estimation। কোনো exact calculation না — শুধু rough idea। একটা কাগজের পিছনে যেভাবে দ্রুত হিসাব করা যায়, সেটাই।

---

## সবচেয়ে বড় Trick — Zeros গোনা

প্রতিটা unit আসলে কিছু zeros। এটা মাথায় ঢুকলেই সব calculation automatic হয়ে যায়।

- KB = 10³ → **3 টা zero**
- MB = 10⁶ → **6 টা zero**
- GB = 10⁹ → **9 টা zero**
- TB = 10¹² → **12 টা zero**
- PB = 10¹⁵ → **15 টা zero**

একইভাবে users-এর ক্ষেত্রে:

- Million = 10⁶ → **6 টা zero**
- Billion = 10⁹ → **9 টা zero**

**Rule:** দুটো জিনিস multiply করলে শুধু তাদের zeros যোগ করো, তাহলেই answer পেয়ে যাবে।

উদাহরণ: Million users × KB data = 6 + 3 = **9 zeros = GB**। ব্যস, এটুকুই।

---

## The Magic Table — মুখস্থ করো

| Users | Payload | Zeros যোগ | Result |
|---|---|---|---|
| 1 Million | 1 KB | 6 + 3 = 9 | **1 GB** |
| 1 Billion | 1 KB | 9 + 3 = 12 | **1 TB** |
| 1 Million | 1 MB | 6 + 6 = 12 | **1 TB** |
| 1 Billion | 1 MB | 9 + 6 = 15 | **1 PB** |
| 1 Million | 1 GB | 6 + 9 = 15 | **1 PB** |
| 1 Billion | 1 GB | 9 + 9 = 18 | **1 EB** |

---

## Storage বের করার Formula

**Total Storage = Users × Operations per user per day × Size per operation**

এটুকু মনে রাখলেই যেকোনো storage estimation করা যায়।

---

## QPS বের করার Formula

QPS মানে Queries Per Second — মানে প্রতি সেকেন্ডে কতটা request system-এ আসছে।

**QPS = (Users × Requests per user per day) ÷ 86,400**

86,400 কোথা থেকে আসলো? এক দিনে মোট seconds = 24 × 60 × 60 = 86,400।

Quick mental math এর জন্য 86,400 কে 100,000 ধরে নাও। এতে answer মাত্র 14% কম আসে, কিন্তু calculation অনেক সহজ হয়ে যায়। Estimation-এ এটুকু error সম্পূর্ণ acceptable।

---

## Facebook — Full Walkthrough

**Assumptions:**

- Daily Active Users = 10 Billion
- প্রতি user গড়ে 1 post per day
- প্রতিটি post = 1 KB (শুধু text)
- প্রতি user daily = 4 reads + 2 writes = 6 requests

**Storage calculation:**

10 Billion × 1 × 1 KB = 10 Billion KB। এখন zeros গোনো — Billion মানে 9 zeros, KB মানে 3 zeros, মোট 12 zeros মানে TB। তাই **10 TB per day**।

এক বছরে = 10 TB × 365 = 3,650 TB = **3.65 PB per year**।

**QPS calculation:**

Total daily requests = 10 Billion × 6 = 60 Billion।

QPS = 60 Billion ÷ 86,400 = **≈ 700,000 requests per second**।

মানে Facebook-কে গড়ে প্রতি সেকেন্ডে 7 লাখ request handle করতে হয়।

---

## YouTube — Full Walkthrough

**Assumptions:**

- Daily uploaders = 50 Million
- প্রতি uploader = 1 video per day
- প্রতিটি video = 100 MB

**Storage calculation:**

50 Million × 1 × 100 MB = 5,000 Million MB = 5 Billion MB। Billion মানে 9 zeros, MB মানে 6 zeros, মোট 15 zeros মানে PB। তাই **5 PB per day**।

কিন্তু বাস্তবে একটা video শুধু raw file হিসেবে থাকে না। 360p, 480p, 720p, 1080p — এই সব resolution-এ রাখতে হয়। তাই actual storage হয় raw-এর 3 গুণ পর্যন্ত। মানে **প্রায় 15 PB per day**।

---

## WhatsApp — Full Walkthrough

**Assumptions:**

- Daily active users = 2 Billion
- প্রতি user = 20 messages per day
- প্রতিটি message = 1 KB

**Storage calculation:**

2 Billion × 20 × 1 KB = 40 Billion KB = **40 TB per day**।

**QPS calculation:**

ধরি প্রতি user = 40 requests per day (send + receive + status update ইত্যাদি)।

Total = 2 Billion × 40 = 80 Billion requests per day।

QPS = 80 Billion ÷ 86,400 = **≈ 926,000 requests per second**। প্রায় 10 লাখ!

---

## Google Search — Full Walkthrough

**Assumptions:**

- Daily users = 1 Billion
- প্রতি user = 10 searches per day
- প্রতিটি query = 2 KB
- প্রতিটি response = 50 KB

**Query storage:**

1 Billion × 10 × 2 KB = 20 Billion KB = **20 TB per day** (incoming data)।

**Response traffic:**

1 Billion × 10 × 50 KB = 500 Billion KB = **500 TB per day** (outgoing data)।

**QPS:**

Total searches = 1 Billion × 10 = 10 Billion per day।

QPS = 10 Billion ÷ 86,400 = **≈ 116,000 searches per second**।

---

## Netflix — Bandwidth Estimation (আলাদা ধরন)

Netflix এর জন্য storage নয়, bandwidth বের করতে হয়।

**Assumptions:**

- Concurrent viewers = 100 Million
- Average bitrate = 5 Mbps per stream

**Total bandwidth:**

100 Million × 5 Mbps = 500 Million Mbps।

1,000 Mbps = 1 Gbps, আর 1,000 Gbps = 1 Tbps।

তাই 500 Million Mbps = 500,000 Gbps = **500 Tbps**।

এটাই Netflix-এর internet traffic — প্রায় পুরো internet-এর একটা বড় অংশ।

---

## Peak Traffic সম্পর্কে একটা কথা

আমরা যা বের করি সেটা **average QPS**। কিন্তু বাস্তবে রাত 10টায় সবাই একসাথে Netflix দেখে, সকালে সবাই WhatsApp চেক করে। Peak time-এ traffic average-এর 2x থেকে 5x পর্যন্ত যেতে পারে।

তাই system design করার সময় সবসময় বলো — "Average QPS হবে X, কিন্তু peak handle করতে 2-3x capacity রাখতে হবে।"

---

## Interview-তে কীভাবে বলবে

এইভাবে শুরু করো:

"Let me start with a quick back-of-the-envelope estimate. I'll assume [X] daily active users, each doing [Y] operations per day with an average payload of [Z]. This gives us [storage] per day. For traffic, dividing total daily requests by 86,400 gives approximately [QPS] average, so we should design for around [2-3× QPS] peak capacity."

এটা interviewer-কে দেখায় যে তুমি scale বোঝো, assumptions clearly state করতে পারো, এবং numbers থেকে architecture decision নিতে পারো।

---

## সব shortcuts এক জায়গায়

Storage এর জন্য: Users × Payload মানে শুধু zeros যোগ করো — Million (6) আর KB (3) যোগ করলে 9 মানে GB, Billion (9) আর KB (3) যোগ করলে 12 মানে TB, এভাবেই বাকিগুলো।

Traffic এর জন্য: Daily total requests কে 86,400 দিয়ে ভাগ করো, বা quick estimate এর জন্য 100,000 দিয়ে ভাগ করো।

এই দুটো rule মাথায় থাকলে যেকোনো system-এর estimation 2 মিনিটে করা সম্ভব।
