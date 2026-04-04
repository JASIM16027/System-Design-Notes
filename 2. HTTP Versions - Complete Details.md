# HTTP Versions - Complete Details

---

## HTTP কী?

### Definition
```
HTTP = HyperText Transfer Protocol
Web এ data transfer করার নিয়ম/protocol
Client (Browser) ও Server এর মধ্যে communication
```

### Basic Concept
```
┌──────────────────────────────────────────────────────┐
│              HTTP Communication                      │
└──────────────────────────────────────────────────────┘

CLIENT (Browser)                    SERVER
     │                                 │
     │  HTTP Request                   │
     │  "আমাকে index.html দাও"        │
     ├────────────────────────────────>│
     │                                 │
     │                          ┌──────▼──────┐
     │                          │ Find & Send │
     │                          │   File      │
     │                          └──────┬──────┘
     │                                 │
     │  HTTP Response                  │
     │  "এই নাও index.html"            │
     │<────────────────────────────────┤
     │                                 │
```

---

## 1. HTTP/1.0 (1996)

### Definition
```
HTTP/1.0 = HTTP এর প্রথম widely-used version
প্রতিটা request এর জন্য নতুন connection
```

### Key Features
```
┌──────────────────────────────────────────────────────┐
│              HTTP/1.0 FEATURES                       │
└──────────────────────────────────────────────────────┘

1. Non-persistent Connection
   ════════════════════════════
   প্রতিটা request-response এর পর connection বন্ধ

2. Simple Request-Response
   ════════════════════════
   শুধু GET method ছিল initially
   
3. No Host Header
   ══════════════
   একটা IP তে একটাই website

4. No Caching
   ══════════
   কোনো built-in cache mechanism নেই
```

### How It Works
```
┌──────────────────────────────────────────────────────┐
│         HTTP/1.0 CONNECTION FLOW                     │
└──────────────────────────────────────────────────────┘

Scenario: একটা webpage load করতে হবে যেখানে আছে:
- 1টা HTML file
- 2টা CSS file  
- 3টা Image file
Total = 6 files

Connection Flow:
═══════════════

Request 1: HTML File
CLIENT                          SERVER
  │                               │
  │ 1. TCP Connection খোলো       │
  ├──────────────────────────────>│
  │ 2. GET /index.html            │
  ├──────────────────────────────>│
  │ 3. Response: index.html       │
  │<──────────────────────────────┤
  │ 4. Connection বন্ধ            │
  ├──────────────────────────────>│
  │                               │

Request 2: CSS File
  │ 5. নতুন TCP Connection        │
  ├──────────────────────────────>│
  │ 6. GET /style.css             │
  ├──────────────────────────────>│
  │ 7. Response: style.css        │
  │<──────────────────────────────┤
  │ 8. Connection বন্ধ            │
  ├──────────────────────────────>│
  │                               │

Request 3: Another CSS
  │ 9. আবার নতুন Connection       │
  ├──────────────────────────────>│
  │ 10. GET /layout.css           │
  ├──────────────────────────────>│
  │ 11. Response: layout.css      │
  │<──────────────────────────────┤
  │ 12. Connection বন্ধ           │
  │                               │

... (এভাবে 6 বার)

Timeline:
════════

File 1: ████░░░░░░░░░░░░ (Connect + Request + Close)
File 2:     ████░░░░░░░░ (Connect + Request + Close)
File 3:         ████░░░░ (Connect + Request + Close)
File 4:             ████ (Connect + Request + Close)
File 5:                 ████ (Connect + Request + Close)
File 6:                     ████ (Connect + Request + Close)

Total Time: অনেক বেশি! ⏱️
```

### HTTP/1.0 Request Example
```
┌──────────────────────────────────────────────────────┐
│          HTTP/1.0 REQUEST                            │
└──────────────────────────────────────────────────────┘

GET /index.html HTTP/1.0
User-Agent: Mozilla/4.0
Accept: text/html

(empty line - request শেষ)


Response:
HTTP/1.0 200 OK
Date: Sat, 07 Feb 2026 10:00:00 GMT
Server: Apache/1.3
Content-Type: text/html
Content-Length: 1234

<html>
  <body>Hello World!</body>
</html>

(connection closes automatically)
```

### Problems with HTTP/1.0
```
┌──────────────────────────────────────────────────────┐
│          HTTP/1.0 PROBLEMS                           │
└──────────────────────────────────────────────────────┘

1. Slow Performance (ধীর গতি)
   ════════════════════════════
   প্রতিটা file এর জন্য নতুন connection
   = অনেক সময় নষ্ট
   
   Connection overhead for 10 files:
   ┌─────────────────────────────────────┐
   │ TCP Handshake: 100ms                │
   │ ×10 files = 1000ms (1 second)       │
   │ শুধু connection এর জন্য!           │
   └─────────────────────────────────────┘

2. Resource Wastage
   ════════════════
   বারবার connection খুলতে ও বন্ধ করতে
   server এর CPU ও memory খরচ

3. No Pipelining
   ══════════════
   একসাথে multiple request পাঠানো যায় না

4. No Virtual Hosting
   ══════════════════
   একটা IP = একটা website
   Host header নেই
```

---

## 2. HTTP/1.1 (1997)

### Definition
```
HTTP/1.1 = HTTP/1.0 এর improved version
Persistent connections, pipelining, virtual hosting added
বর্তমানে সবচেয়ে বেশি ব্যবহৃত
```

### Major Improvements
```
┌──────────────────────────────────────────────────────┐
│          HTTP/1.1 NEW FEATURES                       │
└──────────────────────────────────────────────────────┘

1. Persistent Connections (Keep-Alive)
   ════════════════════════════════════
   একই connection দিয়ে multiple requests

2. Pipelining
   ══════════
   Wait না করে একসাথে multiple requests

3. Host Header
   ═══════════
   একটা IP তে multiple websites

4. Chunked Transfer Encoding
   ═════════════════════════
   বড় file টুকরো টুকরো করে পাঠানো

5. Better Caching
   ══════════════
   Cache-Control, ETag headers

6. More Methods
   ════════════
   PUT, DELETE, OPTIONS, TRACE, CONNECT added

7. Range Requests
   ══════════════
   File এর শুধু একটা অংশ download
```

### 1. Persistent Connections (Keep-Alive)
```
┌──────────────────────────────────────────────────────┐
│       PERSISTENT CONNECTIONS                         │
└──────────────────────────────────────────────────────┘

Definition: একই TCP connection দিয়ে multiple requests

HTTP/1.0 vs HTTP/1.1:
═══════════════════

HTTP/1.0 (Non-persistent):
CLIENT                          SERVER
  │ Connect                       │
  ├──────────────────────────────>│
  │ Request 1                     │
  ├──────────────────────────────>│
  │ Response 1                    │
  │<──────────────────────────────┤
  │ Close                         │
  ├──────────────────────────────>│
  │                               │
  │ Connect (আবার)               │
  ├──────────────────────────────>│
  │ Request 2                     │
  ├──────────────────────────────>│
  │ Response 2                    │
  │<──────────────────────────────┤
  │ Close                         │
  
  প্রতিটা request এ:
  - TCP handshake = 100ms
  - Total waste = 100ms × 6 files = 600ms

HTTP/1.1 (Persistent):
CLIENT                          SERVER
  │ Connect (একবার)              │
  ├──────────────────────────────>│
  │                               │
  │ Request 1                     │
  ├──────────────────────────────>│
  │ Response 1                    │
  │<──────────────────────────────┤
  │                               │
  │ Request 2 (same connection)   │
  ├──────────────────────────────>│
  │ Response 2                    │
  │<──────────────────────────────┤
  │                               │
  │ Request 3 (same connection)   │
  ├──────────────────────────────>│
  │ Response 3                    │
  │<──────────────────────────────┤
  │                               │
  │ ... (more requests)           │
  │                               │
  │ Close (শেষে)                 │
  
  শুধু একবার handshake = 100ms
  Time saved = 500ms! ⚡

Headers:
Connection: keep-alive          (HTTP/1.0 তে manually)
Connection: close               (বন্ধ করতে চাইলে)

HTTP/1.1 তে default = keep-alive
```

### 2. HTTP Pipelining
```
┌──────────────────────────────────────────────────────┐
│              PIPELINING                              │
└──────────────────────────────────────────────────────┘

Definition: Response এর অপেক্ষা না করে 
           একের পর এক request পাঠানো

Without Pipelining (Normal):
CLIENT                          SERVER
  │ Request 1                     │
  ├──────────────────────────────>│
  │ (wait...)                     │
  │                        ┌──────▼──────┐
  │                        │  Process    │
  │                        └──────┬──────┘
  │ Response 1                    │
  │<──────────────────────────────┤
  │                               │
  │ Request 2 (এখন পাঠাচ্ছি)     │
  ├──────────────────────────────>│
  │ (wait...)                     │
  │                        ┌──────▼──────┐
  │                        │  Process    │
  │                        └──────┬──────┘
  │ Response 2                    │
  │<──────────────────────────────┤

Timeline:
Req1 ████──────────
Res1     ──────████
Req2         ████──────────
Res2             ──────████
Total: ████████████████████


With Pipelining:
CLIENT                          SERVER
  │ Request 1                     │
  ├──────────────────────────────>│
  │ Request 2 (wait না করে)      │
  ├──────────────────────────────>│
  │ Request 3 (wait না করে)      │
  ├──────────────────────────────>│
  │                               │
  │                        ┌──────▼──────┐
  │                        │  Process    │
  │                        │  All Three  │
  │                        └──────┬──────┘
  │                               │
  │ Response 1                    │
  │<──────────────────────────────┤
  │ Response 2                    │
  │<──────────────────────────────┤
  │ Response 3                    │
  │<──────────────────────────────┤

Timeline:
Req1 ██
Req2   ██
Req3     ██
Res1       ████
Res2           ████
Res3               ████
Total: ██████████████ (কম সময়!)

সুবিধা:
- Network latency কমে
- Fast page load

সমস্যা:
- Head-of-line blocking
  (প্রথম response আটকে গেলে সব আটকে যায়)
- বেশিরভাগ browser disable করে রেখেছে
```

