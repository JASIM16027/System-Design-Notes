# REST API - Complete Notes with Diagrams

## REST API - Basics
68. What is REST & RESTful API?
69. What are HTTP Request and Response structures in UI and REST API?
70. What are Top 5 REST guidelines and the advantages of them?
71. What is the difference between REST API and SOAP API?

## REST API - HTTP Methods & Status Codes
72. What are HTTP Verbs and HTTP methods?
73. What are GET, POST, PUT & DELETE HTTP methods?
74. What is the difference between PUT & PATCH methods?
75. Explain the concept of Idempotence in RESTful APIs.
76. What are the role of status codes in RESTful APIs?
    

---

## Definition

```
REST API:
- যেকোনো API যা REST এর কিছু principles follow করে
- Partially REST compliant
- "REST-inspired" or "REST-like"

RESTful API:
- যে API REST এর সব 6টি constraints পুরোপুরি মেনে চলে
- Fully REST compliant
- True REST implementation
```

---

## Key Differences

### 1. URL Design

```
REST API (❌ Improper):
/getUsers
/createUser
/updateUser?id=123
/deleteUser?id=456
/getUserById?id=123

RESTful API (✅ Proper):
GET    /users
POST   /users
PUT    /users/123
DELETE /users/456
GET    /users/123
```

### 2. HTTP Methods

```
REST API (❌):
POST /getAllUsers      (should be GET)
GET  /deleteUser       (should be DELETE)
POST /updateProduct    (should be PUT/PATCH)

RESTful API (✅):
GET    /users          (read)
POST   /users          (create)
PUT    /users/123      (replace)
PATCH  /users/123      (update)
DELETE /users/123      (delete)
```

### 3. Status Codes

```
REST API (❌):
All responses: 200 OK
Even errors return 200:
{
  "status": "error",
  "message": "Not found"
}

RESTful API (✅):
200 OK         - Success
201 Created    - Resource created
204 No Content - Deleted successfully
400 Bad Request- Invalid input
401 Unauthorized - Auth required
404 Not Found  - Resource missing
500 Server Error - Server problem
```

### 4. Response Structure

```
REST API (❌ Inconsistent):
Endpoint 1: {"user": {...}}
Endpoint 2: {"data": {...}}
Endpoint 3: {"result": {...}}
Different structures!

RESTful API (✅ Consistent):
All endpoints: Same structure
{
  "id": 123,
  "name": "Value",
  ...
}
```

### 5. Statelessness

```
REST API (❌ Stateful):
POST /login → Server stores session
GET /profile → No auth needed (server remembers)

RESTful API (✅ Stateless):
POST /login → Returns token
GET /profile
Authorization: Bearer token123
(Every request complete!)
```

---

## Comparison Table

```
┌──────────────────┬─────────────┬──────────────┐
│ Feature          │ REST API    │ RESTful API  │
├──────────────────┼─────────────┼──────────────┤
│ URL Style        │ Actions in  │ Resources    │
│                  │ URL         │ only         │
├──────────────────┼─────────────┼──────────────┤
│ HTTP Methods     │ Misused     │ Correct      │
├──────────────────┼─────────────┼──────────────┤
│ Status Codes     │ Always 200  │ Appropriate  │
├──────────────────┼─────────────┼──────────────┤
│ Statelessness    │ May be      │ Always       │
│                  │ stateful    │ stateless    │
├──────────────────┼─────────────┼──────────────┤
│ Consistency      │ Varies      │ Uniform      │
├──────────────────┼─────────────┼──────────────┤
│ Caching          │ Rarely      │ Fully        │
│                  │ implemented │ supported    │
├──────────────────┼─────────────┼──────────────┤
│ HATEOAS          │ No          │ Yes          │
├──────────────────┼─────────────┼──────────────┤
│ REST Compliance  │ 30-70%      │ 100%         │
└──────────────────┴─────────────┴──────────────┘
```

---

## Real Examples

### REST API Example (❌):

```
POST /api/getUser
{
  "userId": 123
}

Response: 200 OK
{
  "status": "success",
  "userData": {
    "userId": 123,
    "userName": "Rahim"
  }
}
```

### RESTful API Example (✅):

```
GET /api/users/123
Authorization: Bearer token123

Response: 200 OK
{
  "id": 123,
  "name": "Rahim",
  "email": "rahim@email.com",
  "_links": {
    "self": "/users/123",
    "orders": "/users/123/orders"
  }
}
```

---

## Summary

**REST API:**
- আংশিক REST মানে
- কিছু ভালো practices আছে
- সম্পূর্ণ নয়

**RESTful API:**
- সম্পূর্ণ REST মানে
- সব constraints follow করে
- True REST implementation

**মনে রাখার উপায়:**
REST API = REST-ish (REST এর মতো)
RESTful API = Pure REST (খাঁটি REST) ✓



---

## 1. Resource (রিসোর্স)

### Definition
```
Resource = যেকোনো জিনিস/তথ্য যার একটা unique identifier (URI) আছে
```

### Diagram
```
┌─────────────────────────────────────────────────────────┐
│                    RESOURCE                             │
└─────────────────────────────────────────────────────────┘

Real World:                    Digital World:
┌──────────┐                   ┌──────────────────┐
│  লাইব্রেরি │                   │   E-commerce     │
├──────────┤                   ├──────────────────┤
│ • Books  │                   │ • Products       │
│ • Members│ ────────────────► │ • Users          │
│ • Records│                   │ • Orders         │
└──────────┘                   │ • Reviews        │
                               └──────────────────┘

Resource Types:

1. Individual Resource (একক):
   /users/123          → একজন নির্দিষ্ট user
   /products/iphone-15 → একটা নির্দিষ্ট product
   /orders/ORD-001     → একটা নির্দিষ্ট order

2. Collection Resource (সমষ্টি):
   /users              → সব users
   /products           → সব products
   /orders             → সব orders

3. Sub-Resource (উপ-রিসোর্স):
   /users/123/orders           → User 123 এর orders
   /products/iphone-15/reviews → iPhone 15 এর reviews
   /posts/456/comments         → Post 456 এর comments
```

### Resource Example
```
Facebook Resources:

/users/rahim_ahmed              → User profile
/users/rahim_ahmed/posts        → User's posts
/users/rahim_ahmed/friends      → User's friends
/posts/123456789                → Specific post
/posts/123456789/comments       → Post's comments
/posts/123456789/likes          → Post's likes
/photos/pic_abc123              → Photo
```

---

## 2. State (স্টেট)

### Definition
```
State = Resource এর বর্তমান অবস্থা/তথ্য/data
```

### Diagram
```
┌─────────────────────────────────────────────────────────┐
│                    STATE                                │
└─────────────────────────────────────────────────────────┘

Resource: User #123

Current State (বর্তমান অবস্থা):
┌────────────────────────────────────┐
│ {                                  │
│   "id": 123,                       │
│   "name": "Rahim Ahmed",           │
│   "email": "rahim@example.com",    │
│   "status": "active",              │
│   "balance": 5000,                 │
│   "last_login": "2026-02-07",      │
│   "friends_count": 532             │
│ }                                  │
└────────────────────────────────────┘
      │
      │ User actions cause state changes
      ▼

State Transitions (অবস্থা পরিবর্তন):

Action: Login
┌────────────────────────────────────┐
│ "last_login": "2026-02-07 10:30"   │ ← Changed
└────────────────────────────────────┘

Action: Add Friend
┌────────────────────────────────────┐
│ "friends_count": 533               │ ← Changed
└────────────────────────────────────┘

Action: Payment
┌────────────────────────────────────┐
│ "balance": 4500                    │ ← Changed
└────────────────────────────────────┘
```

### State Example - Order Lifecycle
```
Order State Transitions:

State 1: Created
┌──────────────────────────────────────┐
│ {                                    │
│   "order_id": "ORD-001",             │
│   "status": "pending",               │
│   "payment_status": "unpaid",        │
│   "total": 5000                      │
│ }                                    │
└──────────────────────────────────────┘
          │
          │ Payment করলে
          ▼
State 2: Paid
┌──────────────────────────────────────┐
│ {                                    │
│   "order_id": "ORD-001",             │
│   "status": "confirmed",             │ ← Changed
│   "payment_status": "paid",          │ ← Changed
│   "total": 5000                      │
│ }                                    │
└──────────────────────────────────────┘
          │
          │ Shipping শুরু হলে
          ▼
State 3: Shipped
┌──────────────────────────────────────┐
│ {                                    │
│   "order_id": "ORD-001",             │
│   "status": "shipped",               │ ← Changed
│   "tracking": "TRK-123",             │ ← New
│   "payment_status": "paid",          │
│   "total": 5000                      │
│ }                                    │
└──────────────────────────────────────┘
```

---

## 3. Representation (রিপ্রেজেন্টেশন)

### Definition
```
Representation = State কে কোন format/structure এ প্রকাশ করা
একই state কে বিভিন্ন format এ দেখানো যায়
```

### Diagram
```
┌─────────────────────────────────────────────────────────┐
│               REPRESENTATION                            │
└─────────────────────────────────────────────────────────┘

        Resource: User #123
              │
              │ Has State
              ▼
    ┌─────────────────────┐
    │ id: 123             │
    │ name: Rahim Ahmed   │
    │ email: rahim@...    │
    │ age: 28             │
    └─────────┬───────────┘
              │
              │ Same State,
              │ Different Representations
              │
    ┌─────────┴─────────┬─────────────┬──────────────┐
    │                   │             │              │
    ▼                   ▼             ▼              ▼

JSON Format          XML Format    HTML Format    Plain Text
┌──────────────┐  ┌───────────┐  ┌──────────┐  ┌────────────┐
│ {            │  │ <user>    │  │ <html>   │  │ User: 123  │
│  "id": 123,  │  │  <id>123  │  │  <div>   │  │ Name: Rahim│
│  "name":     │  │  </id>    │  │   <h1>   │  │ Email: ... │
│  "Rahim"     │  │  <name>   │  │   Rahim  │  │ Age: 28    │
│ }            │  │   Rahim   │  │   </h1>  │  └────────────┘
└──────────────┘  │  </name>  │  │  </div>  │
                  │ </user>   │  │ </html>  │
                  └───────────┘  └──────────┘

       ▲                ▲            ▲             ▲
       │                │            │             │
       └────────────────┴────────────┴─────────────┘
              Same Data, Different Formats
```

### Content Negotiation
```
Client বলে কোন format চায়:

Request 1: JSON চাই
┌────────────────────────────────┐
│ GET /users/123                 │
│ Accept: application/json       │
└────────────────────────────────┘
         │
         ▼
Response: JSON
┌────────────────────────────────┐
│ {                              │
│   "id": 123,                   │
│   "name": "Rahim Ahmed"        │
│ }                              │
└────────────────────────────────┘

Request 2: XML চাই
┌────────────────────────────────┐
│ GET /users/123                 │
│ Accept: application/xml        │
└────────────────────────────────┘
         │
         ▼
Response: XML
┌────────────────────────────────┐
│ <user>                         │
│   <id>123</id>                 │
│   <name>Rahim Ahmed</name>     │
│ </user>                        │
└────────────────────────────────┘
```

