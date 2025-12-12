# Personalized Retrieval for YouTube Recommendations  
*A RAG-style semantic search and ranking system using SBERT, kNN, and user profile embeddings*

---

## Overview

This project implements a personalized retrieval system inspired by YouTube’s two-stage recommendation pipeline and Retrieval-Augmented Generation (RAG) retrieval architectures. The goal is to explore how semantic similarity, popularity signals, and personalization interact to rank videos—and how these signals can drift away from strict query intent.

The system retrieves candidate videos using **Sentence-BERT embeddings + kNN semantic search**, then re-ranks them using:

- **Semantic similarity**  
- **Popularity score** (log(view_count))  
- **Personalization score** (user profile embedding)  
- **Keyword boosts**

The project includes quantitative evaluation (Precision@5, Recall@5), as well as qualitative case studies showing how two different simulated users receive different ranked results for the same query.

---

## Repository Structure

<pre style="font-size: 13px;">
youtube-personalized-retrieval/
├── README.md
│
├── notebooks/
│   └── code.ipynb                  # Main implementation
│
├── report/
│   └── Final_Report.pdf            # Full project write-up
│
├── src/
│   ├── embedding.py                # SBERT embedding functions
│   ├── semantic_search.py          # kNN semantic retrieval
│   ├── ranking.py                  # Hybrid ranking functions
│   ├── user_profile.py             # User profile vector construction
│   ├── evaluation.py               # Precision@5 and Recall@5 metrics
│   └── utils.py
│
├── data/
│   └── sample_data.csv             
│
└── requirements.txt

</pre>

## Installation

### 1. Clone the repository
```bash
git clone https://github.com/your-username/youtube-personalized-retrieval.git
cd youtube-personalized-retrieval
```

### 2. Install dependencies
```bash
pip install -r requirements.txt
```

### 3. Launch the notebook
```bash
jupyter notebook
# open notebooks/code.ipynb to run the full retrieval and evaluation pipeline
```
---
## How It Works
1. Use sequence-aware user models (RNN / Transformer)
2. Dynamic personalization weights
3. Cross-encoder re-ranking for more accurate ranking
4. Integrate into a complete RAG pipeline with LLM query rewriting
5. Expand dataset and evaluation queries

---
## Features

### Semantic Search
- Uses Sentence-BERT (`all-MiniLM-L6-v2`)
- Embeds titles + descriptions → dense semantic vectors  
- kNN retrieves closest meaning-based results  

### User Profile Embeddings
- Synthetic user interaction histories for controlled evaluation  
- User profile = average embedding of watched videos  
- Enables personalized retrieval behavior  

### Re-Ranking Function
Final ranking score:
score = 
+ α * semantic_similarity
+ β * popularity_score
+ γ * personalization_score
+ keyword_boost

### Evaluation
- **Precision@5**  
- **Recall@5**  
- Case studies showing personalization vs semantic-only behavior  

---

## Results Summary

### **Quantitative Findings**
- Semantic search baseline achieved the highest Precision@5 and Recall@5.  
- Adding popularity sometimes surfaced trending but irrelevant content.  
- Personalization weakened accuracy in factual queries due to **query dilution** (history overpowering the query).

### **Qualitative Findings**
- For the same query (“space news”),  
  - *User A* (black-hole preference) received black-hole-leaning results.  
  - *User B* (exoplanet preference) received planet-related results.  
- Confirms personalization logic is working even when accuracy drops.

---

## Analysis Summary (From Final Report)

This project demonstrated several important insights:

### **1. Understanding Retrieval Behavior**
- Semantic search returned the most accurate results.  
- Personalization can shift rankings away from the query when user history dominates.  
- Popularity introduces bias toward high-engagement videos, not necessarily relevant ones.

### **2. Limitations Discovered**
- User profiles are manually constructed from a small set of chosen videos, so they reflect simulated behavior rather than real watch histories.
- Profiles are built by simple averaging of past item embeddings, ignoring sequence effects, recency, and session structure.
- Personalization strength is controlled by a single global parameter(alpha) rather than a learned, context-aware mechanism.
- The current system focuses only on retrieval and re-ranking; it does not include any LLM components such as query rewriting or LLM-as-judge evaluation.

### **3. Future Work**
- Use sequence-aware user models (RNN / Transformer)
- Dynamic personalization weights
- Cross-encoder re-ranking for more accurate ranking
- Integrate into a complete RAG pipeline with LLM query rewriting
- Expand dataset and evaluation queries

A detailed explanation is provided in [Final Report](report/Final_Report.pdf`)

--- 
## Data Source
This project uses a custom-scraped dataset prepared by **[Lisha]**, collected via the YouTube Trending Videos API (Public Data).  
- Region: United States  
- Sample Size: 13,017 trending videos  
--- 

## License (MIT)
This project is licensed under the MIT License.


## Contact
If you have question or world like to build on this project, feel free to reach out.