### 3. Host Header (Virtual Hosting)
```
┌──────────────────────────────────────────────────────┐
│              HOST HEADER                             │
└──────────────────────────────────────────────────────┘

Definition: একটা IP address এ multiple websites

HTTP/1.0 এ:
══════════
একটা server (IP: 192.168.1.1):
- শুধু একটা website serve করতে পারতো

HTTP/1.1 এ:
══════════
একটা server (IP: 192.168.1.1):
- example.com
- test.com
- demo.com
- সব একসাথে!

How It Works:
════════════

Server: 192.168.1.1
        │
        ├─── example.com (files in /var/www/example)
        ├─── test.com    (files in /var/www/test)
        └─── demo.com    (files in /var/www/demo)

Request 1:
GET / HTTP/1.1
Host: example.com        ← এই header বলছে কোন site

Server: "Ah, example.com চাইছে"
Response: /var/www/example/index.html


Request 2:
GET / HTTP/1.1
Host: test.com          ← ভিন্ন site

Server: "Ah, test.com চাইছে"
Response: /var/www/test/index.html

সুবিধা:
- Shared hosting সম্ভব
- একটা server এ অনেক websites
- IP address বাঁচে
```

### 4. Chunked Transfer Encoding
```
┌──────────────────────────────────────────────────────┐
│        CHUNKED TRANSFER ENCODING                     │
└──────────────────────────────────────────────────────┘

Definition: বড় file টুকরো টুকরো করে পাঠানো
           Total size জানার আগেই পাঠানো শুরু করা যায়

Problem Without Chunking:
════════════════════════

Large File (10MB):
1. পুরো file generate করো
2. Total size calculate করো  
3. Content-Length header দাও
4. তারপর পাঠাও

User: ⏳ অপেক্ষা করছে...

Solution With Chunking:
══════════════════════

Response Header:
HTTP/1.1 200 OK
Transfer-Encoding: chunked    ← এই header

Response Body:
5\r\n              ← chunk size (5 bytes)
Hello\r\n          ← chunk data
6\r\n              ← next chunk size
 World\r\n         ← chunk data
E\r\n              ← chunk size (14 bytes)
! How are you?\r\n ← chunk data
0\r\n              ← size 0 = শেষ
\r\n               ← empty line

Flow Diagram:
════════════

SERVER                          CLIENT
  │                               │
  │ Chunk 1: "Hello"              │
  ├──────────────────────────────>│
  │                               │ Display: "Hello"
  │ Chunk 2: " World"             │
  ├──────────────────────────────>│
  │                               │ Display: "Hello World"
  │ Chunk 3: "! How are you?"     │
  ├──────────────────────────────>│
  │                               │ Display: "Hello World! How..."
  │ Chunk 4: (size 0 - শেষ)      │
  ├──────────────────────────────>│
  │                               │ Complete!

Use Case:
════════
- Live streaming
- Real-time data
- Dynamic content generation
- Server-sent events
```

### 5. Better Caching
```
┌──────────────────────────────────────────────────────┐
│              CACHING MECHANISM                       │
└──────────────────────────────────────────────────────┘

Cache Headers:
═════════════

1. Cache-Control:
   ═══════════════
   Cache behavior নিয়ন্ত্রণ করে

   Response:
   Cache-Control: max-age=3600      (1 hour cache)
   Cache-Control: no-cache          (must revalidate)
   Cache-Control: no-store          (don't cache)
   Cache-Control: public            (any cache can store)
   Cache-Control: private           (শুধু browser cache)

2. ETag (Entity Tag):
   ══════════════════
   Resource এর version identifier

   First Request:
   CLIENT                          SERVER
     │ GET /image.jpg                │
     ├──────────────────────────────>│
     │                               │
     │ 200 OK                        │
     │ ETag: "abc123"                │
     │ (image data)                  │
     │<──────────────────────────────┤
     │                               │
     │ Store: ETag="abc123"          │
   
   Second Request (পরে):
     │ GET /image.jpg                │
     │ If-None-Match: "abc123"       │
     ├──────────────────────────────>│
     │                               │
     │ Check: ETag still "abc123"?   │
     │ Yes! File unchanged           │
     │                               │
     │ 304 Not Modified              │
     │ (no image data)               │
     │<──────────────────────────────┤
     │                               │
     │ Use cached version            │

   Benefits:
   - Bandwidth বাঁচে (304 response ছোট)
   - Fast loading

3. Last-Modified / If-Modified-Since:
   ═══════════════════════════════════
   
   First Request:
   Response:
   Last-Modified: Sat, 01 Feb 2026 10:00:00 GMT
   
   Second Request:
   Request:
   If-Modified-Since: Sat, 01 Feb 2026 10:00:00 GMT
   
   Response:
   304 Not Modified (যদি unchanged হয়)

Cache Flow:
══════════

Request → Check Cache → Found & Valid? 
                           │
                ┌──────────┴──────────┐
                │                     │
               Yes                   No
                │                     │
          Use Cache          Request Server
                                     │
                              Update Cache
```