---

## 4. Transfer (ট্রান্সফার)

### Definition
```
Transfer = Representation কে Client এবং Server এর মধ্যে পাঠানো
HTTP Protocol ব্যবহার করে data আদান-প্রদান
```

### Diagram
```
┌─────────────────────────────────────────────────────────┐
│                    TRANSFER                             │
└─────────────────────────────────────────────────────────┘

CLIENT                                        SERVER
┌──────────┐                                ┌──────────┐
│ Browser  │                                │   Web    │
│  Mobile  │                                │  Server  │
│  App     │                                │          │
└─────┬────┘                                └─────┬────┘
      │                                           │
      │ 1. HTTP Request পাঠাও                   │
      │    (Representation পাঠাচ্ছি)             │
      │                                           │
      │  ┌─────────────────────────────────┐     │
      │  │ GET /users/123 HTTP/1.1         │     │
      │  │ Host: api.example.com           │     │
      │  │ Accept: application/json        │     │
      │  │ Authorization: Bearer token123  │     │
      │  └─────────────────────────────────┘     │
      ├──────────────────────────────────────────>│
      │                                           │
      │                                    ┌──────▼──────┐
      │                                    │  Database   │
      │                                    │  থেকে State │
      │                                    │  নিলো       │
      │                                    └──────┬──────┘
      │                                           │
      │                                    ┌──────▼──────┐
      │                                    │ JSON এ      │
      │                                    │ Convert     │
      │                                    │ করলো       │
      │                                    └──────┬──────┘
      │                                           │
      │ 2. HTTP Response পেলাম                  │
      │    (Representation পাচ্ছি)               │
      │                                           │
      │  ┌─────────────────────────────────┐     │
      │  │ HTTP/1.1 200 OK                 │     │
      │  │ Content-Type: application/json  │     │
      │  │                                 │     │
      │  │ {                               │     │
      │  │   "id": 123,                    │     │
      │  │   "name": "Rahim Ahmed"         │     │
      │  │ }                               │     │
      │  └─────────────────────────────────┘     │
      │<──────────────────────────────────────────┤
      │                                           │
┌─────▼─────┐                                    │
│ UI তে     │                                    │
│ Display   │                                    │
└───────────┘                                    │
```

### Transfer Protocol Details
```
HTTP Request Structure:
┌────────────────────────────────────────┐
│ Request Line:                          │
│   METHOD /path HTTP/version            │
│                                        │
│ Headers:                               │
│   Key: Value                           │
│   Key: Value                           │
│                                        │
│ Body: (optional)                       │
│   {data}                               │
└────────────────────────────────────────┘

HTTP Response Structure:
┌────────────────────────────────────────┐
│ Status Line:                           │
│   HTTP/version STATUS_CODE Message     │
│                                        │
│ Headers:                               │
│   Key: Value                           │
│   Key: Value                           │
│                                        │
│ Body:                                  │
│   {data}                               │
└────────────────────────────────────────┘
```

---

## 5. REST = Representational State Transfer

### Definition
```
REST = Resource এর State কে Representation আকারে Transfer করা

Representational → কীভাবে দেখানো (JSON, XML, HTML)
State → বর্তমান তথ্য (Database data)
Transfer → পাঠানো (HTTP এর মাধ্যমে)
```

### Complete Flow Diagram
```
┌─────────────────────────────────────────────────────────┐
│        REPRESENTATIONAL STATE TRANSFER (REST)           │
└─────────────────────────────────────────────────────────┘

Step 1: Client Request করে
┌──────────────────────────────────────────────────────┐
│ CLIENT: "আমাকে User 123 এর তথ্য দাও"               │
│                                                      │
│ GET /api/users/123                                   │
│ Accept: application/json                             │
└───────────────────┬──────────────────────────────────┘
                    │
                    │ HTTP Request
                    ▼
┌──────────────────────────────────────────────────────┐
│ SERVER: Request পেলো                                │
└───────────────────┬──────────────────────────────────┘
                    │
Step 2: Resource Identify করে
                    │
                    ▼
┌──────────────────────────────────────────────────────┐
│ Resource: User                                       │
│ Identifier: 123                                      │
└───────────────────┬──────────────────────────────────┘
                    │
Step 3: State fetch করে
                    │
                    ▼
┌──────────────────────────────────────────────────────┐
│ DATABASE Query:                                      │
│ SELECT * FROM users WHERE id = 123                   │
│                                                      │
│ Result (Raw State):                                  │
│ id: 123                                              │
│ name: Rahim Ahmed                                    │
│ email: rahim@example.com                             │
│ age: 28                                              │
│ created_at: 2020-05-15                               │
└───────────────────┬──────────────────────────────────┘
                    │
Step 4: Representation তৈরি করে
                    │
                    ▼
┌──────────────────────────────────────────────────────┐
│ JSON Representation:                                 │
│ {                                                    │
│   "id": 123,                                         │
│   "name": "Rahim Ahmed",                             │
│   "email": "rahim@example.com",                      │
│   "age": 28,                                         │
│   "accountCreated": "2020-05-15"                     │
│ }                                                    │
└───────────────────┬──────────────────────────────────┘
                    │
Step 5: Transfer করে
                    │
                    │ HTTP Response
                    ▼
┌──────────────────────────────────────────────────────┐
│ HTTP/1.1 200 OK                                      │
│ Content-Type: application/json                       │
│                                                      │
│ {                                                    │
│   "id": 123,                                         │
│   "name": "Rahim Ahmed",                             │
│   "email": "rahim@example.com",                      │
│   "age": 28                                          │
│ }                                                    │
└───────────────────┬──────────────────────────────────┘
                    │
                    ▼
┌──────────────────────────────────────────────────────┐
│ CLIENT: Data পেয়ে UI তে display করলো               │
└──────────────────────────────────────────────────────┘

Summary:
Resource (User 123) → State (Database data) → 
Representation (JSON) → Transfer (HTTP) → Client
```

---

## 6. RESTful API

### Definition
```
RESTful API = যে API REST এর সব principles/constraints মেনে চলে
```

### 6 RESTful Constraints
```
┌─────────────────────────────────────────────────────────┐
│             6 RESTful CONSTRAINTS                       │
└─────────────────────────────────────────────────────────┘

1. CLIENT-SERVER ARCHITECTURE
   ════════════════════════════
   Definition: Client ও Server আলাদা, independent
   
   ┌──────────┐              ┌──────────┐
   │  CLIENT  │              │  SERVER  │
   │          │              │          │
   │ • UI     │◄────────────►│ • Logic  │
   │ • Display│ Independent  │ • Database│
   │          │              │ • Process│
   └──────────┘              └──────────┘
   
   সুবিধা:
   - Mobile app ও Web app same API use করতে পারে
   - Server change করলে client change লাগবে না


2. STATELESS
   ═══════════
   Definition: প্রতিটা request এ সব info থাকতে হবে
               Server আগের request মনে রাখবে না
   
   Request 1:
   ┌────────────────────────────────┐
   │ GET /api/profile               │
   │ Authorization: Bearer token123 │ ← Token পাঠাতে হবে
   └────────────────────────────────┘
   
   Request 2:
   ┌────────────────────────────────┐
   │ GET /api/orders                │
   │ Authorization: Bearer token123 │ ← আবার token পাঠাতে হবে
   └────────────────────────────────┘
   
   ❌ Stateful (ভুল):
   Server session store করে রাখবে
   
   ✅ Stateless (সঠিক):
   প্রতিটা request complete & self-contained
   
   সুবিধা:
   - Scaling সহজ
   - Any server যেকোনো request handle করতে পারে


3. CACHEABLE
   ══════════
   Definition: Response cache করা যাবে কিনা specify করতে হবে
   
   ┌─────────┐  Request  ┌───────┐  Query  ┌────────┐
   │ Client  │──────────►│ Cache │────────►│ Server │
   └─────────┘           └───┬───┘         └────────┘
       ▲                     │
       │   Cached Response   │
       └─────────────────────┘
   
   Response Headers:
   ┌────────────────────────────────┐
   │ Cache-Control: max-age=3600    │ ← 1 hour cache
   │ ETag: "abc123"                 │ ← Version ID
   └────────────────────────────────┘
   
   সুবিধা:
   - Performance বাড়ে
   - Network load কমে
   - Server load কমে


4. UNIFORM INTERFACE
   ══════════════════
   Definition: Consistent, standard way তে interact করা
   
   a) Resource-based URLs:
   ✅ /users/123
   ✅ /products/iphone-15
   ❌ /getUser?id=123
   ❌ /getUserProducts
   
   b) HTTP Methods সঠিকভাবে:
   GET    /users     → Read all
   GET    /users/123 → Read one
   POST   /users     → Create
   PUT    /users/123 → Replace
   PATCH  /users/123 → Update
   DELETE /users/123 → Delete
   
   c) Self-descriptive Messages:
   ┌────────────────────────────────┐
   │ Content-Type: application/json │
   │ Content-Length: 234            │
   │ Authorization: Bearer token    │
   └────────────────────────────────┘
   
   d) HATEOAS (Links):
   {
     "order_id": "ORD-001",
     "status": "shipped",
     "_links": {
       "self": "/orders/ORD-001",
       "track": "/orders/ORD-001/tracking",
       "cancel": "/orders/ORD-001/cancel"
     }
   }


5. LAYERED SYSTEM
   ═══════════════
   Definition: Client জানবে না মাঝে কয়টা layer আছে
   
   Client
     │
     ▼
   ┌─────────────────┐
   │ Load Balancer   │ Layer 1
   └────────┬────────┘
            ▼
   ┌─────────────────┐
   │ API Gateway     │ Layer 2
   └────────┬────────┘
            ▼
   ┌─────────────────┐
   │ Authentication  │ Layer 3
   └────────┬────────┘
            ▼
   ┌─────────────────┐
   │ App Server      │ Layer 4
   └────────┬────────┘
            ▼
   ┌─────────────────┐
   │ Database        │ Layer 5
   └─────────────────┘
   
   Client শুধু API call করবে,
   মাঝে কী আছে জানতে হবে না
   
   সুবিধা:
   - Security layers add করা যায়
   - Caching layers add করা যায়
   - Client unchanged থাকে


6. CODE ON DEMAND (Optional)
   ═════════════════════════
   Definition: Server executable code পাঠাতে পারে
   
   Response:
   {
     "form_data": {...},
     "validation_script": "function validate() {...}"
   }
   
   Note: এটা optional, বেশিরভাগ REST API তে নেই
```

---

## 7. HTTP Methods (CRUD Operations)

### Definition
```
HTTP Methods = REST API তে কোন action করতে চাই সেটা বলার উপায়
CRUD = Create, Read, Update, Delete
```

