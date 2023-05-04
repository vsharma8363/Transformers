# Attention

**TLDR:** Attention provides a token-to-token relationship representation for a sequence that takes into account the contextual information of that token (i.e. how it is used in the sentence)
- It will find a *dot-product* value for each token and all other tokens in the sentence

### Attention Is All You Need - Vaswani et. al
- Paper that gave birth to the transformer
  - Why was the transformer a big deal?
    - Trained faster than previous architectures
    - Obtained higher evaluation results

### Example usage
```
Consider some function `f(...)` that calculates the embedding value.

Traditional embedding: "I ate an apple"
  - "I" --> f("I")=0.1
  - "ate" --> f("ate")=0.2
  - "an" --> f("an")=0.3
  - "apple" --> f("apple")=0.4

Transformer embedding: "I at an apple"
  - "I" --> f("I") * f("I") + f("I") * f("ate") ....
  - "ate" --> f("ate") * f("I") + f("ate") * f("ate") ....
  - "an" --> f("an") * f("I") + f("an") * f("ate") ....
  - "apple" --> f("apple") * f("I") + f("apple") * f("ate") ....
```