### 6. Range Requests
```
┌──────────────────────────────────────────────────────┐
│            RANGE REQUESTS                            │
└──────────────────────────────────────────────────────┘

Definition: File এর শুধু একটা অংশ download করা

Use Cases:
═════════
- Resume broken downloads
- Video streaming (seek করা)
- PDF এর specific pages

How It Works:
════════════

File: video.mp4 (100MB)
Bytes: 0 to 104,857,599

Request 1: প্রথম 10MB
GET /video.mp4 HTTP/1.1
Range: bytes=0-10485759      ← First 10MB চাই

Response:
HTTP/1.1 206 Partial Content
Content-Range: bytes 0-10485759/104857600
Content-Length: 10485760

(10MB data)


Request 2: পরের 10MB
GET /video.mp4 HTTP/1.1
Range: bytes=10485760-20971519   ← Next 10MB

Response:
HTTP/1.1 206 Partial Content
Content-Range: bytes 10485760-20971519/104857600
Content-Length: 10485760

(next 10MB data)

Resume Download Example:
═══════════════════════

Download started: 0-50MB ✓
Connection lost! ✗

Resume:
GET /largefile.zip HTTP/1.1
Range: bytes=52428800-        ← From 50MB to end

Response:
HTTP/1.1 206 Partial Content
Content-Range: bytes 52428800-104857599/104857600

(remaining 50MB)

Video Streaming:
═══════════════

User seeks to 5:00 minute mark

Calculate: 5 minutes = byte position X

Request:
GET /movie.mp4 HTTP/1.1
Range: bytes=X-

Start playing from that position!
```

### HTTP/1.1 Complete Request Example
```
┌──────────────────────────────────────────────────────┐
│          HTTP/1.1 REQUEST EXAMPLE                    │
└──────────────────────────────────────────────────────┘

Request:
GET /api/users/123 HTTP/1.1
Host: api.example.com                    ← Required!
User-Agent: Mozilla/5.0
Accept: application/json
Accept-Language: en-US,en;q=0.9
Accept-Encoding: gzip, deflate, br
Connection: keep-alive                   ← Persistent
Cache-Control: no-cache
If-None-Match: "abc123"                 ← ETag
Authorization: Bearer token123

Response:
HTTP/1.1 200 OK
Date: Sat, 07 Feb 2026 10:30:00 GMT
Server: nginx/1.20.1
Content-Type: application/json
Content-Length: 234
Connection: keep-alive                   ← Keep connection
Cache-Control: max-age=3600
ETag: "abc123"
Last-Modified: Sat, 07 Feb 2026 09:00:00 GMT

{
  "id": 123,
  "name": "Rahim Ahmed",
  "email": "rahim@example.com"
}
```

### HTTP/1.1 Problems
```
┌──────────────────────────────────────────────────────┐
│          HTTP/1.1 LIMITATIONS                        │
└──────────────────────────────────────────────────────┘

1. Head-of-Line Blocking
   ═══════════════════════
   
   Browser opens 6 connections per domain
   
   Connection 1:
   Req1(slow) ████████████──────────
   Req2       wait...    ████
   Req3       wait...        ████
   
   যদি Req1 ধীর হয়, Req2 ও Req3 অপেক্ষা করবে!

2. Header Overhead
   ═══════════════
   
   প্রতিটা request এ same headers বারবার:
   
   Request 1:
   Host: example.com
   User-Agent: Mozilla/5.0 Chrome/120...
   Accept: text/html,application/xhtml+xml...
   Accept-Language: en-US,en;q=0.9
   Cookie: session=abc123; user=xyz; ...
   (500+ bytes)
   
   Request 2:
   Host: example.com
   User-Agent: Mozilla/5.0 Chrome/120...
   Accept: text/html,application/xhtml+xml...
   (same 500+ bytes আবার!)
   
   Waste! 🗑️

3. Uncompressed Headers
   ═══════════════════
   Headers plain text এ পাঠানো হয়
   Compression নেই

4. Limited Parallelism
   ═══════════════════
   শুধু 6 connections per domain
   
   100 resources load করতে হলে:
   - 6 parallel
   - বাকি 94 অপেক্ষা করবে
   
   Queue:
   [1][2][3][4][5][6] → loading
   [7][8][9]...[100]  → waiting ⏳

5. No Server Push
   ══════════════
   Server নিজে থেকে কিছু পাঠাতে পারে না
   Client request করা ছাড়া
```

---

## 3. HTTP/2 (2015)

### Definition
```
HTTP/2 = HTTP/1.1 এর major upgrade
Binary protocol, multiplexing, server push
Google এর SPDY protocol based on
```

### Major Features
```
┌──────────────────────────────────────────────────────┐
│            HTTP/2 FEATURES                           │
└──────────────────────────────────────────────────────┘

1. Binary Protocol
   ═══════════════
   Text এর বদলে binary format

2. Multiplexing
   ═══════════
   একই connection এ multiple requests parallel

3. Header Compression (HPACK)
   ═══════════════════════════
   Headers compress করা

4. Server Push
   ═══════════
   Client চাওয়ার আগেই resources পাঠানো

5. Stream Prioritization
   ══════════════════════
   Important resources আগে

6. Single Connection
   ═════════════════
   Per domain শুধু একটা connection
```

### 1. Binary Protocol
```
┌──────────────────────────────────────────────────────┐
│           BINARY vs TEXT                             │
└──────────────────────────────────────────────────────┘

HTTP/1.1 (Text-based):
═══════════════════════

GET /index.html HTTP/1.1\r\n
Host: example.com\r\n
User-Agent: Mozilla/5.0\r\n
\r\n

Human readable ✓
Easy to debug ✓
But slower to parse ✗


HTTP/2 (Binary):
═══════════════

01001000 01010100 01010100 01010000  (HTTP)
00101111 00110010 00101110 00110000  (2.0)
...binary frames...

Human readable ✗
Hard to debug ✗
But much faster to parse ✓
Less errors ✓
Smaller size ✓

Binary Frames:
═════════════

HTTP/2 Message = Frames

Frame Structure:
┌─────────────────────────────────────┐
│ Length (24 bits)                    │
│ Type (8 bits)                       │
│ Flags (8 bits)                      │
│ Stream ID (32 bits)                 │
│ Frame Payload                       │
└─────────────────────────────────────┘

Frame Types:
- DATA: actual content
- HEADERS: HTTP headers
- PRIORITY: stream priority
- RST_STREAM: cancel stream
- SETTINGS: connection settings
- PUSH_PROMISE: server push
- PING: keep-alive
- GOAWAY: close connection
```

