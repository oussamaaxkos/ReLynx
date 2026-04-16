# ReLynx — Relation-Aware BERT

ReLynx is a small experiment around a simple idea: instead of measuring similarity between two sentences, why not train BERT to understand the *relationship* between them?

The model is fine-tuned to take a pair of sentences and predict how they relate whether they mean the same thing, contradict each other, or are just loosely related.

---

## What it does

Given two sentences, ReLynx outputs one of three labels:

- **same** → they express the same idea (or one entails the other)  
- **opposite** → they contradict each other  
- **limited** → they are related, but not equivalent  

This is closer to how we actually reason about text than cosine similarity between embeddings.

---

## How it works

The setup is straightforward. It uses a standard BERT encoder with the usual sentence-pair format:
[CLS] sentence1 [SEP] sentence2 [SEP]

The `[CLS]` token ends up acting as a compact representation of the relationship between the two inputs. A simple classification head on top of it predicts the label.

No custom architecture, just a different way of using the encoder.

---

## Training

Our model was trained on the **MultiNLI** dataset.

The model can be trained directly on NLI datasets like SNLI or MultiNLI by remapping labels:

- entailment → **same**  
- contradiction → **opposite**  
- neutral → **limited**

You can also plug in your own data as long as it follows a simple format:
label[SEP]sentence1[SEP]sentence2


In practice, starting from **our MultiNLI-trained model** and then fine-tuning on a smaller custom dataset works well.

---

## Example

```python
predict_relation(
    "The car is fast.",
    "The car is slow."
)
# → opposite
