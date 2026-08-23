<h1 align="center">Jashkaran Singh</h1>

<p align="center">
  <strong>Retrieval systems &nbsp;·&nbsp; LLM serving infrastructure &nbsp;·&nbsp; Storage engines</strong>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/CS%20%2B%20Math-Penn%20State-1E407C?style=flat-square" alt="CS + Math @ Penn State">
  <a href="https://www.linkedin.com/in/jashkaran-singh/"><img src="https://img.shields.io/badge/LinkedIn-0A66C2?style=flat-square&logo=linkedin&logoColor=white" alt="LinkedIn"></a>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white" alt="Python">
  <img src="https://img.shields.io/badge/C%2B%2B-00599C?style=flat-square&logo=cplusplus&logoColor=white" alt="C++">
  <img src="https://img.shields.io/badge/Rust-000000?style=flat-square&logo=rust&logoColor=white" alt="Rust">
  <img src="https://img.shields.io/badge/Swift-FA7343?style=flat-square&logo=swift&logoColor=white" alt="Swift">
  <img src="https://img.shields.io/badge/TypeScript-3178C6?style=flat-square&logo=typescript&logoColor=white" alt="TypeScript">
  <img src="https://img.shields.io/badge/PyTorch-EE4C2C?style=flat-square&logo=pytorch&logoColor=white" alt="PyTorch">
  <img src="https://img.shields.io/badge/FAISS-0467DF?style=flat-square&logo=meta&logoColor=white" alt="FAISS">
  <img src="https://img.shields.io/badge/PostgreSQL-4169E1?style=flat-square&logo=postgresql&logoColor=white" alt="PostgreSQL">
  <img src="https://img.shields.io/badge/Redis-DC382D?style=flat-square&logo=redis&logoColor=white" alt="Redis">
  <img src="https://img.shields.io/badge/AWS-232F3E?style=flat-square&logo=amazonwebservices&logoColor=white" alt="AWS">
</p>

---

I build the layers most applications import rather than write: the scheduler that sits between an HTTP handler and a forward pass, the fusion and reranking stages under a retrieval pipeline, the log-structured merge tree under a key/value store.

The through line across these projects is measurement. Every number below was produced by a script committed to its repository, with raw output alongside it, including the results that argue against the design. A retrieval loop that improves recall but not answers, lock striping that buys nothing until the workload is dense enough to contend, and compaction settings where the lowest write amplification comes from turning compaction off are all reported as measured.

---

## Proof