### 2. Multiplexing (সবচেয়ে গুরুত্বপূর্ণ!)
```
┌──────────────────────────────────────────────────────┐
│              MULTIPLEXING                            │
└──────────────────────────────────────────────────────┘

Definition: একই connection দিয়ে একসাথে 
           multiple requests/responses

HTTP/1.1 (Without Multiplexing):
════════════════════════════════

6 Connections needed:

Conn1: ████──────────── (HTML)
Conn2:     ████──────── (CSS)
Conn3:         ████──── (JS)
Conn4:             ████ (Image1)
Conn5:                 ████ (Image2)
Conn6:                     ████ (Image3)

Problems:
- Multiple TCP connections = overhead
- Head-of-line blocking
- Limited to 6 connections


HTTP/2 (With Multiplexing):
═══════════════════════════

Single Connection, Multiple Streams:

Connection 1:
Stream 1 (HTML):   ██░░██░░██
Stream 2 (CSS):    ░░██░░██░░
Stream 3 (JS):     ██░░██░░██
Stream 4 (Img1):   ░░██░░██░░
Stream 5 (Img2):   ██░░██░░██
Stream 6 (Img3):   ░░██░░██░░

All interleaved! 🎯

Detailed Flow:
═════════════

CLIENT                              SERVER
  │                                   │
  │ Single TCP Connection             │
  ├──────────────────────────────────>│
  │                                   │
  │ Stream 1: GET /index.html         │
  │ Stream 2: GET /style.css          │
  │ Stream 3: GET /app.js             │
  │ Stream 4: GET /image1.jpg         │
  ├──────────────────────────────────>│
  │    (all requests simultaneously)  │
  │                                   │
  │              ┌────────────────────┤
  │              │ Process all streams│
  │              │ in parallel        │
  │              └────────┬───────────┤
  │                       │           │
  │ Stream 1: Response    │           │
  │<──────────────────────┴───────────┤
  │ Stream 3: Response (faster!)      │
  │<──────────────────────────────────┤
  │ Stream 2: Response                │
  │<──────────────────────────────────┤
  │ Stream 4: Response                │
  │<──────────────────────────────────┤

Benefits:
- No head-of-line blocking
- Better resource utilization
- Faster page loads
- শুধু একটা connection = কম overhead

Example Timeline:
════════════════

HTTP/1.1:
Total Time: ████████████████████ (20 units)

HTTP/2:
Total Time: ██████████ (10 units)

50% faster! 🚀
```

### 3. Header Compression (HPACK)
```
┌──────────────────────────────────────────────────────┐
│          HEADER COMPRESSION (HPACK)                  │
└──────────────────────────────────────────────────────┘

Problem in HTTP/1.1:
═══════════════════

Request 1:
GET /page1.html HTTP/1.1
Host: example.com
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64...)
Accept: text/html,application/xhtml+xml,application...
Accept-Language: en-US,en;q=0.9
Accept-Encoding: gzip, deflate, br
Cookie: session=abc123; user_id=456; preferences=xyz
Connection: keep-alive
(Total: ~800 bytes)

Request 2:
GET /page2.html HTTP/1.1
Host: example.com                              (same)
User-Agent: Mozilla/5.0 (Windows NT 10.0...)   (same)
Accept: text/html,application/xhtml+xml...     (same)
Accept-Language: en-US,en;q=0.9                (same)
Accept-Encoding: gzip, deflate, br             (same)
Cookie: session=abc123; user_id=456...         (same)
Connection: keep-alive                         (same)
(আবার ~800 bytes! Duplicate!)

Solution: HPACK
══════════════

Static Table (Pre-defined):
┌───────┬──────────────────────┬─────────────┐
│ Index │ Header Name          │ Value       │
├───────┼──────────────────────┼─────────────┤
│ 1     │ :authority           │             │
│ 2     │ :method              │ GET         │
│ 3     │ :method              │ POST        │
│ 4     │ :path                │ /           │
│ 8     │ :status              │ 200         │
│ 15    │ accept-encoding      │ gzip,deflate│
│ 28    │ content-length       │             │
└───────┴──────────────────────┴─────────────┘

Dynamic Table (Built during connection):
┌───────┬──────────────────────┬─────────────┐
│ Index │ Header Name          │ Value       │
├───────┼──────────────────────┼─────────────┤
│ 62    │ host                 │ example.com │
│ 63    │ cookie               │ session=... │
│ 64    │ user-agent           │ Mozilla/5.0.│
└───────┴──────────────────────┴─────────────┘

Request 1 (First time):
Host: example.com              → Store in dynamic[62]
User-Agent: Mozilla/5.0...     → Store in dynamic[64]
Cookie: session=abc123...      → Store in dynamic[63]
(Send full headers = 800 bytes)

Request 2 (Subsequent):
Instead of sending full headers, send indices:
:method = 2          (GET from static table)
:path = /page2.html  (new value)
host = 62            (from dynamic table)
user-agent = 64      (from dynamic table)
cookie = 63          (from dynamic table)
(Send only ~50 bytes! 🎉)

Compression Example:
═══════════════════

Before HPACK:
Request 1: 800 bytes
Request 2: 800 bytes
Request 3: 800 bytes
Total: 2400 bytes

After HPACK:
Request 1: 800 bytes (first time full)
Request 2: 50 bytes  (indices)
Request 3: 50 bytes  (indices)
Total: 900 bytes

Saved: 1500 bytes (62% reduction!) 📉
```

