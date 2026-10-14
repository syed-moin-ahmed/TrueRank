<div align="center">

# TrueRank

### Trust-Aware NLP Framework for Matching Informal Workers to Industry Demand

**Ranks electricians, plumbers, carpenters and other informal workers for a plain-English service request by combining *what they can do* with *how much they can be trusted*.**

[**🔗 Live demo**](https://truerank-teal.vercel.app) · [📄 Research paper (Scopus · Taylor & Francis)](#research-publication) · [How it works](#how-it-works) · [💻 Source code](https://github.com/syed-moin-ahmed/TrueRank-source) · [Tech stack](#tech-stack)

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

Evaluated on a dataset of **1,500 informal-worker profiles** from Bengaluru, Mumbai, Delhi, Hyderabad and Chennai (from the research paper):

| Model | Accuracy | Precision | Recall | F1 |
|---|---|---|---|---|
| Naïve Bayes | 78.4 % | 77.9 % | 76.8 % | 77.3 % |
| Logistic Regression | 82.1 % | 81.6 % | 80.9 % | 81.2 % |
| SVM | 85.7 % | 85.2 % | 84.6 % | 84.8 % |
| Random Forest | 87.9 % | 87.4 % | 87.1 % | 87.2 % |
| **TrueRank** | **90.6 %** | **90.2 %** | **89.8 %** | **90.0 %** |

TrueRank beats the strongest baseline (Random Forest) by **2.7 points** of accuracy.

**Ablation study: every component earns its place**

| Variant | Accuracy |
|---|---|
| Full TrueRank | **90.6 %** |
| Without Sentiment Feature | 86.0 % |
| Without Trust Score | 87.3 % |
| Without TF-IDF Layer | 84.1 % |
| Baseline (TF-IDF only) | 78.4 % |

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

## Research publication

📄 **True Rank: Trust-Aware NLP Framework for Matching Informal Workers to Industry Demand**

**S. M. Ahmed** *(first & corresponding author)*, N. Ghorpade, S. Farookh, N. F. Baig, S. U. Kulsum

- Presented at **ICSIIT**; forthcoming in **Scopus-indexed Taylor & Francis (CRC Press)** conference proceedings

> **Abstract.** India has over 2 crore informal blue-collar workers who have largely gone unnoticed by today's digital hiring platforms as they lack formal resumes and provable work histories. True Rank is a machine learning framework based on natural language processing (NLP) which ranks informal workers and matches them to service requests using unstructured profile text only. The system uses TF-IDF vectorization to convert worker profiles and user queries into a common feature space. The semantic similarity score is then merged with a Composite Trust Score based on historical ratings, job-completion rate, peer endorsements and dispute history. On a proprietary dataset of 1,500 informal worker profiles, True Rank achieves 90.6% classification accuracy, outperforming Naïve Bayes, Logistic Regression, SVM and Random Forest baselines, confirming that jointly modelling skill relevance and behavioural trust outperforms either signal alone.

**Keywords:** Skill Matching · Gig Economy · Digital Employment Ecosystem · Trust Score Analytics · Informal Labor Sector · Workforce Intelligence

<details>
<summary><b>Cite this work</b></summary>

```text
S. M. Ahmed, N. Ghorpade, S. Farookh, N. F. Baig, and S. U. Kulsum,
"True Rank: Trust-Aware NLP Framework for Matching Informal Workers to Industry Demand,"
in Proc. ICSIIT, Taylor & Francis (CRC Press), forthcoming.
```
</details>

The web application implements the paper's method directly: the same preprocessing pipeline, TF-IDF + cosine similarity matching, and the paper's trust and ranking formulas. The full paper is available on request.

---

## Author

**Syed Moin Ahmed**: AI & Machine Learning · First author of the TrueRank research paper (Scopus-indexed, Taylor & Francis)  
📧 syedmoin1907@gmail.com
[GitHub](https://github.com/syed-moin-ahmed)

## Source code

💻 **Full source code:** [github.com/syed-moin-ahmed/TrueRank-source](https://github.com/syed-moin-ahmed/TrueRank-source): Python ML core, Flask API, React frontend and 40 automated tests. Published for viewing and evaluation; all rights reserved.

---

<sub>© 2026 Syed Moin Ahmed. All rights reserved. TrueRank, its design, algorithm and implementation may not be copied, reused or redistributed without written permission.</sub>
