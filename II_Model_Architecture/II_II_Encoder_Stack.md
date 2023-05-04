### Notes:
- Each layer output has constant dimension: `d_model` (Originally 512)
  - All key ops are dot-products, dimensions are stable
  - Reduces num ops to calculate

# Encoder Stack

# I. Input Embedding (Standard transudction model)

**Core question:** How do we go from text to numeric or vector representation?

- Transduction models - Basically a translator from one data type to another representation (i.e. texts to number representations)

## 1. Tokenization
- The tokenizer will convert some sequence into a tokenized representation.

- **Example:**
  - Input: "The transformer is an innovative nlp model"
  - Output: [1996, 4924, 2421, 9542, 4325, 3432, 53243, ...]

## 2. Embeddings
- Text is now represented in integers, now contextual information will be applied (something like word2vec, called a *skip-gram*)

- **Example:** This example is on a single word, but we'd generate one of these for each token in the input sequence
  - Input: [1996] --> (Tokenized form of "The")
  - Output: [[-0.012, 0.543, 0.943, ...]] --> (each 1d array of 512 length - based on `d_model`)

### Aside: How do we check if the encoding is right?
- Cosine similarity between two embeddings of similar meaning should yield a result close to 1
- Cosine similarity tells us how close two vectors are from each other.

- **Example:**
  - Red = [[0.321, 0.543, ....]] (length=512)
  - Blue = [[0.21, 0.431, ....]] (length=512)
  - Cosine_similarity(Red, Blue) == [[0.9998991]]

# II. Positional Encoding

**Core question:** How do we encode the position of a word in a sequence?
- More specifically, if we have a 512-element representation of a word, how do we add it's positional information to all 512 elements?!

Original transformer approach: Sin/Cosine

- Odd indices of embedding vector: `2i + 1`
- Even indices of embedding vector: `2i`
- Position of the word in the sequence: `pos`

For each token, now an embedding, a new positional encoding is created.

```
Embedding Vector = [[-0.012, 0.543, 0.943, ...], [embedding for word at index 1], [embedding for word at index 2], ...];
Positional Embedding = [PE(0), PE(1), ...];

Positional Encoding(x) = Positional Embedding(x) + Embedding Vector(x) * kSomeConstant;
```

- **Example:**
  - Input: [[-0.012, 0.543, 0.943, ...]] --> (each 1d array of 512 length - based on `d_model`)
    - This is just the word embeddings
  - Output: Array of same size with different numeric values
    1. A positional encoding vector is generated
    2. The positional encoding vector and embedding vector are combined and output

# III. Multi-Head Attention

The full breakdown of this layer can be found in the `Multi_Headed_Attention.ipynb` notebook, but I have a breakdown here.

## III.I. Heads are created

We usually create something like 8 heads, each of which will do the same operational steps:

1. A Key, Value, and Query Matrices set is created
  - Each of these matrices is of same dimensionality as input embeddings
2. K, V, Q matrices are generated by matrix multiplying each input * key, value, and query
3. The transformer equation is used to calculate an attention score for each input
  - The basic equation is (Q * K.transpose()) / sqrt(number_of_inputs)
4. These attention scores are scaled via a softmax function
5. The attention scores are multiplied by V
6. The attention scores are summed for each input so you end up with the same dimensionality as the previous array
7. Each of these inputs from the heads are concatenated
8. Normalization occurs

## III.II. Feedforward Neural Network

Pretty basic, contains two layers, applies a ReLu activation function
- Two convolutions with size 1 kernel.


# My sparknotes summary of how transformer encoder stacks work:

1. Generate embeddings
  - Input: Sequence of words/tokens
  - Output: Embeddings of each word/token (an embedding is a vector)
  - **TLDR:** A skip-gram model is used to generate embeddings from each token in a sequence

2. Add positional info
  - Input: Embeddings of each word/token
  - Output:  Embeddings of each word/token but each word/token has some small value added to it representative of it's position
  - **TLDR:** Embeddings are transformed to also encode positional info 

3. Self-Attention is applied
  - Input: Embeddings with positional info
  - Output: Embeddings with positional info + attention score
  - **TLDR:** Attention score is just the token value, and the values of all tokens that relate to that token value, combined together

4. All self-attention heads have their output concatenated

5. Feed-Forward Neural Network

6. Layer Normalization

* Residual information is applied throughout the stack

# My sparknotes summary of what key, query, value mean

TOKEN: Used here to refer to the token under observation for the current iteration

- Query: The TOKEN we are observing
- Key: Matrix containing the words related to TOKEN
- Value: The value of all words related to TOKEN and TOKEN

Combined together to form: Attention scores for each TOKEN