### Diagram
```
┌─────────────────────────────────────────────────────────┐
│              HTTP METHODS & CRUD                        │
└─────────────────────────────────────────────────────────┘

┌──────────┬──────────┬────────────┬──────────────────────┐
│ HTTP     │ CRUD     │ Action     │ Example              │
│ Method   │ Operation│            │                      │
├──────────┼──────────┼────────────┼──────────────────────┤
│ GET      │ READ     │ পড়া       │ GET /users/123       │
│          │          │            │ → User 123 দেখাও    │
├──────────┼──────────┼────────────┼──────────────────────┤
│ POST     │ CREATE   │ তৈরি করা  │ POST /users          │
│          │          │            │ → নতুন user তৈরি    │
├──────────┼──────────┼────────────┼──────────────────────┤
│ PUT      │ UPDATE   │ পুরো       │ PUT /users/123       │
│          │ (Replace)│ replace    │ → User 123 replace   │
├──────────┼──────────┼────────────┼──────────────────────┤
│ PATCH    │ UPDATE   │ আংশিক     │ PATCH /users/123     │
│          │ (Partial)│ update     │ → শুধু name update   │
├──────────┼──────────┼────────────┼──────────────────────┤
│ DELETE   │ DELETE   │ মুছে ফেলা │ DELETE /users/123    │
│          │          │            │ → User 123 delete    │
└──────────┴──────────┴────────────┴──────────────────────┘

Complete CRUD Flow:
═══════════════════

1. CREATE (POST)
   ═════════════
   Client                           Server
     │                                │
     │  POST /api/users               │
     │  {                             │
     │    "name": "Rahim",            │
     │    "email": "rahim@ex.com"     │
     │  }                             │
     ├───────────────────────────────>│
     │                                │
     │                         ┌──────▼──────┐
     │                         │ INSERT INTO │
     │                         │ users ...   │
     │                         └──────┬──────┘
     │                                │
     │  201 Created                   │
     │  Location: /api/users/123      │
     │  {                             │
     │    "id": 123,                  │
     │    "name": "Rahim",            │
     │    "created_at": "..."         │
     │  }                             │
     │<───────────────────────────────┤


2. READ (GET)
   ══════════
   Client                           Server
     │                                │
     │  GET /api/users/123            │
     ├───────────────────────────────>│
     │                                │
     │                         ┌──────▼──────┐
     │                         │ SELECT *    │
     │                         │ FROM users  │
     │                         │ WHERE id=123│
     │                         └──────┬──────┘
     │                                │
     │  200 OK                        │
     │  {                             │
     │    "id": 123,                  │
     │    "name": "Rahim",            │
     │    "email": "rahim@ex.com"     │
     │  }                             │
     │<───────────────────────────────┤


3. UPDATE (PATCH)
   ═══════════════
   Client                           Server
     │                                │
     │  PATCH /api/users/123          │
     │  {                             │
     │    "name": "Rahim Ahmed Khan"  │
     │  }                             │
     ├───────────────────────────────>│
     │                                │
     │                         ┌──────▼──────┐
     │                         │ UPDATE users│
     │                         │ SET name=.. │
     │                         │ WHERE id=123│
     │                         └──────┬──────┘
     │                                │
     │  200 OK                        │
     │  {                             │
     │    "id": 123,                  │
     │    "name": "Rahim Ahmed Khan", │
     │    "updated_at": "..."         │
     │  }                             │
     │<───────────────────────────────┤


4. DELETE
   ═══════
   Client                           Server
     │                                │
     │  DELETE /api/users/123         │
     ├───────────────────────────────>│
     │                                │
     │                         ┌──────▼──────┐
     │                         │ DELETE FROM │
     │                         │ users WHERE │
     │                         │ id=123      │
     │                         └──────┬──────┘
     │                                │
     │  204 No Content                │
     │  (no body)                     │
     │<───────────────────────────────┤
```

---

## 8. HTTP Status Codes

### Definition
```
Status Code = Server এর response এর অবস্থা বলে
Client বুঝতে পারে request সফল হয়েছে কিনা
```

### Diagram
```
┌─────────────────────────────────────────────────────────┐
│              HTTP STATUS CODES                          │
└─────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────┐
│ 2xx - SUCCESS (সফল)                                    │
├─────┬────────────────────┬───────────────────────────────┤
│ 200 │ OK                 │ সফল, data আছে                │
│ 201 │ Created            │ নতুন resource তৈরি হয়েছে    │
│ 204 │ No Content         │ সফল, কিন্তু body নেই         │
└─────┴────────────────────┴───────────────────────────────┘

Example:
GET /users/123
  ↓
200 OK
{"id": 123, "name": "Rahim"}

POST /users
  ↓
201 Created
Location: /users/124
{"id": 124, ...}

DELETE /users/123
  ↓
204 No Content
(empty body)


┌──────────────────────────────────────────────────────────┐
│ 4xx - CLIENT ERROR (Client এর ভুল)                     │
├─────┬────────────────────┬───────────────────────────────┤
│ 400 │ Bad Request        │ Invalid data পাঠিয়েছে        │
│ 401 │ Unauthorized       │ Authentication লাগবে          │
│ 403 │ Forbidden          │ Permission নেই                │
│ 404 │ Not Found          │ Resource পাওয়া যায়নি        │
│ 409 │ Conflict           │ Duplicate/Conflict            │
└─────┴────────────────────┴───────────────────────────────┘

Example:
GET /users/99999
  ↓
404 Not Found
{"error": "User not found"}

POST /users (without token)
  ↓
401 Unauthorized
{"error": "Authentication required"}

POST /users (invalid email)
  ↓
400 Bad Request
{"error": "Invalid email format"}


┌──────────────────────────────────────────────────────────┐
│ 5xx - SERVER ERROR (Server এ সমস্যা)                    │
├─────┬────────────────────┬───────────────────────────────┤
│ 500 │ Internal Server    │ Server এ error হয়েছে         │
│     │ Error              │                               │
│ 502 │ Bad Gateway        │ Gateway error                 │
│ 503 │ Service Unavailable│ Server down/overloaded        │
└─────┴────────────────────┴───────────────────────────────┘

Example:
GET /users/123
  ↓
500 Internal Server Error
{"error": "Database connection failed"}

Visual Flow:
═══════════

Request → Server Processing → Response

         ┌─ 2xx Success
         │  └─ Everything OK
         │
         ├─ 4xx Client Error
Status ──┤  └─ Client কিছু ভুল করেছে
Code     │
         └─ 5xx Server Error
            └─ Server এ problem
```

---

## 9. Complete Request-Response Example

### Definition
```
HTTP Request-Response = Client ও Server এর মধ্যে communication
Request = Client থেকে Server এ
Response = Server থেকে Client এ
```

### Diagram
```
┌─────────────────────────────────────────────────────────┐
│         COMPLETE REQUEST-RESPONSE CYCLE                 │
└─────────────────────────────────────────────────────────┘

CLIENT                                          SERVER
  │                                               │
  │ ┌───────────────────────────────────────┐    │
  │ │       HTTP REQUEST                    │    │
  │ ├───────────────────────────────────────┤    │
  │ │ Request Line:                         │    │
  │ │   POST /api/orders HTTP/1.1           │    │
  │ │                                       │    │
  │ │ Headers:                              │    │
  │ │   Host: api.shop.com                  │    │
  │ │   Content-Type: application/json      │    │
  │ │   Authorization: Bearer eyJhbGc...    │    │
  │ │   Accept: application/json            │    │
  │ │   User-Agent: Mobile App 1.0          │    │
  │ │                                       │    │
  │ │ Body:                                 │    │
  │ │   {                                   │    │
  │ │     "product_id": 101,                │    │
  │ │     "quantity": 2,                    │    │
  │ │     "total": 10000                    │    │
  │ │   }                                   │    │
  │ └───────────────────────────────────────┘    │
  ├─────────────────────────────────────────────>│
  │                                               │
  │                                        ┌──────▼──────┐
  │                                        │ 1. Validate │
  │                                        │    Request  │
  │                                        └──────┬──────┘
  │                                               │
  │                                        ┌──────▼──────┐
  │                                        │ 2. Check    │
  │                                        │    Auth     │
  │                                        └──────┬──────┘
  │                                               │
  │                                        ┌──────▼──────┐
  │                                        │ 3. Check    │
  │                                        │    Stock    │
  │                                        └──────┬──────┘
  │                                               │
  │                                        ┌──────▼──────┐
  │                                        │ 4. Create   │
  │                                        │    Order    │
  │                                        └──────┬──────┘
  │                                               │
  │                                        ┌──────▼──────┐
  │                                        │ 5. Format   │
  │                                        │    Response │
  │                                        └──────┬──────┘
  │                                               │
  │ ┌───────────────────────────────────────┐    │
  │ │       HTTP RESPONSE                   │    │
  │ ├───────────────────────────────────────┤    │
  │ │ Status Line:                          │    │
  │ │   HTTP/1.1 201 Created                │    │
  │ │                                       │    │
  │ │ Headers:                              │    │
  │ │   Content-Type: application/json      │    │
  │ │   Content-Length: 345                 │    │
  │ │   Location: /api/orders/ORD-001       │    │
  │ │   Date: Sat, 07 Feb 2026 10:30:00 GMT │    │
  │ │                                       │    │
  │ │ Body:                                 │    │
  │ │   {                                   │    │
  │ │     "order_id": "ORD-001",            │    │
  │ │     "status": "pending",              │    │
  │ │     "product_id": 101,                │    │
  │ │     "quantity": 2,                    │    │
  │ │     "total": 10000,                   │    │
  │ │     "created_at": "2026-02-07T10:30"  │    │
  │ │   }                                   │    │
  │ └───────────────────────────────────────┘    │
  │<─────────────────────────────────────────────┤
  │                                               │
  ▼                                               ▼

Component Breakdown:
═══════════════════

REQUEST:
┌────────────────────────────┐
│ Method: POST               │ ← কী করতে চাই
│ Path: /api/orders          │ ← কোথায় যেতে চাই
│ Headers: {...}             │ ← Extra information
│ Body: {...}                │ ← যে data পাঠাচ্ছি
└────────────────────────────┘

RESPONSE:
┌────────────────────────────┐
│ Status: 201 Created        │ ← কী হয়েছে
│ Headers: {...}             │ ← Response info
│ Body: {...}                │ ← যে data পাচ্ছি
└────────────────────────────┘
```

---

## 10. URL Structure & Best Practices

### Definition
```
URL = Uniform Resource Locator
REST API তে URL হলো resource এর address
```

