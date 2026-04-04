
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