### 4. Server Push
```
┌──────────────────────────────────────────────────────┐
│              SERVER PUSH                             │
└──────────────────────────────────────────────────────┘

Definition: Client চাওয়ার আগেই Server resources পাঠায়

HTTP/1.1 (Traditional):
══════════════════════

CLIENT                              SERVER
  │                                   │
  │ 1. GET /index.html                │
  ├──────────────────────────────────>│
  │                                   │
  │ 2. index.html                     │
  │    (mentions style.css, app.js)   │
  │<──────────────────────────────────┤
  │                                   │
  │ 3. Parse HTML                     │
  │    Found: style.css needed        │
  │                                   │
  │ 4. GET /style.css                 │
  ├──────────────────────────────────>│
  │ (wasted time ⏱️)                  │
  │                                   │
  │ 5. style.css                      │
  │<──────────────────────────────────┤
  │                                   │
  │ 6. Parse more...                  │
  │    Found: app.js needed           │
  │                                   │
  │ 7. GET /app.js                    │
  ├──────────────────────────────────>│
  │ (more wasted time ⏱️)             │
  │                                   │
  │ 8. app.js                         │
  │<──────────────────────────────────┤

Timeline:
Req HTML: ████
Res HTML:     ████
Req CSS:          ████
Res CSS:              ████
Req JS:                   ████
Res JS:                       ████
Total: ████████████████████████


HTTP/2 (With Server Push):
══════════════════════════

CLIENT                              SERVER
  │                                   │
  │ 1. GET /index.html                │
  ├──────────────────────────────────>│
  │                                   │
  │                            ┌──────▼──────┐
  │                            │ "index.html │
  │                            │  needs CSS  │
  │                            │  and JS!"   │
  │                            └──────┬──────┘
  │                                   │
  │ 2. PUSH_PROMISE: style.css        │
  │    "আমি style.css পাঠাচ্ছি"       │
  │<──────────────────────────────────┤
  │                                   │
  │ 3. PUSH_PROMISE: app.js           │
  │    "আমি app.js পাঠাচ্ছি"          │
  │<──────────────────────────────────┤
  │                                   │
  │ 4. index.html (Stream 1)          │
  │<──────────────────────────────────┤
  │                                   │
  │ 5. style.css (Stream 2 - pushed)  │
  │<──────────────────────────────────┤
  │                                   │
  │ 6. app.js (Stream 3 - pushed)     │
  │<──────────────────────────────────┤
  │                                   │
  │ Already have everything! ✓        │

Timeline:
Req HTML: ████
Push CSS:     ████ (no request needed!)
Push JS:          ████ (no request needed!)
Res HTML:             ████
Res CSS:                  ████
Res JS:                       ████
Total: ████████████████ (faster!)

Server Push Example:
═══════════════════

Server config:
"When someone requests /index.html,
 automatically push:
 - /style.css
 - /app.js
 - /logo.png"

Benefits:
- Eliminates round trips
- Faster page load
- Better user experience

Control:
Client can refuse pushed resources:
RST_STREAM (if already cached)
```

### 5. Stream Prioritization
```
┌──────────────────────────────────────────────────────┐
│          STREAM PRIORITIZATION                       │
└──────────────────────────────────────────────────────┘

Definition: Important resources আগে load হবে

Priority Tree:
═════════════

                 HTML (Priority: Highest)
                   │
        ┌──────────┼──────────┐
        │          │          │
      CSS        JS      Images
   (High)     (Medium)   (Low)
      │
   ┌──┴──┐
  Font  Icon
 (High) (Medium)

Example:
═══════

Without Prioritization:
Stream 1 (HTML):    ██░░██░░██
Stream 2 (Image):   ░░██░░██░░
Stream 3 (CSS):     ██░░██░░██
Stream 4 (Logo):    ░░██░░██░░
Stream 5 (JS):      ██░░██░░██

All equal → Slow render


With Prioritization:
Stream 1 (HTML):    ████████░░  (Highest - 100%)
Stream 3 (CSS):     ░░██████░░  (High - 80%)
Stream 5 (JS):      ░░░░████░░  (Medium - 60%)
Stream 2 (Image):   ░░░░░░████  (Low - 40%)
Stream 4 (Logo):    ░░░░░░░░██  (Lowest - 20%)

Critical path first → Fast render! 🚀

Weight-based:
═══════════

HEADERS frame:
Stream 3 (CSS):    weight = 16
Stream 5 (JS):     weight = 8
Stream 2 (Image):  weight = 4

Bandwidth distribution:
CSS:   16/28 = 57%
JS:    8/28  = 29%
Image: 4/28  = 14%
```

