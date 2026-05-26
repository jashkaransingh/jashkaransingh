# hey, I'm Jashkaran 👋

CS + Math @ Penn State. I build full stack apps from the iOS frontend all the way down to the AWS config. Real production stuff, not localhost demos.

Currently shipping a finance app that pulls real bank data through Plaid and runs an AI budget assistant on top of your actual spending history. Also got into retrieval systems and now I can't stop tuning vector stores.

---

## what I actually build

### 🏦 [Maya](https://github.com/jashkaransingh/maya-finance-api)
personal finance iOS app pulling real bank data, deployed end to end

```
├─ Swift UIKit frontend with Lock Screen and Home Screen widgets for instant spending checks
├─ Flask backend on AWS EC2 behind gunicorn and nginx
├─ PostgreSQL schema for transactions, budgets, and user profiles
├─ Plaid API for live bank connections, real transactions from real accounts
├─ Firebase identity with JWT on every backend route
├─ Gemini API running an AI budget assistant that reads your actual spending history
└─ the hard part wasn't auth or the API design. it was that the same Starbucks transaction comes back as Starbucks, SBUX, Starbucks #4421, STARBUCKS COFFEE, and a dozen other variants depending on which bank you connect. wrote a fuzzy matching layer that collapses them all into one merchant. also Plaid's sandbox lies about transaction shapes so production broke things sandbox never did
```

### 🔍 [RAG Document Q&A](https://github.com/jashkaransingh/rag-document-qa)
multi-turn retrieval system you can break and watch heal

```
├─ LangChain for chunking and pipeline glue
├─ sentence-transformers for embeddings
├─ FAISS vector store using maximum marginal relevance, not naive top-k
├─ multi-turn memory so follow-up questions actually know what you asked before
├─ prompt injection guardrails after I broke my own system in an afternoon
└─ the hard part was retrieval quality. top-k kept returning the same chunk three times because everything in a doc clusters semantically close. switching to MMR forced diversity and answer quality jumped immediately. lesson that stuck with me, the model is rarely the bottleneck, the retrieval pipeline is
```

### 🏠 [HomeHarmony](https://github.com/jashkaransingh/homeharmony)
full stack subleasing platform with real money flowing through it

```
├─ React + TypeScript frontend
├─ Supabase Postgres with postgres_changes subscriptions for real-time chat between buyers and sellers
├─ Stripe Connect so landlords get payouts straight to actual bank accounts
├─ Google Cloud Vision OCR verifying lease documents before listings go live
└─ the hard part was Stripe Connect onboarding. partial failures, webhook retries, idempotency keys, what happens when a landlord starts the flow and bails halfway through. every edge case felt like discovering something Stripe forgot to document. I read payment integrations differently now
```

### ✍️ [Handwriting Font Generator](https://github.com/jashkaransingh/handwriting-font-gen)
CNN that renders any text in my own handwriting

```
├─ Python + PyTorch + OpenCV
├─ custom data pipeline with rotations, morphological ops, and synthetic noise augmentation
├─ Matplotlib labeling GUI because relabeling 50k images by hand was destroying me
└─ training prep used to take 3 hours per run because OpenCV preprocessing was running serially. parallelized the pipeline and added smarter augmentation. dropped prep time to 15 minutes
```

---

## what I've actually done with this

**iOS Developer Intern @ Rootchat** (NYC, summer 2024)
rebuilt the entire user onboarding flow, 14 screens in Swift UIKit. wrote the APNs push notification system with retry logic on the Node.js backend. shipped a marketing site in 72 hours that pulled 100 unique visitors in the first 4 hours. sat in on investor meetings the founder pulled me into without warning.

**Math Tutor @ Penn State** (Aug 2024 → now)
500+ students through Calc I and II. group exam reviews for 50+ at a time. teaching forces you to know what you actually understand vs what you think you do.

---

## tech I actually use

**daily**

![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Swift](https://img.shields.io/badge/Swift-FA7343?style=for-the-badge&logo=swift&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-007ACC?style=for-the-badge&logo=typescript&logoColor=white)
![React](https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB)
![Flask](https://img.shields.io/badge/Flask-000000?style=for-the-badge&logo=flask&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-336791?style=for-the-badge&logo=postgresql&logoColor=white)
![AWS](https://img.shields.io/badge/AWS-232F3E?style=for-the-badge&logo=amazonaws&logoColor=white)

**when I need to**

![C++](https://img.shields.io/badge/C++-00599C?style=for-the-badge&logo=cplusplus&logoColor=white)
![Java](https://img.shields.io/badge/Java-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white)
![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Firebase](https://img.shields.io/badge/Firebase-FFCA28?style=for-the-badge&logo=firebase&logoColor=black)
![Supabase](https://img.shields.io/badge/Supabase-3FCF8E?style=for-the-badge&logo=supabase&logoColor=white)
![NGINX](https://img.shields.io/badge/nginx-009639?style=for-the-badge&logo=nginx&logoColor=white)

**experimenting with**

![PyTorch](https://img.shields.io/badge/PyTorch-EE4C2C?style=for-the-badge&logo=pytorch&logoColor=white)
![LangChain](https://img.shields.io/badge/LangChain-1C3C3C?style=for-the-badge&logo=langchain&logoColor=white)
![FAISS](https://img.shields.io/badge/FAISS-0467DF?style=for-the-badge&logo=meta&logoColor=white)
![Gemini](https://img.shields.io/badge/Gemini-8E75B2?style=for-the-badge&logo=googlegemini&logoColor=white)
![OpenCV](https://img.shields.io/badge/OpenCV-27338e?style=for-the-badge&logo=OpenCV&logoColor=white)

---

## things you should probably know

- think the hardest part of RAG is chunking, not the model
- ship code that talks to real bank accounts through Plaid
- once spent more time debugging Stripe Connect webhooks than writing the app they live in
- read API docs for fun and file issues against them for sport
- 500+ Penn State students taught calc, somehow it made me better at code reviews
- prefer 4 deep projects over 20 shallow ones

---

## stats nobody asked for

![Jashkaran's GitHub stats](https://github-readme-stats.vercel.app/api?username=jashkaransingh&show_icons=true&theme=tokyonight&hide_border=true)

![Top Languages](https://github-readme-stats.vercel.app/api/top-langs/?username=jashkaransingh&layout=compact&theme=tokyonight&hide_border=true)

---

## let's build something

if you're working on iOS, backend, payment integrations, retrieval pipelines, or anything full stack that ships to actual users, reach out. always down to talk infra, Stripe edge cases, or why your Plaid sandbox is lying to you.

📫 [linkedin](https://linkedin.com/in/YOUR-HANDLE-HERE)
