# Functional vs Non-Functional Requirements

## আগে একটা real-life example দিয়ে শুরু করি

ধরো তুমি একটা **restaurant** বানাচ্ছো।

**Functional requirements** হলো — রেস্তোরাঁ কী কী করবে। খাবার serve করবে, order নেবে, bill দেবে।

**Non-functional requirements** হলো — রেস্তোরাঁ কেমনভাবে করবে। খাবার কত দ্রুত আসবে, কতজন একসাথে বসতে পারবে, রেস্তোরাঁ কতটা clean থাকবে।

দুটোই দরকার। শুধু "কী করবে" জানলেই হয় না — "কেমনভাবে করবে" সেটাও equally important।

---

## Functional Requirements — সহজ ভাষায়

Functional requirement মানে হলো system **কী কী কাজ করতে পারবে**।

এগুলো হলো system-এর features। User কী করতে পারবে, system কী কী action support করবে — এসবই functional requirement।

সহজে মনে রাখো: **"System কী করবে?"** — এই প্রশ্নের উত্তর হলো functional requirement।

### WhatsApp-এর Functional Requirements

- User account create করতে পারবে
- অন্য user-কে message পাঠাতে পারবে
- Photo, video, file share করতে পারবে
- Group chat create করতে পারবে
- Message delivered হলে tick দেখাবে
- Online/offline status দেখাবে

এগুলোর প্রতিটাই একটা করে feature। এগুলো না থাকলে WhatsApp হয় না।

### YouTube-এর Functional Requirements

- User video upload করতে পারবে
- যেকোনো video search করতে পারবে
- Video play করা যাবে
- Like, dislike, comment করা যাবে
- Channel subscribe করা যাবে
- Recommended videos দেখাবে

---

## Non-Functional Requirements — সহজ ভাষায়

Non-functional requirement মানে হলো system **কেমন হবে, কতটা ভালো হবে**।

এগুলো features না — এগুলো হলো system-এর quality। System কত দ্রুত কাজ করবে, কতজন user একসাথে handle করতে পারবে, system কি কখনো down হবে — এসবই non-functional requirement।

সহজে মনে রাখো: **"System কেমনভাবে করবে?"** — এই প্রশ্নের উত্তর হলো non-functional requirement।

### সবচেয়ে Common Non-Functional Requirements

**Availability** — System কতক্ষণ চালু থাকবে। WhatsApp যদি প্রতিদিন 2 ঘণ্টা বন্ধ থাকে তাহলে কেউ use করবে না। তাই বলা হয় 99.9% বা 99.99% uptime।

**Latency** — Response আসতে কত সময় লাগবে। Google Search-এ search দিলে যদি 10 সেকেন্ড লাগে, কেউ use করবে না। Latency কম হওয়া মানে system fast।

**Scalability** — বেশি user আসলেও system কি handle করতে পারবে? আজকে 1 million user, কালকে 100 million user হলেও কি system ঠিকমতো চলবে?

**Consistency** — Data সবসময় সঠিক থাকবে। তুমি bank-এ 1000 টাকা পাঠালে receiver-এর account-এ 1000 টাকাই যাবে, 999 টাকা না।

**Durability** — Data কি হারিয়ে যাবে না? তুমি WhatsApp-এ photo পাঠালে সেটা কি 5 বছর পরেও থাকবে?

**Throughput** — System প্রতি সেকেন্ডে কতগুলো request handle করতে পারে।

---

## Availability বিস্তারিত — 9-এর হিসাব

Availability সাধারণত "নয়ের সংখ্যা" দিয়ে বলা হয়। এটা system design interview-এ খুব common।

| Availability | বছরে কত সময় down | কী মানে |
|---|---|---|
| 99% (two 9s) | 87 ঘণ্টা | সাধারণ system |
| 99.9% (three 9s) | 8.7 ঘণ্টা | ভালো system |
| 99.99% (four 9s) | 52 মিনিট | বেশিরভাগ big system |
| 99.999% (five 9s) | 5 মিনিট | Bank, hospital level |

