# hey, I'm Jashkaran 👋

CS and Math at Penn State. I build things end to end and I can walk you through all of it. The schema, the middleware, the deployment, the part that broke at 2am.

---

## things I've actually built

**[Maya](https://github.com/jashkaransingh/maya-finance-api)** — personal finance app backed by real bank data

├─ Plaid API pulling and syncing live transactions across accounts  
├─ Flask REST API with JWT auth on every protected route  
├─ PostgreSQL schema built for fast spending queries by category and date  
├─ Gemini-powered budget assistant that reasons over your actual transaction history  
└─ deployed on AWS EC2 with gunicorn and nginx

the hard part was transaction categorization. one merchant shows up under 14 different names depending on which bank processes it. wrote a fuzzy matching layer in the categorizer to handle it. the Plaid sandbox also lies to you about how production behaves so there were edge cases I only found after pointing it at real accounts.

---

**[RAG Document Q&A](https://github.com/jashkaransingh/rag-document-qa)** — not a wrapper, an actual retrieval system

├─ documents chunk via LangChain and get embedded with sentence-transformers  
├─ stored in FAISS with max marginal relevance retrieval for diversity  
├─ multi-turn memory layer so follow-up questions have context  
└─ prompt injection guardrails built after testing showed you could manipulate answers with a crafted document

the hard part was naive top-k retrieval kept surfacing the same chunk repeatedly. switched to MMR and answer quality improved significantly. the guardrails came after I spent an afternoon trying to break my own system and succeeded faster than I expected.

---

**[HomeHarmony](https://github.com/jashkaransingh/homeharmony)** — subleasing platform with real payment flows

├─ real-time buyer-seller chat via Supabase postgres_changes subscriptions  
├─ Stripe Connect so landlords get paid directly to their bank accounts  
├─ Google Cloud Vision OCR verifying lease documents before listings go live  
└─ React TypeScript frontend, filter by city, price, bedrooms, availability

the hard part was Stripe Connect onboarding. partial failures, webhook retries, idempotency across edge cases nobody warns you about. spent two full days just on the payment flow before it felt solid.

---

**[system-monitor](https://github.com/jashkaransingh/system-monitor)** — C++ daemon running on real hardware

├─ polls CPU, memory, and disk every 10 seconds  
├─ inotify filesystem watcher catching file creates, deletes, modifications  
├─ running on 5 embedded devices via systemd since deployment  
└─ caught 3 SD card failures before data loss happened

the hard part was CPU overhead. first version ate 3 percent just from monitoring. rewrote the polling logic and got it under 0.1. the whole point of a monitor is that it doesn't slow down what it's watching.

---

## before all this

**iOS Intern at Rootchat, NYC** — rebuilt their entire onboarding from scratch. 14 screens in Swift UIKit, haptic feedback on every interaction, APNs notification system with retry logic on the backend. shipped a marketing site in 72 hours that pulled 100 unique visitors in the first 4 hours. real production work from day one.

**Math Tutor at Penn State** — 500 students over 18 months. Calculus I and II. one on ones, group sessions of 100 people before exams. turns out being able to explain why something works matters more than being able to do it yourself.

---

## tech

**daily**  
![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Swift](https://img.shields.io/badge/Swift-FA7343?style=for-the-badge&logo=swift&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-007ACC?style=for-the-badge&logo=typescript&logoColor=white)
![C++](https://img.shields.io/badge/C++-00599C?style=for-the-badge&logo=cplusplus&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-336791?style=for-the-badge&logo=postgresql&logoColor=white)

**backend and infra**  
![Flask](https://img.shields.io/badge/Flask-000000?style=for-the-badge&logo=flask&logoColor=white)
![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)
![AWS](https://img.shields.io/badge/AWS-232F3E?style=for-the-badge&logo=amazonaws&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Supabase](https://img.shields.io/badge/Supabase-3ECF8E?style=for-the-badge&logo=supabase&logoColor=white)

**ML and AI**  
![PyTorch](https://img.shields.io/badge/PyTorch-EE4C2C?style=for-the-badge&logo=pytorch&logoColor=white)
![LangChain](https://img.shields.io/badge/LangChain-121212?style=for-the-badge&logo=chainlink&logoColor=white)
![OpenCV](https://img.shields.io/badge/OpenCV-5C3EE8?style=for-the-badge&logo=opencv&logoColor=white)

---

## a few things

- tutored 500 students through calculus and never once said it was straightforward
- I write all my own SQL and I genuinely enjoy it
- grinding hard leetcode problems because I want to pass any interview I walk into

---

<div align="center">

![GitHub Stats](https://github-readme-stats.vercel.app/api?username=jashkaransingh&show_icons=true&theme=dark&hide_border=true&count_private=true)
![Top Languages](https://github-readme-stats.vercel.app/api/top-langs/?username=jashkaransingh&layout=compact&theme=dark&hide_border=true)

</div>

---

<div align="center">
<sub>State College PA · open to SWE internships · May 2027</sub>
</div>
