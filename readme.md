# OSBERT — Relation-Aware BERT

OSBERT is a fine-tuned BERT model designed to predict the **semantic relationship between two sentences**, rather than just measuring similarity.

Unlike traditional embedding similarity approaches, OSBERT learns to classify relationships such as:
- **same** → semantic equivalence / entailment  
- **opposite** → contradiction  
- **limited** → related but not equivalent  

---

## Key Idea

Instead of comparing embeddings using cosine similarity, OSBERT leverages the BERT encoder to **learn relational semantics directly**.

Input format: