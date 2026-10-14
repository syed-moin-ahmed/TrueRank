<div align="center">

# TrueRank

### Trust-Aware NLP Framework for Matching Informal Workers to Industry Demand

**Ranks electricians, plumbers, carpenters and other informal workers for a plain-English service request by combining *what they can do* with *how much they can be trusted*.**

[**🔗 Live demo**](https://truerank-teal.vercel.app) · [📄 Research paper (Scopus · Taylor & Francis)](#research) · [How it works](#how-it-works) · [Tech stack](#tech-stack)

![Python](https://img.shields.io/badge/Python-3.10+-3776AB?logo=python&logoColor=white)
![scikit-learn](https://img.shields.io/badge/scikit--learn-TF--IDF-F7931E?logo=scikitlearn&logoColor=white)
![NLTK](https://img.shields.io/badge/NLTK-lemmatization-154F5B)
![Flask](https://img.shields.io/badge/Flask-REST%20API-000000?logo=flask&logoColor=white)
![React](https://img.shields.io/badge/React-19-61DAFB?logo=react&logoColor=black)
![SQLite](https://img.shields.io/badge/SQLite-13%20tables-003B57?logo=sqlite&logoColor=white)
![Publication](https://img.shields.io/badge/Paper-Scopus%20%C2%B7%20Taylor%20%26%20Francis-0A5EB0)
![Tests](https://img.shields.io/badge/tests-40%20passing-2EA44F)

</div>

---

## The problem

Millions of informal workers find jobs through word of mouth. Online platforms that try to help usually match on keywords alone, so a worker with the right words in their profile but a history of abandoned jobs and disputes ranks as high as a dependable one. Customers can't tell who is reliable, and reliable workers get no credit for it.

## The solution

TrueRank reads a natural-language request such as *"need someone to fix damaged wiring and a tripping circuit breaker"*, finds the workers whose skills match it, and then **re-ranks them by a composite trust score** built from their real track record. Every recommendation is **explained**: which words matched, and how each part of the trust score contributed.

## How it works

```
 Service request ──► NLP preprocessing ──► TF-IDF vectors ──► cosine similarity  S(w,q)
                     (tokenize, stop words,                          │
                      lemmatize)                                     ▼
 Worker history ───► Composite Trust Score ─────────────► R(w,q) = 0.7·S(w,q) + 0.3·TS(w)
 ratings · jobs ·    TS = 0.40·AR + 0.20·CR                          │
 endorsements ·           + 0.20·PE + 0.20·DA                        ▼
 disputes                                              Ranked, explained recommendations
```

| Trust component | What it measures |
|---|---|
| **AR** Average Rating | customer star ratings |
| **CR** Completion Rate | jobs finished vs. abandoned after accepting |
| **PE** Peer Endorsements | endorsements from other workers |
| **DA** Dispute Adjustment | inverted dispute rate: fewer disputes, higher score |

**Live feedback loop.** Every completed job, rating, endorsement or dispute recalculates the worker's trust score, so the next search ranks differently. A full trust history is kept for every worker.

## Results

From the offline evaluation in the research paper:

| Metric | TrueRank |
|---|---|
| Accuracy | **90.6 %** |
| Precision | **90.2 %** |
| Recall | **89.8 %** |
| F1-score | **90.0 %** |

## Features

- **Four portals:** public site, service requester, worker and admin
- **Explainable ranking:** the matched terms add up exactly to the similarity score, and each trust component is shown separately
- **Cold-start handling:** new workers get a documented neutral prior instead of being buried or over-promoted
- **Relevance threshold:** a highly trusted worker with no skill overlap can never outrank a relevant one
- **Admin tools:** dispute resolution, verification review, user management and a live matching console with an adjustable λ
- **Security:** JWT authentication, role-based access, hashed passwords, parameterised SQL and security headers
- **Tested:** 40 automated tests cover the ML core, the trust formula (checked by hand), the real NLTK pipeline and a complete end-to-end API flow

## Tech stack

| Layer | Technologies |
|---|---|
| ML / NLP | Python, scikit-learn (TF-IDF, cosine similarity), NLTK (stop words, WordNet lemmatizer), NumPy, SciPy |
| Backend | Flask REST API, Pydantic validation, PyJWT, SQLite (13 tables with foreign keys and constraints) |
| Frontend | React 19, esbuild |
| Deployment | Vercel (static site + Python serverless API) |

## Architecture

The research core (`ml/`) is fully separate from the web layer: it never imports Flask or touches the database, so the algorithm can be tested and explained on its own.

```
React frontend  ──REST/JSON + JWT──►  Flask backend  ──►  SQLite
                                           │
                                           └──►  ml/  preprocessing · TF-IDF · trust · ranking
```

## Research

📄 **"TrueRank: Trust-Aware NLP Framework for Matching Informal Workers to Industry Demand"**

- **First author:** Syed Moin Ahmed
- **Publication:** forthcoming in a **Scopus-indexed Taylor & Francis (CRC Press)** publication

The web application implements the paper's method exactly: the same preprocessing pipeline, the same TF-IDF + cosine similarity matching, and the paper's trust and ranking formulas, with the paper treated as the source of truth.

---

## Author

**Syed Moin Ahmed**: AI & Machine Learning · First author of the TrueRank research paper (Scopus-indexed, Taylor & Francis)
[GitHub](https://github.com/syed-moin-ahmed)

## Source code

The source code is kept private. If you are a recruiter or hiring manager and would like a code walkthrough or access to the repository, please get in touch through my GitHub profile.

---

<sub>© 2026 Syed Moin Ahmed. All rights reserved. TrueRank, its design, algorithm and implementation may not be copied, reused or redistributed without written permission.</sub>
