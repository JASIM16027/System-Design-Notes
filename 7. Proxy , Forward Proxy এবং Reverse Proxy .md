চলো তিনটা concept একটার পর একটা বুঝি — আগে mental model তৈরি করি, তারপর technical details।

---

## Proxy মানে কী?

Proxy মানে হলো **মাঝখানের লোক।** তুমি সরাসরি কারো সাথে কথা না বলে একজন intermediary-র মাধ্যমে কথা বলছ। এই intermediary-ই হলো Proxy।

কিন্তু "মাঝখানের লোক" দুই রকম হতে পারে — সে তোমার পক্ষে কাজ করতে পারে, অথবা server-এর পক্ষে। এই পার্থক্যটাই Forward Proxy আর Reverse Proxy-কে আলাদা করে।

## ১. Forward Proxy — Client-এর পক্ষে কাজ কর

<img width="709" height="286" alt="image" src="https://github.com/user-attachments/assets/de1982a7-3e19-4162-b712-195439174603" />


Forward Proxy client-এর সামনে বসে। Client জানে proxy আছে, এবং ইচ্ছাকৃতভাবে সব request proxy-র মাধ্যমে পাঠায়। Server কখনো জানে না আসল client কে — সে শুধু proxy-র IP দেখে।


Technical ভাবে কী হয়: তুমি `curl --proxy http://proxy:8080 https://google.com` করলে তোমার request proxy-তে যায়, proxy তার নিজের IP দিয়ে Google-কে request করে, Google-এর response proxy cache করে তোমাকে ফেরত দেয়। তোমার real IP `192.168.x.x` Google কখনো দেখে না। Corporate office-এ এই কারণেই Facebook block করা যায় — forward proxy সব employee-র traffic inspect করে নির্দিষ্ট site block করতে পারে।

---

## ২. Reverse Proxy — Server-এর পক্ষে কাজ কর

<img width="718" height="341" alt="image" src="https://github.com/user-attachments/assets/d1c3b2fd-99e9-4d55-8627-f22e39b5a2bf" />


এখন দিকটা উল্টো। Reverse Proxy server-এর সামনে বসে। Client জানে না এর পেছনে কতগুলো server আছে।Client শুধু দেখে `api.example.com` — সে জানে না পেছনে ৩টা app server আছে। Reverse proxy সিদ্ধান্ত নেয় কোন server-এ request পাঠাবে। SSL termination এখানেই হয় — মানে HTTPS decrypt করা, backend server plaintext HTTP পায়। Client-এর real IP `X-Forwarded-For` header-এ backend-এ পাঠানো হয়।

Nginx config-এ এটা এভাবে দেখায়:

```nginx
server {
    listen 443 ssl;
    server_name api.example.com;

    location / {
        proxy_pass http://backend_pool;
        proxy_set_header X-Forwarded-For $remote_addr;
        proxy_set_header Host $host;
    }
}

upstream backend_pool {
    least_conn;              # load balancing algorithm
    server 10.0.0.1:8001;
    server 10.0.0.2:8002;
    server 10.0.0.3:8003;
}
```

---

## ৩. পাশাপাশি তুলনা — মূল পার্থক্যটা বোঝো---

## ৪. Real traffic-এ HTTP header-এ কী দেখায়

এটা সবচেয়ে important technical detail। দুই ধরনের proxy-তে header কীভাবে আলাদা হয়:

**Forward Proxy-তে** — Server যে request পায়:
```
GET /index.html HTTP/1.1
Host: google.com
Via: 1.1 squid-proxy.company.com   ← proxy-র trace
X-Forwarded-For: 192.168.1.50      ← client-এর real IP (optional, proxy add করতেও পারে না-ও পারে)
```

**Reverse Proxy-তে** — Backend server যে request পায়:
```
GET /api/users HTTP/1.1
Host: api.example.com
X-Forwarded-For: 203.0.113.45      ← client-এর original IP
X-Real-IP: 203.0.113.45
X-Forwarded-Proto: https           ← client https ব্যবহার করেছে
Connection: close                  ← proxy নিজে upstream connection manage করে
```

Backend server `X-Forwarded-For` header না পড়লে সে মনে করবে সব request proxy-র IP থেকে আসছে — তাই rate limiting ভুল হবে।

---

## ৫. একটাই জিনিস মনে রেখো

**Forward Proxy** — তুমি জানো, server জানে না। তোমার পক্ষে কাজ করে।

**Reverse Proxy** — server জানে, তুমি জানো না। Server-এর পক্ষে কাজ করে।

আর Cloudflare, AWS CloudFront, Vercel — এগুলো সব reverse proxy। তুমি যখন `vercel deploy` করো, তোমার app-এর সামনে Vercel-এর reverse proxy বসে যায় — SSL দেয়, cache করে, DDoS block করে, global CDN-এ distribute করে।

আরো কিছু জানতে চাও? যেমন — Nginx-এ microservices routing, Cloudflare Workers, বা HAProxy-র load balancing algorithm?