### Diagram
```
┌─────────────────────────────────────────────────────────┐
│              URL STRUCTURE                              │
└─────────────────────────────────────────────────────────┘

Complete URL Breakdown:
═══════════════════════

https://api.shop.com:443/v1/users/123/orders?status=shipped&page=1#top
│      │   │        │   │   │  │     │   │    │                    │  │
│      │   │        │   │   │  │     │   │    │                    │  └─ Fragment
│      │   │        │   │   │  │     │   │    └─ Query Parameters  │
│      │   │        │   │   │  │     │   └─ Sub-resource          │
│      │   │        │   │   │  │     └─ Resource ID               │
│      │   │        │   │   │  └─ Resource                        │
│      │   │        │   │   └─ Version                            │
│      │   │        │   └─ API Base Path                          │
│      │   │        └─ Port (optional)                            │
│      │   └─ Domain                                              │
│      └─ Subdomain                                               │
└─ Protocol                                                        │

┌────────────────────────────────────────────────────┐
│ Protocol:      https                               │
│ Subdomain:     api                                 │
│ Domain:        shop.com                            │
│ Port:          443 (default for https)             │
│ Base Path:     /v1                                 │
│ Resource:      users                               │
│ Resource ID:   123                                 │
│ Sub-resource:  orders                              │
│ Query Params:  status=shipped&page=1               │
│ Fragment:      top                                 │
└────────────────────────────────────────────────────┘


REST URL Best Practices:
════════════════════════

✅ CORRECT (সঠিক):
┌─────────────────────────────────────┐
│ /users                  (Collection)│
│ /users/123              (Individual)│
│ /users/123/orders       (Related)   │
│ /products               (Plural)    │
│ /product-categories     (Hyphen)    │
│ /api/v1/users           (Versioned) │
└─────────────────────────────────────┘

❌ INCORRECT (ভুল):
┌─────────────────────────────────────┐
│ /getUsers              (Verb)       │
│ /user                  (Singular)   │
│ /user/123/getOrders    (Verb)       │
│ /product_categories    (Underscore) │
│ /Users/123             (Uppercase)  │
│ /api/users             (No version) │
└─────────────────────────────────────┘

Resource Hierarchy:
═══════════════════

              /api/v1
                 │
      ┌──────────┼──────────┐
      │          │          │
   /users    /products   /orders
      │          │          │
   /users/     /products/  /orders/
    {id}        {id}        {id}
      │          │          │
   ┌──┴──┐    ┌──┴──┐    ┌──┴──┐
   │     │    │     │    │     │
/orders /posts /reviews /images /items /tracking

Example Paths:
═════════════

Individual Resource:
/users/123
/products/iphone-15
/orders/ORD-2026-001

Collection Resource:
/users
/products
/orders

Sub-resource:
/users/123/orders
/products/iphone-15/reviews
/orders/ORD-001/items

Filtering:
/products?category=phones
/products?minPrice=5000&maxPrice=10000
/users?status=active&role=admin

Sorting:
/products?sort=price_asc
/products?sort=created_at_desc

Pagination:
/products?page=2&limit=20
/products?offset=40&limit=20

Search:
/products?search=iphone
/users?q=rahim
```

---

## 11. REST API vs SOAP API

### Definition
```
REST = Architectural Style (নিয়ম-কানুন)
SOAP = Protocol (কঠোর standard)
```

### Diagram
```
┌─────────────────────────────────────────────────────────┐
│              REST vs SOAP                               │
└─────────────────────────────────────────────────────────┘

REST API                          SOAP API
┌──────────────────┐              ┌──────────────────┐
│ Lightweight      │              │ Heavyweight      │
│                  │              │                  │
│ ┌──────────┐     │              │ ┌──────────┐     │
│ │   JSON   │     │              │ │   XML    │     │
│ │  Simple  │     │              │ │ Complex  │     │
│ └──────────┘     │              │ └──────────┘     │
│                  │              │                  │
│ Multiple Formats │              │ Only XML         │
│ JSON, XML, HTML  │              │                  │
└──────────────────┘              └──────────────────┘

Message Structure:
═════════════════

REST Request:                 SOAP Request:
┌────────────────────┐        ┌─────────────────────────┐
│ GET /users/123     │        │ <?xml version="1.0"?>   │
│ Accept: JSON       │        │ <soap:Envelope>         │
│                    │        │   <soap:Header>         │
│                    │        │     ...                 │
│                    │        │   </soap:Header>        │
│                    │        │   <soap:Body>           │
│                    │        │     <GetUser>           │
│                    │        │       <id>123</id>      │
│                    │        │     </GetUser>          │
│                    │        │   </soap:Body>          │
│                    │        │ </soap:Envelope>        │
└────────────────────┘        └─────────────────────────┘
    Simple ✓                      Complex ✗

REST Response:                SOAP Response:
┌────────────────────┐        ┌─────────────────────────┐
│ 200 OK             │        │ <?xml version="1.0"?>   │
│ {                  │        │ <soap:Envelope>         │
│   "id": 123,       │        │   <soap:Body>           │
│   "name": "Rahim"  │        │     <GetUserResponse>   │
│ }                  │        │       <user>            │
│                    │        │         <id>123</id>    │
│                    │        │         <name>Rahim</..>│
│                    │        │       </user>           │
│                    │        │     </GetUserResponse>  │
│                    │        │   </soap:Body>          │
│                    │        │ </soap:Envelope>        │
└────────────────────┘        └─────────────────────────┘

Comparison Table:
════════════════

┌──────────────┬────────────────┬─────────────────┐
│ Feature      │ REST           │ SOAP            │
├──────────────┼────────────────┼─────────────────┤
│ Type         │ Architectural  │ Protocol        │
│              │ Style          │ (Strict)        │
├──────────────┼────────────────┼─────────────────┤
│ Format       │ JSON, XML,     │ Only XML        │
│              │ HTML, Text     │                 │
├──────────────┼────────────────┼─────────────────┤
│ Protocol     │ HTTP only      │ HTTP, SMTP,     │
│              │                │ TCP, etc        │
├──────────────┼────────────────┼─────────────────┤
│ Speed        │ Fast           │ Slow            │
│              │ ████████░░     │ ███░░░░░░░      │
├──────────────┼────────────────┼─────────────────┤
│ Complexity   │ Simple         │ Complex         │
│              │ Easy to learn  │ Hard to learn   │
├──────────────┼────────────────┼─────────────────┤
│ Security     │ HTTPS,         │ Built-in        │
│              │ OAuth          │ WS-Security     │
├──────────────┼────────────────┼─────────────────┤
│ Caching      │ ✅ Yes         │ ❌ No           │
├──────────────┼────────────────┼─────────────────┤
│ Error        │ HTTP Status    │ SOAP Faults     │
│ Handling     │ Codes          │                 │
├──────────────┼────────────────┼─────────────────┤
│ Use Case     │ • Web apps     │ • Banking       │
│              │ • Mobile apps  │ • Payment       │
│              │ • Public APIs  │ • Enterprise    │
│              │ • Microservices│ • Legacy        │
└──────────────┴────────────────┴─────────────────┘

When to Use:
═══════════

REST ব্যবহার করো:
┌────────────────────────────────┐
│ ✓ Modern web/mobile apps       │
│ ✓ Public APIs                  │
│ ✓ Fast development চাইলে       │
│ ✓ JSON data                    │
│ ✓ Bandwidth limited            │
│ ✓ Caching দরকার               │
└────────────────────────────────┘

SOAP ব্যবহার করো:
┌────────────────────────────────┐
│ ✓ Banking systems              │
│ ✓ Payment gateways             │
│ ✓ High security দরকার          │
│ ✓ ACID transactions            │
│ ✓ Enterprise applications      │
│ ✓ Legacy system integration    │
└────────────────────────────────┘
```

---

## 12. Authentication in REST API

### Definition
```
Authentication = User verify করা (কে তুমি?)
Authorization = Permission check করা (তুমি কী করতে পারবে?)
```

### Diagram
```
┌─────────────────────────────────────────────────────────┐
│           AUTHENTICATION FLOW                           │
└─────────────────────────────────────────────────────────┘

Token-Based Authentication:
═══════════════════════════

Step 1: Login করো
CLIENT                                 SERVER
  │                                      │
  │  POST /auth/login                    │
  │  {                                   │
  │    "username": "rahim",              │
  │    "password": "secret123"           │
  │  }                                   │
  ├─────────────────────────────────────>│
  │                                      │
  │                               ┌──────▼──────┐
  │                               │   Verify    │
  │                               │ Credentials │
  │                               └──────┬──────┘
  │                                      │
  │                               ┌──────▼──────┐
  │                               │  Generate   │
  │                               │    Token    │
  │                               └──────┬──────┘
  │                                      │
  │  200 OK                              │
  │  {                                   │
  │    "token": "eyJhbGciOiJ...",        │
  │    "expires_in": 3600,               │
  │    "user": {                         │
  │      "id": 123,                      │
  │      "name": "Rahim"                 │
  │    }                                 │
  │  }                                   │
  │<─────────────────────────────────────┤
  │                                      │
  │  Store token locally                 │
  │  (localStorage/cookie)               │
  │                                      │

Step 2: Protected Resource Access
  │                                      │
  │  GET /api/profile                    │
  │  Authorization: Bearer eyJhbGc...    │
  ├─────────────────────────────────────>│
  │                                      │
  │                               ┌──────▼──────┐
  │                               │   Verify    │
  │                               │    Token    │
  │                               └──────┬──────┘
  │                                      │
  │                               ┌──────▼──────┐
  │                               │  Get User   │
  │                               │    Data     │
  │                               └──────┬──────┘
  │                                      │
  │  200 OK                              │
  │  {                                   │
  │    "id": 123,                        │
  │    "name": "Rahim Ahmed",            │
  │    "email": "rahim@example.com"      │
  │  }                                   │
  │<─────────────────────────────────────┤


Authentication Methods:
══════════════════════

1. Bearer Token (JWT):
   ┌────────────────────────────────────┐
   │ Authorization: Bearer eyJhbGc...   │
   └────────────────────────────────────┘
   
   Most Common ✓

2. API Key:
   ┌────────────────────────────────────┐
   │ X-API-Key: your_api_key_here       │
   └────────────────────────────────────┘
   
   For Service-to-Service

3. Basic Auth:
   ┌────────────────────────────────────┐
   │ Authorization: Basic dXNlcjpwYXNz │
   └────────────────────────────────────┘
   
   Not recommended for production

4. OAuth 2.0:
   ┌────────────────────────────────────┐
   │ Authorization: Bearer oauth_token  │
   └────────────────────────────────────┘
   
   For third-party apps (Google, Facebook)


Error Scenarios:
═══════════════

No Token:
GET /api/profile
  ↓
401 Unauthorized
{
  "error": "Authentication required",
  "message": "Please login"
}

Invalid Token:
GET /api/profile
Authorization: Bearer invalid_token
  ↓
401 Unauthorized
{
  "error": "Invalid token",
  "message": "Token is expired or invalid"
}

No Permission:
DELETE /api/users/456
Authorization: Bearer valid_token
  ↓
403 Forbidden
{
  "error": "Permission denied",
  "message": "You don't have admin access"
}
```

---

## 13. Complete E-commerce API Example

### Definition
```
E-commerce REST API = Online shop এর জন্য complete API
Products, Users, Orders, Cart সব manage করে
```