WhatsApp, Facebook এরা generally **four 9s** (99.99%) target করে। মানে বছরে মাত্র 52 মিনিট down থাকতে পারবে।

---

## Consistency vs Availability — এখানে একটা Tradeoff আছে

এটা system design-এর সবচেয়ে গুরুত্বপূর্ণ concept।

অনেক সময় Consistency আর Availability দুটো একসাথে 100% রাখা যায় না। একটা বাড়ালে অন্যটা কমে। এটাকে বলে **CAP Theorem**।

**উদাহরণ দিয়ে বোঝাই:**

ধরো WhatsApp-এ তুমি message পাঠালে, সেটা এক সাথে 50টা server-এ store হয়। এখন হঠাৎ একটা server-এর সাথে internet connection গেলো।

দুটো option আছে।

**Option 1 — Consistency choose করো:** সব server sync না হওয়া পর্যন্ত কাউকে message দেখাবে না। এতে data সঠিক থাকবে, কিন্তু system কিছুক্ষণের জন্য কাজ করবে না।

**Option 2 — Availability choose করো:** যে server চালু আছে সেখান থেকে message দেখাও, পরে sync করে নাও। System চালু থাকবে, কিন্তু হয়তো কিছুক্ষণ কেউ পুরনো data দেখবে।

WhatsApp, Facebook এরা **Availability** choose করে। তাই তুমি যদি কখনো Facebook-এ like দাও, সেটা তোমার বন্ধু 2-3 সেকেন্ড পরে দেখে — সাথে সাথে না। এটাকে বলে **Eventual Consistency**।

কিন্তু Bank সবসময় **Consistency** choose করে। তোমার account থেকে টাকা গেলে সেটা immediately সব জায়গায় update হতে হবে। একটু slow হলেও চলবে, কিন্তু wrong data চলবে না।

---

## Latency vs Throughput — আরেকটা Tradeoff

**Latency** মানে একটা request কতক্ষণে complete হয়। যেমন Google Search-এ একটা search করলে result আসতে 200ms লাগলো।

**Throughput** মানে system একসাথে কতগুলো request handle করতে পারে। যেমন Google-এর server প্রতি সেকেন্ডে 1 লাখ search handle করতে পারে।

এই দুটো সাধারণত একসাথে ভালো রাখা কঠিন। Throughput বাড়াতে গেলে latency বাড়তে পারে, কারণ system busy থাকে।

তাই system design-এ সিদ্ধান্ত নিতে হয় — এই system-এর জন্য কোনটা বেশি important?

Video call-এর জন্য low latency বেশি important। Batch data processing-এর জন্য high throughput বেশি important।

---

## দুটোর মধ্যে পার্থক্য এক নজরে

| বিষয় | Functional | Non-Functional |
|---|---|---|
| প্রশ্ন | System কী করবে? | System কেমন হবে? |
| উদাহরণ | User login করতে পারবে | Login 200ms-এর মধ্যে হবে |
| উদাহরণ | Video upload করা যাবে | System 99.99% uptime রাখবে |
| উদাহরণ | Payment করা যাবে | Payment data কখনো হারাবে না |
| Nature | Features | Quality / Performance |
| Without it | System কাজ করে না | System poorly কাজ করে |

---

## Interview-তে কীভাবে বলবে

System design interview-এ সবার আগে requirements clarify করতে হয়। এইভাবে বলো:

"আমি আগে functional requirements define করি — system কী কী করবে। তারপর non-functional requirements — system কতটা available হবে, latency কত হবে, কতজন user support করবে। এই দুটো clear হলে architecture design শুরু করতে পারবো।"

এতে interviewer বুঝবে তুমি systematically চিন্তা করতে পারো।

---

## এক কথায় মনে রাখো

**Functional** = System-এর **কাজের তালিকা**। Feature গুলো কী কী।

**Non-Functional** = System-এর **মানের মাপকাঠি**। কত দ্রুত, কত stable, কত reliable।

দুটো ছাড়া কোনো system complete না।
