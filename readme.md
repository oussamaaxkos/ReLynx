# ReLynx — Relation-Aware BERT

ReLynx is a fine-tuned BERT model designed to predict the **semantic relationship between two sentences**, rather than just measuring similarity.

Unlike traditional embedding similarity approaches, ReLynx learns to classify relationships such as:
- **same** → semantic equivalence / entailment  
- **opposite** → contradiction  
- **limited** → related but not equivalent  

---

## 🚀 Key Idea

Instead of comparing embeddings using cosine similarity, ReLynx leverages the BERT encoder to **learn relational semantics directly**.

**Input format:**
```
[CLS] sentence1 [SEP] sentence2 [SEP]
```

The `[CLS]` token acts as a **relation representation vector** (similar to a custom `[REL]` token).

---

## 🧠 Model

- Base model: `bert-base-uncased` (default)
- Task: Sentence-pair classification
- Head: Linear classification layer on top of `[CLS]`

### 🔄 Supported pretrained variants
You can easily swap the base model:

- `bert-base-uncased` — solid baseline  
- `distilbert-base-uncased` — faster & lighter  
- `roberta-base` — often better performance  
- `roberta-large-mnli` — high accuracy (NLI-pretrained)  
- `cross-encoder/nli-MiniLM2-L6-H768` — efficient & strong  

---

## 📊 Training Data

ReLynx can be trained on Natural Language Inference datasets:

- **SNLI / MultiNLI**

**Label mapping:**

| ReLynx Label | NLI Label        |
|--------------|------------------|
| same         | entailment       |
| opposite     | contradiction    |
| limited      | neutral          |

Custom datasets format:
```
label[SEP]sentence1[SEP]sentence2
```

**Examples:**
```
opposite[SEP]The sun rises in the east.[SEP]The sun sets in the west.
same[SEP]Dogs are mammals.[SEP]Canines belong to the mammal family.
limited[SEP]Water is wet.[SEP]Fire is hot.
```

---

## ⚙️ Training

Built using Hugging Face Transformers:

- Learning rate: `2e-5`
- Batch size: `16`
- Epochs: `3–5`
- Loss: Cross-entropy
- Metrics: Accuracy + Macro F1

---

## 🔍 Inference

```python
from transformers import pipeline

classifier = pipeline(\"text-classification\", model=\"path/to/ReLynx\")

result = classifier(\"The car is fast. ||| The car is slow.\")
print(result)  # [{'label': 'opposite', 'score': 0.98}]
```

---

## 📈 Applications
- Duplicate detection
- Fact checking & contradiction detection
- Chatbot consistency validation
- Semantic search & clustering
- QA answer verification
- Knowledge base cleaning

---

## ⚠️ Limitations
- Works at sentence level (not full documents)
- No explanation of predictions (classification only)
- Domain adaptation required for specialized fields
- \"limited\" class is inherently ambiguous

---

## 💡 Future Work
- Add more fine-grained relation types
- Train on domain-specific corpora (medical, legal, etc.)
- Add explainability (attention visualization, rationale extraction)
- Deploy as an API or service

---

## 📌 Summary

ReLynx turns BERT into a relation-aware encoder, enabling deeper semantic understanding beyond similarity scoring.