### Diagram
```
┌─────────────────────────────────────────────────────────┐
│         E-COMMERCE API STRUCTURE                        │
└─────────────────────────────────────────────────────────┘

API Base: https://api.shop.com/v1

Resource Map:
════════════

/api/v1
│
├─ /products ────────────────── Products Management
│  │
│  ├─ GET    /products                (সব products)
│  ├─ POST   /products                (নতুন product)
│  ├─ GET    /products/{id}           (product details)
│  ├─ PATCH  /products/{id}           (update product)
│  ├─ DELETE /products/{id}           (delete product)
│  └─ GET    /products/{id}/reviews   (reviews)
│
├─ /users ──────────────────── User Management
│  │
│  ├─ GET    /users/{id}              (profile)
│  ├─ PATCH  /users/{id}              (update profile)
│  │
│  ├─ GET    /users/{id}/cart         (shopping cart)
│  ├─ POST   /users/{id}/cart         (add to cart)
│  ├─ PATCH  /users/{id}/cart/items/{item_id}  (update qty)
│  ├─ DELETE /users/{id}/cart/items/{item_id}  (remove)
│  │
│  └─ GET    /users/{id}/orders       (user orders)
│
├─ /orders ─────────────────── Order Management
│  │
│  ├─ POST   /orders                  (create order)
│  ├─ GET    /orders/{id}             (order details)
│  ├─ GET    /orders/{id}/tracking    (track order)
│  ├─ POST   /orders/{id}/cancel      (cancel order)
│  └─ GET    /orders/{id}/invoice     (invoice)
│
├─ /categories ─────────────── Categories
│  │
│  ├─ GET    /categories              (all categories)
│  └─ GET    /categories/{id}         (category details)
│
└─ /auth ───────────────────── Authentication
   │
   ├─ POST   /auth/login              (login)
   ├─ POST   /auth/register           (register)
   ├─ POST   /auth/logout             (logout)
   └─ POST   /auth/refresh            (refresh token)


Complete Shopping Flow:
══════════════════════

1. Browse Products
   ═══════════════
   GET /products?category=smartphones&sort=price_asc&page=1
   
   Response:
   {
     "data": [
       {
         "id": 101,
         "name": "iPhone 15 Pro",
         "price": 120000,
         "stock": 25,
         "rating": 4.8
       }
     ],
     "pagination": {...}
   }

2. View Product Details
   ════════════════════
   GET /products/101
   
   Response:
   {
     "id": 101,
     "name": "iPhone 15 Pro",
     "price": 120000,
     "specifications": {...},
     "images": [...],
     "rating": 4.8
   }

3. Add to Cart
   ═══════════
   POST /users/123/cart
   {
     "product_id": 101,
     "quantity": 1
   }
   
   Response: 201 Created
   {
     "cart_item_id": 1,
     "product_id": 101,
     "quantity": 1
   }

4. View Cart
   ═════════
   GET /users/123/cart
   
   Response:
   {
     "items": [
       {
         "product_id": 101,
         "product_name": "iPhone 15 Pro",
         "price": 120000,
         "quantity": 1,
         "subtotal": 120000
       }
     ],
     "summary": {
       "total": 120000
     }
   }

5. Checkout (Create Order)
   ═══════════════════════
   POST /orders
   {
     "shipping_address": {...},
     "payment_method": "bKash",
     "items": [...]
   }
   
   Response: 201 Created
   {
     "order_id": "ORD-2026-001",
     "status": "pending_payment",
     "total": 120000,
     "payment": {
       "payment_url": "https://pay.bkash.com/..."
     }
   }

6. Track Order
   ═══════════
   GET /orders/ORD-2026-001/tracking
   
   Response:
   {
     "order_id": "ORD-2026-001",
     "current_status": "shipped",
     "tracking_number": "SUN-123456",
     "estimated_delivery": "2026-02-09",
     "tracking_history": [...]
   }

Flow Diagram:
════════════

User Journey:
┌──────────┐    ┌──────────┐    ┌──────────┐
│  Browse  │───►│   View   │───►│   Add    │
│ Products │    │ Details  │    │ to Cart  │
└──────────┘    └──────────┘    └──────────┘
                                      │
                                      ▼
┌──────────┐    ┌──────────┐    ┌──────────┐
│  Track   │◄───│ Checkout │◄───│   View   │
│  Order   │    │  & Pay   │    │   Cart   │
└──────────┘    └──────────┘    └──────────┘
```

---

## 14. Quick Reference Sheet

### Summary of All Concepts
```
┌─────────────────────────────────────────────────────────┐
│              REST API QUICK REFERENCE                   │
└─────────────────────────────────────────────────────────┘

Core Concepts:
═════════════

Resource ────► কী নিয়ে কাজ (User, Product, Order)
State ───────► বর্তমান তথ্য (Database data)
Representation ─► Format (JSON, XML, HTML)
Transfer ────► পাঠানো (HTTP)

REST = Resource + State + Representation + Transfer

HTTP Methods:
════════════

GET    ────► Read (পড়া)
POST   ────► Create (তৈরি)
PUT    ────► Replace (পুরো replace)
PATCH  ────► Update (আংশিক update)
DELETE ────► Delete (মুছে ফেলা)

Status Codes:
════════════

2xx ────► Success (সফল)
  200 OK
  201 Created
  204 No Content

4xx ────► Client Error
  400 Bad Request
  401 Unauthorized
  403 Forbidden
  404 Not Found

5xx ────► Server Error
  500 Internal Server Error
  503 Service Unavailable

URL Patterns:
════════════

/resources              ────► Collection
/resources/{id}         ────► Individual
/resources/{id}/sub     ────► Sub-resource

Query Parameters:
════════════════

?page=1&limit=20        ────► Pagination
?sort=price_asc         ────► Sorting
?category=phones        ────► Filtering
?search=iphone          ────► Search

Headers:
═══════

Request:
  Content-Type: application/json
  Authorization: Bearer {token}
  Accept: application/json

Response:
  Content-Type: application/json
  Cache-Control: max-age=3600
  ETag: "abc123"

Best Practices:
══════════════

✓ Use nouns, not verbs in URLs
✓ Use plural names (/users not /user)
✓ Use lowercase
✓ Version your API (/v1/)
✓ Use HTTP methods correctly
✓ Return proper status codes
✓ Provide meaningful error messages
✓ Use pagination for large datasets
✓ Implement caching
✓ Secure with HTTPS & Auth tokens

✗ Don't use verbs (/getUsers)
✗ Don't expose database structure
✗ Don't use actions in URL
✗ Don't return sensitive data
✗ Don't forget error handling
```

---


# REST API - Basics (Easy Way)

---

## 68. What is REST & RESTful API?

### REST কী?

```
┌────────────────────────────────────────────────────┐
│             REST = কী?                             │
└────────────────────────────────────────────────────┘

REST = REpresentational State Transfer

সহজ বাংলায়:
Resource এর State কে Representation আকারে Transfer করা

উপমা দিয়ে বুঝি:
═══════════════

একটা রেস্টুরেন্টে:

┌──────────┐        ┌──────────┐        ┌──────────┐
│   তুমি   │───────►│  Waiter  │───────►│  Kitchen │
│ (Client) │        │ (REST    │        │ (Server/ │
│          │        │  API)    │        │ Database)│
└──────────┘        └──────────┘        └──────────┘
     │                   │                    │
     │ Order: Biriyani   │                    │
     │ ─────────────────►│                    │
     │                   │ Order forward      │
     │                   │───────────────────►│
     │                   │                    │
     │                   │      Cook করছে    │
     │                   │                    │
     │                   │    Biriyani        │
     │                   │◄───────────────────┤
     │   Biriyani        │                    │
     │◄──────────────────┤                    │
     │                   │                    │

এখানে:
- তুমি = Client (Browser/App)
- Waiter = REST API
- Kitchen = Server/Database
- Order = HTTP Request
- Biriyani = HTTP Response
```

### REST এর মূল Concepts

```
┌────────────────────────────────────────────────────┐
│           4টি মূল Concept                          │
└────────────────────────────────────────────────────┘

1. RESOURCE (রিসোর্স)
   ═══════════════════
   কী নিয়ে কাজ করছি?
   
   Examples:
   - User (ব্যবহারকারী)
   - Product (পণ্য)
   - Order (অর্ডার)
   
   URL:
   /users/123      ← User resource
   /products/456   ← Product resource
   /orders/789     ← Order resource

2. STATE (স্টেট)
   ══════════════
   Resource এর বর্তমান তথ্য
   
   Example - User 123:
   {
     "id": 123,
     "name": "Rahim",
     "email": "rahim@email.com",
     "balance": 5000
   }

3. REPRESENTATION (রিপ্রেজেন্টেশন)
   ═══════════════════════════════
   State কে কোন format এ দেখাচ্ছি?
   
   Same state, different formats:
   
   JSON:                  XML:
   {                      <user>
     "name": "Rahim"        <name>Rahim</name>
   }                      </user>

4. TRANSFER (ট্রান্সফার)
   ═══════════════════════
   HTTP দিয়ে data পাঠানো
   
   Client ────HTTP────► Server
          ◄───HTTP────
```

### RESTful API কী?

```
┌────────────────────────────────────────────────────┐
│           RESTful API                              │
└────────────────────────────────────────────────────┘

Definition:
যে API REST এর নিয়ম-কানুন মেনে চলে

Example - E-commerce API:

✅ RESTful (সঠিক):
GET    /products           → সব products
GET    /products/101       → Product 101
POST   /products           → নতুন product তৈরি
PUT    /products/101       → Product 101 update
DELETE /products/101       → Product 101 delete

❌ Not RESTful (ভুল):
GET    /getAllProducts
GET    /getProductById?id=101
POST   /createNewProduct
POST   /updateProduct?id=101
POST   /deleteProduct?id=101

পার্থক্য:
- RESTful: Resource-based URLs + HTTP methods
- Not RESTful: Action-based URLs
```

### Real Example

```
┌────────────────────────────────────────────────────┐
│        Facebook API Example                        │
└────────────────────────────────────────────────────┘

Scenario: তুমি Facebook এ তোমার profile দেখতে চাও

Step 1: Request
CLIENT (Your Phone)
  │
  │ GET /users/rahim HTTP/1.1
  │ Authorization: Bearer token123
  │
  ▼
SERVER (Facebook)

Step 2: Server Processing
  ┌────────────────────┐
  │ Database থেকে      │
  │ Rahim এর data নাও  │
  └────────┬───────────┘
           │
  ┌────────▼───────────┐
  │ JSON এ convert করো│
  └────────┬───────────┘
           │
Step 3: Response
           │
           ▼
  {
    "id": "rahim",
    "name": "Rahim Ahmed",
    "friends": 532,
    "posts": 245
  }
           │
           ▼
CLIENT
  └─► Display in App ✓
```

---

## 69. HTTP Request and Response Structures

### HTTP Request Structure

```
┌────────────────────────────────────────────────────┐
│          HTTP REQUEST STRUCTURE                    │
└────────────────────────────────────────────────────┘

Request এর 3টি অংশ:

┌─────────────────────────────────────────┐
│ 1. REQUEST LINE                         │
│    METHOD  PATH  HTTP-VERSION           │
├─────────────────────────────────────────┤
│ 2. HEADERS                              │
│    Key: Value                           │
│    Key: Value                           │
├─────────────────────────────────────────┤
│ 3. BODY (optional)                      │
│    {data}                               │
└─────────────────────────────────────────┘

Real Example:
════════════

POST /api/users HTTP/1.1                  ← REQUEST LINE
Host: api.example.com                     ┐
Content-Type: application/json            │
Authorization: Bearer token123            │ HEADERS
Accept: application/json                  │
User-Agent: Mobile App 1.0                ┘
                                          ← Empty line
{                                         ┐
  "name": "Rahim Ahmed",                  │
  "email": "rahim@email.com",             │ BODY
  "age": 28                               │
}                                         ┘
```