### HTTP/2 Complete Example
```
┌──────────────────────────────────────────────────────┐
│          HTTP/2 CONNECTION FLOW                      │
└──────────────────────────────────────────────────────┘

1. Connection Setup:
   ═════════════════

CLIENT                              SERVER
  │                                   │
  │ TCP Connection                    │
  ├──────────────────────────────────>│
  │                                   │
  │ TLS Handshake (HTTPS required)    │
  ├──────────────────────────────────>│
  │                                   │
  │ Connection Preface:               │
  │ "PRI * HTTP/2.0\r\n\r\nSM\r\n\r\n"│
  ├──────────────────────────────────>│
  │                                   │
  │ SETTINGS frame exchange           │
  ├<─────────────────────────────────>│
  │                                   │
  │ HTTP/2 connection established ✓   │


2. Multiple Requests (Multiplexed):
   ════════════════════════════════

  │ HEADERS frame (Stream 1)          │
  │ :method: GET                      │
  │ :path: /index.html                │
  │ :scheme: https                    │
  │ :authority: example.com           │
  ├──────────────────────────────────>│
  │                                   │
  │ HEADERS frame (Stream 3)          │
  │ :method: GET                      │
  │ :path: /style.css                 │
  ├──────────────────────────────────>│
  │                                   │
  │ HEADERS frame (Stream 5)          │
  │ :method: GET                      │
  │ :path: /app.js                    │
  ├──────────────────────────────────>│
  │   (all sent immediately)          │
  │                                   │

3. Server Push:
   ════════════

  │ PUSH_PROMISE (Stream 2)           │
  │ (pushed for Stream 1)             │
  │ Will push: /logo.png              │
  │<──────────────────────────────────┤
  │                                   │

4. Responses (Interleaved):
   ════════════════════════

  │ HEADERS (Stream 1)                │
  │ :status: 200                      │
  │<──────────────────────────────────┤
  │                                   │
  │ DATA (Stream 1, partial)          │
  │<──────────────────────────────────┤
  │                                   │
  │ DATA (Stream 3, partial)          │
  │<──────────────────────────────────┤
  │                                   │
  │ DATA (Stream 1, more)             │
  │<──────────────────────────────────┤
  │                                   │
  │ DATA (Stream 5, partial)          │
  │<──────────────────────────────────┤
  │                                   │
  │ DATA (Stream 2, pushed logo)      │
  │<──────────────────────────────────┤
  │                                   │
  │ ... frames interleaved ...        │
```

---

## HTTP Versions Comparison

### Complete Comparison Table
```
┌──────────────────────────────────────────────────────┐
│          HTTP/1.0 vs HTTP/1.1 vs HTTP/2              │
└──────────────────────────────────────────────────────┘

┌─────────────────┬──────────┬──────────┬──────────┐
│ Feature         │ HTTP/1.0 │ HTTP/1.1 │ HTTP/2   │
├─────────────────┼──────────┼──────────┼──────────┤
│ Year            │ 1996     │ 1997     │ 2015     │
├─────────────────┼──────────┼──────────┼──────────┤
│ Connection      │ Close    │ Keep-    │ Persist- │
│                 │ after    │ Alive    │ ent +    │
│                 │ each     │ (default)│ Multiplex│
├─────────────────┼──────────┼──────────┼──────────┤
│ Protocol        │ Text     │ Text     │ Binary   │
├─────────────────┼──────────┼──────────┼──────────┤
│ Multiplexing    │ ✗        │ ✗        │ ✓        │
├─────────────────┼──────────┼──────────┼──────────┤
│ Header          │ ✗        │ ✗        │ ✓ (HPACK)│
│ Compression     │          │          │          │
├─────────────────┼──────────┼──────────┼──────────┤
│ Server Push     │ ✗        │ ✗        │ ✓        │
├─────────────────┼──────────┼──────────┼──────────┤
│ Prioritization  │ ✗        │ ✗        │ ✓        │
├─────────────────┼──────────┼──────────┼──────────┤
│ Connections     │ Many     │ 6 per    │ 1 per    │
│ per domain      │          │ domain   │ domain   │
├─────────────────┼──────────┼──────────┼──────────┤
│ HOL Blocking    │ ✓        │ ✓        │ ✗        │
├─────────────────┼──────────┼──────────┼──────────┤
│ TLS Required    │ ✗        │ ✗        │ ✓ (browsers)
├─────────────────┼──────────┼──────────┼──────────┤
│ Speed           │ Slow     │ Medium   │ Fast     │
│                 │ ░░░░     │ ████░░   │ ████████ │
└─────────────────┴──────────┴──────────┴──────────┘
```

### Performance Comparison
```
┌──────────────────────────────────────────────────────┐
│          PERFORMANCE COMPARISON                      │
└──────────────────────────────────────────────────────┘

Scenario: Load a webpage with:
- 1 HTML file
- 2 CSS files
- 3 JS files
- 10 images
Total: 16 files

HTTP/1.0:
════════
16 files × (100ms connection + 50ms transfer)
= 16 × 150ms
= 2400ms (2.4 seconds) ⏱️

Timeline:
File 1:  ████████
File 2:          ████████
File 3:                  ████████
...
File 16:                         ████████
Total:   ████████████████████████████████

HTTP/1.1:
════════
1 connection (100ms)
+ 16 files × 50ms transfer
= 100ms + 800ms
= 900ms (0.9 seconds) ⚡

Timeline:
Connect: ████
File 1:      ████
File 2:          ████
File 3:              ████
...
Total:   ████████████████████

HTTP/2:
══════
1 connection (100ms)
+ Multiplexed transfer (all parallel)
= 100ms + 50ms (slowest file)
= 150ms (0.15 seconds) 🚀

Timeline:
Connect: ████
Files:       ████ (all parallel!)
Total:   ████████

Speed Improvement:
HTTP/1.0 → HTTP/1.1: 62% faster
HTTP/1.1 → HTTP/2:   83% faster
HTTP/1.0 → HTTP/2:   94% faster! 🎉
```

