# hey, I'm Jashkaran 👋

CS + Math @ Penn State. I build full stack apps from the iOS frontend all the way down to the AWS config. Real production stuff, not localhost demos.

most of what I build starts because something annoyed me. a banking app with a cluttered UI, a RAG system I couldn't trust, a subleasing process that was a mess to deal with. the problems I like are the ones where the hard part is hidden a layer below where everyone stops looking. the merchant string that comes back fourteen different ways. the retrieval step that quietly returns the same chunk three times. the Stripe webhook that fires twice and corrupts your state if you let it.

Currently shipping a finance app that pulls real bank data through Plaid and runs an AI budget assistant on your actual spending. Also went deep on retrieval systems, built one, then built the framework to measure how good it actually is.

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

### 🔍 [RAG Document Q&A](https://github.com/jashkaransingh/rag-document-qa)  +  📊 [rag-eval](https://github.com/jashkaransingh/rag-eval)
a multi-turn retrieval system, and the framework I built to prove it works

```
├─ RAG side, LangChain chunking, sentence-transformers embeddings, FAISS with MMR instead of naive top-k, multi-turn memory, prompt injection guardrails
├─ eval side, retrieval metrics like recall@k, mrr, and ndcg, plus LLM-as-judge scoring for faithfulness, answer relevance, and context precision
├─ the eval framework plugs into any RAG system through one adapter interface, not just mine, and writes self-contained HTML reports
└─ the through line, I built the RAG system, did not trust my own eyeballing of the answers, so I built the tool that puts numbers on it. top-k kept returning the same chunk three times until MMR fixed it, and I only knew the fix actually worked because the eval numbers moved
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
CNN that renders any text in a learned handwriting style

```
├─ Python + PyTorch + OpenCV, 92% validation accuracy across 62 character classes
├─ custom data pipeline with rotations, morphological ops, elastic distortion, and synthetic noise
├─ Matplotlib labeling GUI because relabeling thousands of glyphs by hand was destroying me
└─ training prep used to take 3 hours per run because OpenCV preprocessing was running serially. parallelized the pipeline and added smarter augmentation. dropped prep time to 15 minutes
```

---

## what I've actually done with this

**iOS Developer Intern @ Rootchat** (NYC, summer 2024)
rebuilt the entire user onboarding from scratch, 14 screens in Swift UIKit, and added haptics across the whole app so every interaction had feedback. wrote the push notification system end to end, Swift on the front and Node.js on the back, with APNs retry logic for failed deliveries and payloads tuned for instant lock screen delivery. shipped a marketing site in 72 hours that pulled 100 unique visitors in the first 4 hours. the part that taught me the most was the room. the founder pulled me into investor meetings without warning and once handed me a live call with the tech team to take over and brief everyone the next day. small team, every decision mattered, and I got used to being thrown past my comfort zone until it stopped feeling like a stretch.

**Math Tutor @ Penn State** (Aug 2024 → now)
500+ students over 18 months through Calc I and II, plus group exam reviews for 50+ at a time. breaking down limits and integrals for someone who is genuinely lost forces a kind of clarity you cannot fake, and it turned out to be the same muscle as a good code review, finding where someone's understanding actually breaks and meeting them there.

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
- build eval tools for my own systems because I do not trust what I have not measured
- read API docs for fun and file issues against them for sport
- 500+ Penn State students taught calc, somehow it made me better at code reviews
- prefer 4 deep projects over 20 shallow ones

---

## stats nobody asked for

![Jashkaran's GitHub stats](https://github-readme-stats.vercel.app/api?username=jashkaransingh&show_icons=true&theme=tokyonight&hide_border=true)

![Top Languages](https://github-readme-stats.vercel.app/api/top-langs/?username=jashkaransingh&layout=compact&theme=tokyonight&hide_border=true)

---

## let's build something

if you're working on iOS, backend, payment integrations, retrieval pipelines, LLM evals, or anything full stack that ships to actual users, reach out. always down to talk infra, Stripe edge cases, or why your Plaid sandbox is lying to you.

📫 [linkedin](https://www.linkedin.com/in/jashkaran-singh/)