#### 1. Request Line বিস্তারিত

```
┌────────────────────────────────────────────────────┐
│          REQUEST LINE                              │
└────────────────────────────────────────────────────┘

Format: METHOD PATH HTTP-VERSION

METHOD (HTTP Methods):
═══════════════════

GET     → Data পড়ো (Read)
POST    → নতুন তৈরি করো (Create)
PUT     → পুরো replace করো (Replace)
PATCH   → আংশিক update করো (Update)
DELETE  → মুছে ফেলো (Delete)

PATH (Resource Path):
═══════════════════

/users              → সব users
/users/123          → User 123
/users/123/orders   → User 123 এর orders
/products?page=1    → Products with query

HTTP-VERSION:
════════════

HTTP/1.1    (most common)
HTTP/2
HTTP/3

Examples:
════════

GET /api/products HTTP/1.1
POST /api/orders HTTP/1.1
DELETE /api/users/456 HTTP/1.1
```

#### 2. Headers বিস্তারিত

```
┌────────────────────────────────────────────────────┐
│          COMMON REQUEST HEADERS                    │
└────────────────────────────────────────────────────┘

Host:
════
Host: api.example.com
→ কোন server এ যাচ্ছি (Required in HTTP/1.1)

Content-Type:
════════════
Content-Type: application/json
→ Body তে কী format এ data পাঠাচ্ছি

Common values:
- application/json        (JSON data)
- application/xml         (XML data)
- application/x-www-form-urlencoded (Form data)
- multipart/form-data     (File upload)
- text/html               (HTML)

Accept:
══════
Accept: application/json
→ Response কোন format এ চাই

Authorization:
═════════════
Authorization: Bearer eyJhbGc...
→ Authentication token

Common types:
- Bearer <token>          (JWT tokens)
- Basic <credentials>     (username:password)
- API-Key <key>           (API key)

User-Agent:
══════════
User-Agent: Mozilla/5.0 Chrome/120
→ কোন client/browser থেকে request

Accept-Language:
═══════════════
Accept-Language: bn,en
→ কোন language এ response চাই

Cache-Control:
═════════════
Cache-Control: no-cache
→ Caching behavior

Cookie:
══════
Cookie: session=abc123; user_id=456
→ Session/user data
```

#### 3. Body বিস্তারিত

```
┌────────────────────────────────────────────────────┐
│          REQUEST BODY                              │
└────────────────────────────────────────────────────┘

কখন Body থাকে?
═══════════════

✓ POST   - নতুন data তৈরি করতে
✓ PUT    - Data update করতে
✓ PATCH  - Partial update করতে
✗ GET    - Body থাকে না
✗ DELETE - সাধারণত Body থাকে না

JSON Body Example:
═════════════════

POST /api/orders
Content-Type: application/json

{
  "user_id": 123,
  "product_id": 456,
  "quantity": 2,
  "total": 10000,
  "shipping_address": {
    "street": "Road 5, Dhanmondi",
    "city": "Dhaka",
    "zip": "1209"
  }
}

Form Data Example:
═════════════════

POST /api/login
Content-Type: application/x-www-form-urlencoded

username=rahim&password=secret123

File Upload Example:
═══════════════════

POST /api/upload
Content-Type: multipart/form-data

------boundary123
Content-Disposition: form-data; name="file"; filename="photo.jpg"
Content-Type: image/jpeg

(binary file data)
------boundary123--
```

### HTTP Response Structure

```
┌────────────────────────────────────────────────────┐
│          HTTP RESPONSE STRUCTURE                   │
└────────────────────────────────────────────────────┘

Response এর 3টি অংশ:

┌─────────────────────────────────────────┐
│ 1. STATUS LINE                          │
│    HTTP-VERSION  STATUS-CODE  MESSAGE   │
├─────────────────────────────────────────┤
│ 2. HEADERS                              │
│    Key: Value                           │
│    Key: Value                           │
├─────────────────────────────────────────┤
│ 3. BODY                                 │
│    {data}                               │
└─────────────────────────────────────────┘

Real Example:
════════════

HTTP/1.1 200 OK                           ← STATUS LINE
Date: Sat, 07 Feb 2026 10:30:00 GMT      ┐
Server: nginx/1.20.1                      │
Content-Type: application/json            │ HEADERS
Content-Length: 234                       │
Cache-Control: max-age=3600               ┘
                                          ← Empty line
{                                         ┐
  "id": 123,                              │
  "name": "Rahim Ahmed",                  │ BODY
  "email": "rahim@email.com",             │
  "age": 28                               │
}                                         ┘
```

#### 1. Status Line বিস্তারিত

```
┌────────────────────────────────────────────────────┐
│          STATUS LINE                               │
└────────────────────────────────────────────────────┘

Format: HTTP-VERSION STATUS-CODE MESSAGE

STATUS CODES:
════════════

2xx - SUCCESS (সফল) ✅
┌─────┬────────────────┬────────────────────┐
│ 200 │ OK             │ সফল, data আছে     │
│ 201 │ Created        │ নতুন তৈরি হয়েছে   │
│ 204 │ No Content     │ সফল, body নেই     │
└─────┴────────────────┴────────────────────┘

4xx - CLIENT ERROR (Client এর ভুল) ❌
┌─────┬────────────────┬────────────────────┐
│ 400 │ Bad Request    │ Invalid data       │
│ 401 │ Unauthorized   │ Login লাগবে        │
│ 403 │ Forbidden      │ Permission নেই     │
│ 404 │ Not Found      │ পাওয়া যায়নি      │
│ 409 │ Conflict       │ Duplicate entry    │
└─────┴────────────────┴────────────────────┘

5xx - SERVER ERROR (Server এ সমস্যা) 💥
┌─────┬────────────────┬────────────────────┐
│ 500 │ Internal Error │ Server crash       │
│ 502 │ Bad Gateway    │ Gateway problem    │
│ 503 │ Unavailable    │ Server down        │
└─────┴────────────────┴────────────────────┘

Examples:
════════

HTTP/1.1 200 OK
HTTP/1.1 201 Created
HTTP/1.1 404 Not Found
HTTP/1.1 500 Internal Server Error
```

#### 2. Response Headers বিস্তারিত

```
┌────────────────────────────────────────────────────┐
│          COMMON RESPONSE HEADERS                   │
└────────────────────────────────────────────────────┘

Content-Type:
════════════
Content-Type: application/json
→ Response body কী format এ আছে

Content-Length:
══════════════
Content-Length: 1234
→ Body এর size (bytes এ)

Date:
════
Date: Sat, 07 Feb 2026 10:30:00 GMT
→ Response এর time

Server:
══════
Server: nginx/1.20.1
→ কোন server software

Cache-Control:
═════════════
Cache-Control: max-age=3600
→ কতক্ষণ cache রাখতে পারবে

ETag:
════
ETag: "abc123"
→ Resource এর version ID

Location:
════════
Location: /api/users/124
→ নতুন resource এর URL (201 Created এ)

Set-Cookie:
══════════
Set-Cookie: session=xyz789; HttpOnly
→ Browser এ cookie set করো

Access-Control-Allow-Origin:
═══════════════════════════
Access-Control-Allow-Origin: *
→ CORS - কোন domain থেকে access করা যাবে
```

#### 3. Response Body বিস্তারিত

```
┌────────────────────────────────────────────────────┐
│          RESPONSE BODY EXAMPLES                    │
└────────────────────────────────────────────────────┘

Success Response:
════════════════

GET /api/users/123
→
200 OK
{
  "id": 123,
  "name": "Rahim Ahmed",
  "email": "rahim@email.com",
  "age": 28,
  "balance": 5000
}

Created Response:
════════════════

POST /api/products
→
201 Created
Location: /api/products/456
{
  "id": 456,
  "name": "iPhone 15",
  "price": 120000,
  "created_at": "2026-02-07T10:30:00Z"
}

Error Response:
══════════════

GET /api/users/99999
→
404 Not Found
{
  "error": {
    "code": "USER_NOT_FOUND",
    "message": "User with ID 99999 not found",
    "status": 404
  }
}

No Content Response:
═══════════════════

DELETE /api/users/123
→
204 No Content
(empty body)

List Response:
═════════════

GET /api/products?page=1
→
200 OK
{
  "data": [
    {"id": 1, "name": "Product 1"},
    {"id": 2, "name": "Product 2"}
  ],
  "pagination": {
    "current_page": 1,
    "total_pages": 10,
    "total_items": 100
  }
}
```

### Complete Request-Response Flow

```
┌────────────────────────────────────────────────────┐
│       COMPLETE REQUEST-RESPONSE CYCLE              │
└────────────────────────────────────────────────────┘

CLIENT                              SERVER
  │                                   │
  │  REQUEST ────────────────────────►│
  │                                   │
  │  POST /api/orders HTTP/1.1        │
  │  Host: api.shop.com               │
  │  Content-Type: application/json   │
  │  Authorization: Bearer token      │
  │                                   │
  │  {                                │
  │    "product_id": 101,             │
  │    "quantity": 2                  │
  │  }                                │
  │                                   │
  │                            ┌──────▼──────┐
  │                            │ 1. Validate │
  │                            │    Request  │
  │                            └──────┬──────┘
  │                                   │
  │                            ┌──────▼──────┐
  │                            │ 2. Check    │
  │                            │    Auth     │
  │                            └──────┬──────┘
  │                                   │
  │                            ┌──────▼──────┐
  │                            │ 3. Process  │
  │                            │    Order    │
  │                            └──────┬──────┘
  │                                   │
  │                            ┌──────▼──────┐
  │                            │ 4. Create   │
  │                            │    Response │
  │                            └──────┬──────┘
  │                                   │
  │  RESPONSE ◄───────────────────────┤
  │                                   │
  │  HTTP/1.1 201 Created             │
  │  Content-Type: application/json   │
  │  Location: /api/orders/ORD-001    │
  │                                   │
  │  {                                │
  │    "order_id": "ORD-001",         │
  │    "status": "pending",           │
  │    "total": 10000                 │
  │  }                                │
  │                                   │
  ▼                                   ▼
```

---

## 70. Top 5 REST Guidelines and Advantages

### REST এর মূল Guidelines

```
┌────────────────────────────────────────────────────┐
│         TOP 5 REST GUIDELINES                      │
└────────────────────────────────────────────────────┘

1. CLIENT-SERVER ARCHITECTURE
2. STATELESS
3. UNIFORM INTERFACE
4. CACHEABLE
5. LAYERED SYSTEM
```

### Guideline 1: Client-Server Architecture