| Project | What's real | CI |
|---|---|---|
| [llama-serve](https://github.com/jashkaransingh/llama-serve) | 27.95 QPS sustained, 1.78× the blocking baseline at 85% lower p50 TTFT. | ![CI](https://github.com/jashkaransingh/llama-serve/actions/workflows/ci.yml/badge.svg) |
| [multihop-qa](https://github.com/jashkaransingh/multihop-qa) | nDCG@10 0.7223 → 0.8529 on HotpotQA, significant at p = 0.0005. | ![CI](https://github.com/jashkaransingh/multihop-qa/actions/workflows/ci.yml/badge.svg) |
| [memcask](https://github.com/jashkaransingh/memcask) | 572,096 ops/sec at 256 concurrent connections, zero errors. | ![CI](https://github.com/jashkaransingh/memcask/actions/workflows/ci.yml/badge.svg) |
| [minidb](https://github.com/jashkaransingh/minidb) | ~180K durable writes/sec at 16 threads; 1,420 crash scenarios, zero losses. | ![CI](https://github.com/jashkaransingh/minidb/actions/workflows/ci.yml/badge.svg) |

---

## Selected work

### [llama-serve](https://github.com/jashkaransingh/llama-serve) <sub>Python · llama.cpp · Metal · Prometheus</sub>

A local LLM inference server built around the part of model serving that sits between the HTTP handler and the forward pass. Serving one request is a loop; serving a hundred concurrent ones well is a scheduling problem.

- Continuous, iteration-level batching, so the batch is rebuilt every step instead of waiting for the slowest sequence in a static one.
- Paged KV-cache allocator with prefix sharing across requests over a common preamble.
- Priority scheduling with pause-and-resume preemption and starvation protection for low-priority work.
- Prometheus `/metrics` with exact quantiles, and a quantile is omitted rather than invented below its sample floor.

**Measured** on an M1 Pro against a real model. Peak sustained **27.95 QPS** (Qwen2.5-0.5B Q4_K_M, 16 output tokens, 64 slots). Against the blocking baseline on an identical workload: **1.78× sustained throughput at 85% lower p50 TTFT**. Preemption cuts urgent-request TTFT from 15.81 s to **0.112 s** with no measurable slowdown for background generations. Prefix sharing skips **77% of prefill work** for an 85% drop in warm-wave TTFT, and a `temperature == 0` sampler fast path is **5.16× faster**, verified token-for-token against llama.cpp's own sampler. Step profiling locates the ceiling: decode is memory-bandwidth-bound below batch width 32 and compute-bound above it, so more scheduler work is not the remaining lever. 93 tests run against a deterministic mock backend with no model loaded.

### [multihop-qa](https://github.com/jashkaransingh/multihop-qa) <sub>Python · FAISS · BM25 · cross-encoders · HotpotQA / MuSiQue</sub>

Hybrid retrieval with reciprocal rank fusion, cross-encoder reranking, a multi-hop retrieval loop, and a learned query-complexity router, evaluated on pooled-distractor subsets of HotpotQA and MuSiQue.

- Dense FAISS and BM25 fused by RRF, with fusion weights fitted on a dev split disjoint from eval.
- Cross-encoder reranking over the top 50 fused candidates.
- Multi-hop loop with entity-expansion query reformulation, capped at 3 hops because ablation showed full-recall@10 saturating there on both datasets.
- Logistic-regression router over question-text features only, so a "simple" verdict skips the expensive path entirely rather than just its tail.

**Measured** over 500 eval questions per dataset with bootstrap confidence intervals. nDCG@10 rises **0.7223 → 0.8529** on HotpotQA and **0.5290 → 0.6100** on MuSiQue over a dense-only baseline, both significant at p = 0.0005. The router removes **48.3% of retrieval calls and 42.9% of latency** with no measurable answer-F1 change. The repository also reports what the measurements do not support: with a 250M-parameter reader, the multi-hop loop produces no end-to-end answer gain, its benefit is visible in retrieval metrics only, and textbook equal-weight RRF is worth nothing on MuSiQue because equal weighting lets the weaker retriever drag the stronger one down.

### [memcask](https://github.com/jashkaransingh/memcask) <sub>C++17 · epoll / kqueue · RESP2</sub>

A multithreaded in-memory key-value store: a non-blocking, edge-triggered event loop over a fixed thread pool, speaking a RESP2 subset, on top of a lock-striped sharded map with per-shard LRU eviction and asynchronous op-log replication.

- One event loop per thread, connections pinned after handoff so all per-connection state stays single-threaded and needs no locking.
- Lock-striped shards, each `alignas(64)`, with the shard index taken from the high bits of the hash and the inner table indexing on the low bits.
- Per-shard LRU with a byte budget, riding on a lock already held, because a single global LRU list would turn every read into a global write.
- Async per-shard op-log replication with full resync and live tail, run off a dedicated session thread so a slow replica cannot stall the event loop.

**Measured** on a 6-vCPU Linux VM that is also running the load generator. **572,096 ops/sec** at 256 concurrent connections and **464,272 ops/sec** at 10,000, with zero errors and zero dropped connections. Lock striping is worth **3.97×** once the workload is dense enough to contend for the lock, and roughly nothing at one request per connection, which the README explains rather than hides. Hash quality is measured rather than assumed (chi-square 65.9 against 63 degrees of freedom over 200,000 keys), and the suite runs clean under ThreadSanitizer and Address/UndefinedBehavior sanitizers.

### [minidb](https://github.com/jashkaransingh/minidb) <sub>Rust · LSM tree · MVCC</sub>

An embedded log-structured merge-tree key/value store with one dependency (`crc32fast`). The log format, the on-disk tables, and the recovery logic are written from scratch. Same family of design as RocksDB and LevelDB, small enough to read in one sitting.

- Write-ahead log with group commit: one leader writes every waiting writer's mutations as a single checksummed frame and pays a single fsync for all of them.
- Lock-free skiplist memtable, insert-only because MVCC turns a delete into a tombstone insert and an overwrite into a new version insert.
- SSTables with bloom filters and sparse indexes, so a read filters through four stages cheapest first: key range, bloom, binary search, then one 4 KiB block.
- Size-tiered compaction with correct tombstone lifetime and a journalled crash-safe table swap.

**Measured** on Apple Silicon. **~180K durable writes/sec** at 16 threads by 56-mutation batches, median of 24 runs, against a device that can only fsync ~235 times per second, which is why batch size is the only real lever. Point reads: **1,833 ns p50 on a hit, 125 ns p50 on a miss**, the second being the bloom filter answering without touching the data section. 285 tests including **1,420 randomized crash scenarios** across four failure families, verifying **78,728 acknowledged mutations** present after recovery with zero losses. Write amplification is measured across compaction strategies, and the honest finding is reported: disabling compaction gives the lowest write amplification of the four, at 23.43× space amplification, which is what compaction actually buys.

### [maya-finance-api](https://github.com/jashkaransingh/maya-finance-api) <sub>Flask · Plaid · PostgreSQL · scikit-learn · AWS</sub>

Backend for Maya, a personal finance iOS app. Pulls live bank transactions through Plaid, categorizes them, and serves a JWT-protected REST API to a Swift UIKit client, deployed on EC2 behind gunicorn and nginx with Firebase identity and a Gemini-backed budget assistant.

- Merchant normalization strips store numbers, processor prefixes, and bank reference ids, then a fuzzy pass over Levenshtein distance collapses `STARBUCKS #4421 SEATTLE WA`, `SBUX 00291`, and `Starbucks Coffee` into one canonical merchant.
- A rules-based card reward optimizer that prices realized dollars against live cap state, so a card that has run out of headroom degrades to its base rate instead of winning on its headline number.
- Gradient-boosted quantile regression for 7-day spend forecasting, with a feature-leakage test that truncates history at `as_of` and asserts no feature value moves.
- A safe-to-spend engine that reserves against committed outflow at a conformally corrected upper bound, because under-reserving tells someone they can spend money a bill is about to claim.

**Measured** on seeded synthetic data, labeled as synthetic in every results file, since the production database holds live Plaid data for real people. The reward optimizer returns **67.99% more gross yield** than the best single card chosen with hindsight over 25,534 purchases. The forecaster cuts MAE **65.81%** against a rolling average, and **9.17%** against a day-of-month profile baseline, which is the fairer number and the one the README leads with when explaining what gradient boosting actually contributes. Conformal calibration moves q10/q90 coverage from **60.71% to 81.78%** against a nominal 80%, at the honest cost of a wider band. 111 tests, and the committed experiments rerun end to end in CI.

### [rag-eval](https://github.com/jashkaransingh/rag-eval) <sub>Python · LLM-as-judge · NumPy</sub>

A framework for measuring whether a RAG system actually works, built because shipping a retriever and eyeballing a few answers is not an evaluation.

- Retrieval metrics (hit@k, recall@k, precision@k, MRR, nDCG@k) alongside LLM-as-judge metrics for faithfulness, answer relevance, context precision, and correctness.
- Any system plugs in through one small adapter that returns a `RAGOutput`; everything downstream is system-agnostic.
- A defensive verdict parser, because judge models fence their JSON in markdown and add preambles, and without it the judge metrics silently scored zero whenever the model got chatty.
- A deterministic stub judge that exercises every code path, so the full suite runs in CI and offline with no API key.

Retrieval metrics return NaN for unlabeled cases and the aggregator drops NaNs per metric rather than per case, so a partially labeled test set does not poison the averages. Reports render as self-contained HTML. 29 tests.

---

### Also in the repositories

| Project | What it is |
|---|---|
| [hnsw-vector-search](https://github.com/jashkaransingh/hnsw-vector-search) | Header-only C++20 HNSW index with AVX2 and FMA distance kernels. 0.95 recall@10 at 17× brute-force throughput on 100k vectors of dimension 128. Built standalone to understand what sits underneath FAISS and Qdrant. |
| [rag-document-qa](https://github.com/jashkaransingh/rag-document-qa) | Multi-turn RAG over documents. LangChain chunking, sentence-transformers embeddings, FAISS with MMR retrieval, and prompt-injection guardrails. The system `rag-eval` was written to measure. |
| [homeharmony](https://github.com/jashkaransingh/homeharmony) | Full-stack subleasing platform. React and TypeScript on Supabase, real-time chat over `postgres_changes`, Stripe Connect payouts, and Google Cloud Vision OCR for lease verification. |
| [system-monitor](https://github.com/jashkaransingh/system-monitor) | C++ background daemon polling CPU, memory, and disk on Linux with inotify file watching and systemd integration, under 0.1% CPU overhead. |
| [handwriting-gen](https://github.com/jashkaransingh/handwriting-gen) | A 62-class character CNN plus a baseline-aware renderer that writes any string in the learned style. PyTorch and OpenCV, 92% validation accuracy in 4 epochs on CPU, and a synthetic data path that takes training prep from 3 hours to roughly 15 seconds. |

---

## Experience

**Software Engineering Intern, Rootchat** · New York, NY · Summer 2024

Worked across both halves of the product. On iOS, rebuilt user onboarding from scratch in Swift and UIKit, 14 screens with haptic feedback throughout, and wired push notifications end to end through APNs.

On the backend, built the real-time messaging service in FastAPI, with WebSockets holding the live connections and Redis Pub/Sub underneath so a message fans out to every subscribed connection instead of staying trapped in the process that received it. Made the PostgreSQL-backed endpoints idempotent using client-generated UUIDs, because a phone on a bad connection retries, and a retry that writes twice leaves a duplicate message in someone's chat permanently. Replaced the SQL polling that tracked online status with Redis TTL heartbeats, so presence expires on its own when a client goes quiet and the database stops answering the same question every few seconds.

The other half of the experience was the room. Small team, direct exposure to investor meetings and technical calls with little warning, and enough of it that being handed something past my depth stopped feeling like a stretch.

**Math Tutor, Penn State** · Aug 2024 to present

500+ students over 18 months through Calculus I and II, plus group exam reviews for 50+ at a time. Explaining limits and integrals to someone genuinely lost forces a kind of precision that is hard to fake, and it turns out to be the same skill as a good code review: finding where the understanding actually breaks, then meeting them there.

---

## Stack

**Languages**

![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![C++](https://img.shields.io/badge/C%2B%2B-00599C?style=flat-square&logo=cplusplus&logoColor=white)
![Rust](https://img.shields.io/badge/Rust-000000?style=flat-square&logo=rust&logoColor=white)
![Swift](https://img.shields.io/badge/Swift-FA7343?style=flat-square&logo=swift&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=flat-square&logo=typescript&logoColor=white)
![Java](https://img.shields.io/badge/Java-ED8B00?style=flat-square&logo=openjdk&logoColor=white)
![SQL](https://img.shields.io/badge/SQL-4479A1?style=flat-square&logo=postgresql&logoColor=white)

**ML and retrieval**

![PyTorch](https://img.shields.io/badge/PyTorch-EE4C2C?style=flat-square&logo=pytorch&logoColor=white)
![FAISS](https://img.shields.io/badge/FAISS-0467DF?style=flat-square&logo=meta&logoColor=white)
![Hugging Face](https://img.shields.io/badge/Hugging%20Face-FFD21E?style=flat-square&logo=huggingface&logoColor=black)
![LangChain](https://img.shields.io/badge/LangChain-1C3C3C?style=flat-square&logo=langchain&logoColor=white)
![scikit-learn](https://img.shields.io/badge/scikit--learn-F7931E?style=flat-square&logo=scikitlearn&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-013243?style=flat-square&logo=numpy&logoColor=white)
![pandas](https://img.shields.io/badge/pandas-150458?style=flat-square&logo=pandas&logoColor=white)
![OpenCV](https://img.shields.io/badge/OpenCV-5C3EE8?style=flat-square&logo=opencv&logoColor=white)

**Systems and infrastructure**

![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=flat-square&logo=postgresql&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-DC382D?style=flat-square&logo=redis&logoColor=white)
![Linux](https://img.shields.io/badge/Linux-FCC624?style=flat-square&logo=linux&logoColor=black)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white)
![AWS](https://img.shields.io/badge/AWS-232F3E?style=flat-square&logo=amazonwebservices&logoColor=white)
![NGINX](https://img.shields.io/badge/NGINX-009639?style=flat-square&logo=nginx&logoColor=white)
![CMake](https://img.shields.io/badge/CMake-064F8C?style=flat-square&logo=cmake&logoColor=white)
![Prometheus](https://img.shields.io/badge/Prometheus-E6522C?style=flat-square&logo=prometheus&logoColor=white)

**Application layer**

![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=flat-square&logo=fastapi&logoColor=white)
![Flask](https://img.shields.io/badge/Flask-000000?style=flat-square&logo=flask&logoColor=white)
![React](https://img.shields.io/badge/React-20232A?style=flat-square&logo=react&logoColor=61DAFB)
![UIKit](https://img.shields.io/badge/UIKit-2396F3?style=flat-square&logo=apple&logoColor=white)
![Supabase](https://img.shields.io/badge/Supabase-3FCF8E?style=flat-square&logo=supabase&logoColor=white)
![Firebase](https://img.shields.io/badge/Firebase-FFCA28?style=flat-square&logo=firebase&logoColor=black)
![Stripe](https://img.shields.io/badge/Stripe-635BFF?style=flat-square&logo=stripe&logoColor=white)

---

## Contact

Interested in inference infrastructure, retrieval and ranking, and storage systems, and always happy to talk about any of them.

[LinkedIn](https://www.linkedin.com/in/jashkaran-singh/)