---

## Real World Example

### Complete Page Load
```
┌──────────────────────────────────────────────────────┐
│      REAL WEBSITE LOAD: news.example.com            │
└──────────────────────────────────────────────────────┘

Resources needed:
- index.html (10KB)
- style.css (50KB)
- main.js (100KB)
- jquery.js (80KB)
- logo.png (20KB)
- header-img.jpg (150KB)
- article-img1.jpg (200KB)
- article-img2.jpg (180KB)
- article-img3.jpg (220KB)
- ads.js (60KB)

HTTP/1.1 Flow:
═════════════

1. DNS Lookup:        50ms
2. TCP Connect:       100ms
3. TLS Handshake:     100ms
4. Request HTML:      20ms
5. Receive HTML:      30ms
   Parse HTML...
   Found CSS, JS needed!
6. Request CSS:       20ms
7. Receive CSS:       50ms
8. Request JS:        20ms
9. Receive JS:        100ms
   Parse more...
   Found images!
10. Request Img1:     20ms
11. Receive Img1:     200ms
12. Request Img2:     20ms
13. Receive Img2:     180ms
... (continues)

Total Time: ~2.5 seconds


HTTP/2 Flow:
═══════════

1. DNS Lookup:        50ms
2. TCP Connect:       100ms
3. TLS Handshake:     100ms
4. Request HTML:      20ms
5. Receive HTML:      30ms

   Server sees HTML references CSS/JS
   → Pushes CSS, JS immediately!

6. Receive CSS (pushed):  50ms (parallel)
7. Receive JS (pushed):   100ms (parallel)

   Parse HTML
   Found images!

8. Request all images (multiplexed)
9. Receive all (parallel): 220ms (slowest)

Total Time: ~0.8 seconds

Improvement: 68% faster! 📈
```

---

## Migration & Compatibility

```
┌──────────────────────────────────────────────────────┐
│          MIGRATION & COMPATIBILITY                   │
└──────────────────────────────────────────────────────┘

How HTTP/2 Negotiation Works:
═════════════════════════════

CLIENT                              SERVER
  │                                   │
  │ TLS Handshake                     │
  │ ALPN (Application Layer Protocol  │
  │       Negotiation)                │
  │ Supported: h2, http/1.1           │
  ├──────────────────────────────────>│
  │                                   │
  │                            ┌──────▼──────┐
  │                            │ Check: Do I │
  │                            │ support h2? │
  │                            └──────┬──────┘
  │                                   │
  │ Selected: h2 (HTTP/2)             │
  │<──────────────────────────────────┤
  │                                   │
  │ Use HTTP/2 ✓                      │

If server doesn't support HTTP/2:
  │ Selected: http/1.1                │
  │<──────────────────────────────────┤
  │                                   │
  │ Fallback to HTTP/1.1 ✓            │

Browser Support:
═══════════════

✓ Chrome 40+ (2015)
✓ Firefox 36+ (2015)
✓ Safari 9+ (2015)
✓ Edge (all versions)
✓ Opera 27+ (2015)

Server Support:
══════════════

✓ nginx 1.9.5+
✓ Apache 2.4.17+
✓ IIS 10+
✓ Node.js (http2 module)
✓ Cloudflare
✓ AWS CloudFront

Current Usage (2026):
════════════════════

Websites using HTTP/2: ~65%
Websites using HTTP/1.1: ~35%
HTTP/2 is now standard! ✓
```

---

## Quick Reference

```
┌──────────────────────────────────────────────────────┐
│          HTTP VERSIONS QUICK REFERENCE               │
└──────────────────────────────────────────────────────┘

HTTP/1.0 (1996):
═══════════════
✓ Simple
✓ Easy to debug
✗ Slow (new connection each request)
✗ No keep-alive
✗ Limited features

Use: Legacy systems only


HTTP/1.1 (1997):
═══════════════
✓ Keep-alive connections
✓ Pipelining
✓ Host header (virtual hosting)
✓ Chunked transfer
✓ Better caching
✗ Head-of-line blocking
✗ Header overhead
✗ Limited parallelism (6 connections)

Use: Still widely used, default fallback


HTTP/2 (2015):
═════════════
✓ Binary protocol
✓ Multiplexing (parallel requests)
✓ Header compression (HPACK)
✓ Server push
✓ Stream prioritization
✓ Single connection
✓ Much faster
✗ Requires TLS (browsers)
✗ More complex debugging

Use: Modern websites (recommended!)


মনে রাখার সহজ উপায়:
═══════════════════

HTTP/1.0 → একটা লিফট, একজন করে
HTTP/1.1 → একটা লিফট, বারবার উঠানামা
HTTP/2 → একটা বড় লিফট, সবাই একসাথে!

Speed:
HTTP/1.0: 🐌 (very slow)
HTTP/1.1: 🚶 (medium)
HTTP/2:   🚀 (fast!)
```

---