```
┌────────────────────────────────────────────────────┐
│      1. CLIENT-SERVER ARCHITECTURE                 │
└────────────────────────────────────────────────────┘

নিয়ম:
════
Client ও Server আলাদা, independent থাকবে

Diagram:
═══════

┌──────────────┐              ┌──────────────┐
│   CLIENT     │              │   SERVER     │
│              │              │              │
│ • UI/Display │              │ • Logic      │
│ • User Input │◄────────────►│ • Database   │
│ • Rendering  │ Independent  │ • Processing │
│              │              │              │
└──────────────┘              └──────────────┘

Example:
═══════

Same Server, Different Clients:

SERVER: api.shop.com
    │
    ├─► Web App (React)
    ├─► Mobile App (Android)
    ├─► Mobile App (iOS)
    ├─► Desktop App (Electron)
    └─► Smart Watch App

সুবিধা (Advantages):
═══════════════════

✓ Portability:
  - Mobile app আলাদা
  - Web app আলাদা
  - কিন্তু same API ব্যবহার করে

✓ Scalability:
  - Server আলাদা scale করা যায়
  - Client আলাদা scale করা যায়

✓ Development:
  - Frontend team আলাদা কাজ করতে পারে
  - Backend team আলাদা কাজ করতে পারে
  - Parallel development

✓ Updates:
  - Server update করলে client change লাগে না
  - UI change করলে server change লাগে না

Real Example:
════════════

Facebook:
- Facebook.com (Web)
- Facebook Mobile App
- Facebook Lite
- Instagram (same API!)
- WhatsApp (same infra!)

All use same backend servers! ✓
```

### Guideline 2: Stateless

```
┌────────────────────────────────────────────────────┐
│           2. STATELESS                             │
└────────────────────────────────────────────────────┘

নিয়ম:
════
প্রতিটা request complete হতে হবে
Server আগের request মনে রাখবে না

❌ Stateful (ভুল):
════════════════

Request 1:
POST /login
{username: "rahim", password: "123"}

Server stores:
session["user"] = "rahim"    ← Server মনে রাখছে

Request 2:
GET /profile
(no auth - server remembers user)

✅ Stateless (সঠিক):
══════════════════

Request 1:
POST /login
{username: "rahim", password: "123"}

Response:
{token: "abc123xyz"}

Request 2:
GET /profile
Authorization: Bearer abc123xyz    ← প্রতিবার token পাঠাতে হবে

সুবিধা (Advantages):
═══════════════════

✓ Reliability:
  Server crash হলেও data loss নেই
  Session store লাগে না

✓ Scalability:
  Load Balancer:
  
  Request 1 → Server A ✓
  Request 2 → Server B ✓ (কোনো server handle করতে পারে)
  Request 3 → Server C ✓

✓ Simplicity:
  Server simple থাকে
  No session management

✓ Caching:
  সহজে cache করা যায়

Example:
═══════

REST API (Stateless):
══════════════════════

Every request has everything:

GET /api/orders
Authorization: Bearer token123    ← Auth info
Accept: application/json          ← What format
User-Agent: Mobile App            ← Client info

Server না জেনেও process করতে পারে!
```

### Guideline 3: Uniform Interface

```
┌────────────────────────────────────────────────────┐
│         3. UNIFORM INTERFACE                       │
└────────────────────────────────────────────────────┘

নিয়ম:
════
Standard way তে interact করতে হবে

4টি Sub-principles:
══════════════════

a) Resource-Based URLs
b) HTTP Methods সঠিকভাবে ব্যবহার
c) Self-Descriptive Messages
d) HATEOAS (Hypermedia)

a) Resource-Based URLs:
══════════════════════

✅ সঠিক:
/users              (resource = users)
/products           (resource = products)
/orders/123         (resource = order 123)

❌ ভুল:
/getUsers           (action in URL)
/createProduct      (action in URL)
/deleteOrder?id=123 (action in URL)

b) HTTP Methods:
═══════════════

GET    → Read (পড়া)
POST   → Create (তৈরি করা)
PUT    → Replace (replace করা)
PATCH  → Update (update করা)
DELETE → Delete (মুছে ফেলা)

Example:
GET    /users/123      → User 123 দেখাও
POST   /users          → নতুন user তৈরি করো
PUT    /users/123      → User 123 replace করো
PATCH  /users/123      → User 123 update করো
DELETE /users/123      → User 123 delete করো

c) Self-Descriptive:
═══════════════════

Request & Response নিজেই সব বলে দেয়

Request:
GET /api/users/123
Accept: application/json       ← JSON চাই
Authorization: Bearer token    ← Authenticated

Response:
200 OK
Content-Type: application/json ← JSON পাঠাচ্ছি
Content-Length: 234            ← Size
{data}

d) HATEOAS:
══════════

Response এ related links থাকবে

Example:
GET /api/orders/ORD-001

Response:
{
  "order_id": "ORD-001",
  "status": "shipped",
  "total": 5000,
  "_links": {                        ← Links!
    "self": "/orders/ORD-001",
    "track": "/orders/ORD-001/tracking",
    "cancel": "/orders/ORD-001/cancel",
    "invoice": "/orders/ORD-001/invoice"
  }
}

সুবিধা (Advantages):
═══════════════════

✓ Predictable:
  URL দেখলেই বুঝা যায় কী করছে
  /users/123 → definitely user 123

✓ Easy to Learn:
  একবার শিখলে সব API বুঝা যায়
  Same pattern everywhere

✓ Tooling:
  Postman, Swagger সহজে কাজ করে
  Standard tools available

✓ Documentation:
  Auto-generate করা যায়
  Easy to understand
```

### Guideline 4: Cacheable

```
┌────────────────────────────────────────────────────┐
│            4. CACHEABLE                            │
└────────────────────────────────────────────────────┘

নিয়ম:
════
Response cache করা যাবে কিনা বলতে হবে

Headers ব্যবহার করে:
═══════════════════

Response:
GET /api/products/123
→
200 OK
Cache-Control: max-age=3600         ← 1 hour cache করো
ETag: "abc123"                      ← Version ID
Last-Modified: Sat, 07 Feb 10:00 GMT

পরে আবার request:
═══════════════

GET /api/products/123
If-None-Match: "abc123"    ← ETag পাঠালাম

Response:
304 Not Modified           ← Unchanged, cache use করো
(no body - bandwidth saved!)

Cache Flow:
══════════

Request → Check Cache
            │
    ┌───────┴───────┐
    │               │
  Found           Not Found
  & Valid           │
    │               │
Use Cache      Request Server
                    │
              Update Cache

সুবিধা (Advantages):
═══════════════════

✓ Performance:
  Timeline without cache:
  Req1: ████ (100ms)
  Req2: ████ (100ms)
  Req3: ████ (100ms)
  Total: 300ms
  
  Timeline with cache:
  Req1: ████ (100ms - from server)
  Req2: ░░░░ (5ms - from cache!)
  Req3: ░░░░ (5ms - from cache!)
  Total: 110ms
  
  63% faster! 🚀

✓ Bandwidth:
  No cache:
  - 3 requests × 50KB = 150KB
  
  With cache:
  - 1 request × 50KB = 50KB
  - 2 from cache = 0KB
  Total: 50KB (70% saved!)

✓ Server Load:
  কম requests = কম CPU/database usage

✓ Cost:
  কম bandwidth = কম cost

Example:
═══════

Amazon Product Page:
- Product info: Cache 1 hour
- Product images: Cache 1 day
- User cart: No cache
- Stock info: Cache 5 minutes
```

### Guideline 5: Layered System

```
┌────────────────────────────────────────────────────┐
│         5. LAYERED SYSTEM                          │
└────────────────────────────────────────────────────┘

নিয়ম:
════
Client জানবে না মাঝে কয়টা layer আছে

Architecture:
════════════

CLIENT
  │
  │ "আমি শুধু api.shop.com এ request করছি"
  │ "মাঝে কী আছে জানি না, জানতে চাই না"
  ▼
┌─────────────────┐
│ Load Balancer   │ ← Layer 1
└────────┬────────┘
         ▼
┌─────────────────┐
│ API Gateway     │ ← Layer 2
└────────┬────────┘
         ▼
┌─────────────────┐
│ Authentication  │ ← Layer 3
└────────┬────────┘
         ▼
┌─────────────────┐
│ App Server      │ ← Layer 4
└────────┬────────┘
         ▼
┌─────────────────┐
│ Database        │ ← Layer 5
└─────────────────┘

Client শুধু জানে:
"আমি api.shop.com এ request পাঠাই"
"Response পাই"

সুবিধা (Advantages):
═══════════════════

✓ Security:
  Layers যোগ করা যায়:
  
  CLIENT
    ↓
  Firewall        ← নতুন layer!
    ↓
  Rate Limiter    ← নতুন layer!
    ↓
  DDoS Protection ← নতুন layer!
    ↓
  Server

✓ Scalability:
  যেকোনো layer scale করা যায়:
  
  Load Balancer
    ├─► Server 1
    ├─► Server 2
    ├─► Server 3   ← নতুন server যোগ করলাম
    └─► Server 4   ← client জানেও না!

✓ Caching:
  মাঝে caching layer যোগ করা যায়:
  
  CLIENT
    ↓
  CDN Cache      ← Fast response!
    ↓
  Server

✓ Flexibility:
  Backend change করা যায়:
  
  Before:
  Client → MySQL
  
  After:
  Client → PostgreSQL  (client unchanged!)

Example:
═══════

Netflix Architecture:
CLIENT
  ↓
CDN (Akamai)          ← Video cache
  ↓
AWS CloudFront        ← Static content
  ↓
Load Balancer
  ↓
API Gateway
  ↓
Microservices
  ├─► Auth Service
  ├─► Video Service
  ├─► Payment Service
  └─► Recommendation

Client শুধু netflix.com access করে
মাঝে 100+ services আছে, জানে না!
```

### Summary Table

```
┌────────────────────────────────────────────────────┐
│      REST GUIDELINES - SUMMARY                     │
└────────────────────────────────────────────────────┘

┌─────────────┬──────────────┬─────────────────────┐
│ Guideline   │ মানে         │ Main Advantage      │
├─────────────┼──────────────┼─────────────────────┤
│ Client-     │ আলাদা থাকবে  │ Multiple clients    │
│ Server      │              │ possible            │
├─────────────┼──────────────┼─────────────────────┤
│ Stateless   │ সব info      │ Scalability,        │
│             │ request এ    │ Reliability         │
├─────────────┼──────────────┼─────────────────────┤
│ Uniform     │ Standard     │ Easy to learn,      │
│ Interface   │ way          │ Predictable         │
├─────────────┼──────────────┼─────────────────────┤
│ Cacheable   │ Cache করা   │ Performance,        │
│             │ যাবে         │ Less bandwidth      │
├─────────────┼──────────────┼─────────────────────┤
│ Layered     │ Layers আলাদা│ Security,           │
│ System      │              │ Scalability         │
└─────────────┴──────────────┴─────────────────────┘

মনে রাখার সহজ উপায়:
═══════════════════

C - Client-Server    (আলাদা আলাদা)
S - Stateless        (সব info request এ)
U - Uniform          (এক নিয়মে)
C - Cacheable        (মনে রাখা যায়)
L - Layered          (স্তর বিন্যাস)

CSCUL = REST এর মূল ভিত্তি
```

---

## 71. REST API vs SOAP API

### মূল পার্থক্য

```
┌────────────────────────────────────────────────────┐
│          REST vs SOAP - Overview                   │
└────────────────────────────────────────────────────┘

REST:
════
- Architectural Style (নিয়ম-কানুন)
- Flexible
- Modern

SOAP:
════
- Protocol (কঠোর নিয়ম)
- Strict
- Old (but still used)

উপমা:
════

REST = বাসা থেকে বাজার যাওয়া
- হেঁটে যাও ✓
- রিকশায় যাও ✓
- গাড়িতে যাও ✓
- Flexible!

SOAP = ট্রেনে যাওয়া
- শুধু নির্দিষ্ট route
- নির্দিষ্ট সময়
- Strict rules!
```

### Detailed Comparison

```
┌────────────────────────────────────────────────────┐
│           DETAILED COMPARISON                      │
└────────────────────────────────────────────────────┘

1. MESSAGE FORMAT
   ══════════════

REST (Simple JSON):
{
  "name": "Rahim",
  "age": 28
}
(50 bytes - small ✓)

SOAP (Complex XML):
<?xml version="1.0"?>
<soap:Envelope 
  xmlns:soap="http://www.w3.org/2003/05/soap-envelope">
  <soap:Header>
  </soap:Header>
  <soap:Body>
    <GetUserResponse>
      <User>
        <name>Rahim</name>
        <age>28</age>
      </User>
    </GetUserResponse>
  </soap:Body>
</soap:Envelope>
(300+ bytes - large ✗)


2. PROTOCOL
   ════════

REST:
- শুধু HTTP
- GET, POST, PUT, DELETE

SOAP:
- HTTP
- SMTP (Email)
- TCP
- FTP
- Multiple protocols!


3. DATA FORMAT
   ═══════════

REST supports multiple:
- JSON ✓ (most common)
- XML ✓
- HTML ✓
- Plain Text ✓
- CSV ✓

SOAP:
- শুধু XML ✗


4. EASE OF USE
   ═══════════

REST (Easy):
fetch('https://api.example.com/users/123')
  .then(res => res.json())
  .then(data => console.log(data))

3 lines! ✓

SOAP (Complex):
- WSDL file লাগবে
- Special libraries লাগবে
- XML parsing লাগবে
- 20+ lines code!


5. PERFORMANCE
   ═══════════

REST:
Request:  50 bytes
Response: 100 bytes
Total:    150 bytes
Time:     50ms
Speed:    ████████░░ 80%

SOAP:
Request:  300 bytes
Response: 500 bytes
Total:    800 bytes
Time:     150ms
Speed:    ███░░░░░░░ 30%

REST 3x faster! 🚀


6. CACHING
   ═══════

REST:
✓ HTTP caching সরাসরি কাজ করে
Cache-Control: max-age=3600

SOAP:
✗ কোনো built-in caching নেই


7. SECURITY
   ════════

REST:
- HTTPS
- OAuth 2.0
- JWT tokens
- API Keys

SOAP:
- Built-in WS-Security
- SAML tokens
- Encryption
- Digital signatures
(More features but complex)


8. ERROR HANDLING
   ══════════════

REST (Simple):
404 Not Found
{
  "error": "User not found",
  "code": "USER_NOT_FOUND"
}

SOAP (Complex):
<soap:Fault>
  <faultcode>soap:Client</faultcode>
  <faultstring>User not found</faultstring>
  <detail>...</detail>
</soap:Fault>


9. TOOLING
   ═══════

REST:
- Postman ✓
- Insomnia ✓
- Swagger ✓
- curl ✓
- Browser DevTools ✓

SOAP:
- SoapUI (specialized tool needed)
- Complex setup
```

### Visual Comparison

```
┌────────────────────────────────────────────────────┐
│         REST vs SOAP - Side by Side                │
└────────────────────────────────────────────────────┘

REST Example:
════════════

Request:
┌────────────────────────────┐
│ GET /users/123 HTTP/1.1    │
│ Host: api.example.com      │
│ Accept: application/json   │
└────────────────────────────┘

Response:
┌────────────────────────────┐
│ 200 OK                     │
│ {                          │
│   "id": 123,               │
│   "name": "Rahim"          │
│ }                          │
└────────────────────────────┘

Simple! ✓


SOAP Example:
════════════

Request:
┌─────────────────────────────────────────┐
│ POST /soap HTTP/1.1                     │
│ Host: api.example.com                   │
│ Content-Type: text/xml                  │
│                                         │
│ <?xml version="1.0"?>                   │
│ <soap:Envelope>                         │
│   <soap:Body>                           │
│     <GetUser>                           │
│       <userId>123</userId>              │
│     </GetUser>                          │
│   </soap:Body>                          │
│ </soap:Envelope>                        │
└─────────────────────────────────────────┘

Response:
┌─────────────────────────────────────────┐
│ 200 OK                                  │
│ <?xml version="1.0"?>                   │
│ <soap:Envelope>                         │
│   <soap:Body>                           │
│     <GetUserResponse>                   │
│       <user>                            │
│         <id>123</id>                    │
│         <name>Rahim</name>              │
│       </user>                           │
│     </GetUserResponse>                  │
│   </soap:Body>                          │
│ </soap:Envelope>                        │
└─────────────────────────────────────────┘

Complex! ✗
```

### Comparison Table

```
┌────────────────────────────────────────────────────┐
│         COMPARISON TABLE                           │
└────────────────────────────────────────────────────┘

┌──────────────┬─────────────┬──────────────────┐
│ Feature      │ REST        │ SOAP             │
├──────────────┼─────────────┼──────────────────┤
│ Type         │ Style       │ Protocol         │
├──────────────┼─────────────┼──────────────────┤
│ Format       │ JSON/XML/   │ XML only         │
│              │ HTML        │                  │
├──────────────┼─────────────┼──────────────────┤
│ Protocol     │ HTTP        │ HTTP/SMTP/TCP    │
├──────────────┼─────────────┼──────────────────┤
│ Complexity   │ Simple      │ Complex          │
│              │ ████░░      │ ██████████       │
├──────────────┼─────────────┼──────────────────┤
│ Speed        │ Fast        │ Slow             │
│              │ ████████    │ ███░░░░░         │
├──────────────┼─────────────┼──────────────────┤
│ Learning     │ Easy        │ Hard             │
│ Curve        │ ██░░░░      │ ████████         │
├──────────────┼─────────────┼──────────────────┤
│ Security     │ HTTPS/OAuth │ Built-in         │
│              │             │ WS-Security      │
├──────────────┼─────────────┼──────────────────┤
│ Caching      │ ✓ Yes       │ ✗ No             │
├──────────────┼─────────────┼──────────────────┤
│ Bandwidth    │ Low         │ High             │
├──────────────┼─────────────┼──────────────────┤
│ Stateless    │ ✓ Yes       │ Can be stateful  │
├──────────────┼─────────────┼──────────────────┤
│ ACID         │ ✗ No        │ ✓ Yes            │
│ Transaction  │             │                  │
├──────────────┼─────────────┼──────────────────┤
│ Browser      │ ✓ Easy      │ ✗ Difficult      │
│ Friendly     │             │                  │
├──────────────┼─────────────┼──────────────────┤
│ Mobile       │ ✓ Perfect   │ ✗ Heavy          │
│ Apps         │             │                  │
└──────────────┴─────────────┴──────────────────┘
```

### When to Use What?

```
┌────────────────────────────────────────────────────┐
│         WHEN TO USE REST vs SOAP?                  │
└────────────────────────────────────────────────────┘

Use REST When:
═════════════

✓ Modern web/mobile applications
  - Facebook, Twitter, Instagram
  - E-commerce sites
  - Social media apps

✓ Public APIs
  - Google Maps API
  - Weather API
  - Payment APIs (Stripe, PayPal)

✓ Microservices
  - Netflix
  - Amazon
  - Uber

✓ Fast development needed
  - Startups
  - MVPs
  - Prototypes

✓ Bandwidth limited
  - Mobile apps
  - IoT devices
  - Low-bandwidth areas

✓ JSON data
  - JavaScript frontends
  - React, Vue, Angular apps

Example:
────────
Foodpanda, Pathao, bKash mobile apps
→ All use REST APIs! ✓


Use SOAP When:
═════════════

✓ Banking systems
  - Transaction processing
  - Account management
  - Money transfers

✓ Payment gateways
  - Credit card processing
  - Payment verification

✓ Enterprise applications
  - SAP
  - Oracle
  - Microsoft Dynamics

✓ High security required
  - Healthcare systems (HIPAA)
  - Government systems
  - Financial institutions

✓ ACID transactions needed
  - Database-like operations
  - Atomic transactions
  - Rollback support

✓ Legacy system integration
  - Old systems
  - Mainframes

Example:
────────
Bank account transfer
→ SOAP ensures transaction integrity! ✓
```

### Real-world Examples

```
┌────────────────────────────────────────────────────┐
│         REAL-WORLD EXAMPLES                        │
└────────────────────────────────────────────────────┘

REST APIs in Bangladesh:
═══════════════════════

1. bKash Payment:
   GET https://api.bkash.com/checkout/payment/create
   Response: JSON with payment URL

2. Pathao:
   POST https://api.pathao.com/api/v1/orders
   Response: Order tracking info in JSON

3. Foodpanda:
   GET https://api.foodpanda.com/restaurants
   Response: Restaurant list in JSON

4. Robi/GP APIs:
   GET https://api.robi.com.bd/balance
   Response: Balance info in JSON


SOAP APIs (Still Used):
══════════════════════

1. Banking:
   Bangladesh Bank interbank transfers
   → SOAP for security & ACID

2. Government:
   e-TIN registration
   → SOAP for legacy integration

3. Airlines:
   Flight booking systems (Amadeus)
   → SOAP for reliability

4. Healthcare:
   Hospital management systems
   → SOAP for HIPAA compliance
```

### Migration Path

```
┌────────────────────────────────────────────────────┐
│      SOAP → REST MIGRATION                         │
└────────────────────────────────────────────────────┘

Many companies migrating:

Before (SOAP):
POST /soap
<soap:Envelope>
  <GetUser>
    <id>123</id>
  </GetUser>
</soap:Envelope>

After (REST):
GET /users/123

Benefits after migration:
- 70% less bandwidth
- 3x faster response
- Easier mobile app development
- Better developer experience

Example:
eBay migrated from SOAP → REST
Twitter migrated from SOAP → REST
Amazon offers both (backward compatibility)
```

### Summary

```
┌────────────────────────────────────────────────────┐
│              QUICK SUMMARY                         │
└────────────────────────────────────────────────────┘

REST:
════
👍 Simple
👍 Fast
👍 JSON
👍 Mobile-friendly
👍 Easy to learn
👍 Modern
👎 Less secure (but fixable)
👎 No ACID transactions

Best for: Web/Mobile apps


SOAP:
════
👍 Very secure
👍 ACID transactions
👍 Enterprise features
👍 Multiple protocols
👎 Complex
👎 Slow
👎 XML only
👎 Hard to learn

Best for: Banking, Enterprise


মনে রাখার সহজ উপায়:
═══════════════════

REST = আধুনিক, দ্রুত, সহজ (Mobile/Web)
SOAP = পুরাতন, নিরাপদ, জটিল (Banking/Enterprise)

আজকাল 90% নতুন projects → REST! ✓
```

---



