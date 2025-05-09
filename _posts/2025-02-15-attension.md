---
title: 'Delve into the Attention Mechanisum'
date: 2025-02-15
permalink: /posts/2025/02/attension/
tags:
  - GenAI Basics
  - AI explains AI

---

The **Attention Mechanism** is the core idea behind modern **large language models (LLMs)**. It allows models to focus on **important words in a sentence** while ignoring irrelevant details.  

Transformers use **self-attention** and **cross-attention** to process text, making them the foundation for models like **BERT, GPT, and LLaMA**.  


## 0. What is Attention?
Attention is a technique that helps models determine **which parts of the input are important** when making predictions.  Instead of reading every word equally, the model **focuses on key parts** to extract relevant information.  

** Attention Formula**
Given an input sequence X, we compute three matrices:
Inputs:
- Query (Q): What are we looking for?
- Key (K): What do we have in memory?
- Value (V): What information should we retrieve?

$$
\text{Attention}(Q, K, V) = \text{Softmax} \left(\frac{QK^T}{\sqrt{d_k}}\right) V
$$
Outputs:

- \\( QK^T  \\) → Measures similarity between queries and keys (how relevant a token is).
-	\\( sqrt{d_k} \\)  → Scaling factor to prevent large values (stabilizes training).
- \\( softmax()  \\)→ Converts scores into probabilities (attention weights for V).
-  Weighted Sum →  Final word representation is a sum of important words 

**Takeaway:** Attention **helps models understand contextual relationships**, allowing for more **meaningful text generation** by prioritizing relevant information.

----
## 1. Self-Attention
Unlike traditional models that process words sequentially, **self-attention allows every word to interact with every other word** in the sentence.  

- Helps maintain **contextual relationships** between words within the same sentence.
- Allows words to influence each other's representation, improving coherence in generated text.

**Example:**  
Consider the sentence: "The dog chased the cat because it was fast.". What we care about is Who does "it" refer to?. Self-attention helps the model identify that "it" likely refers to the cat, given the prior context.

**Takeaway:** Self-attention **allows models to capture long-range dependencies** in text, making LLMs more powerful.

---
## 2. Multi-Head Attention: Learning Multiple Perspectives
Instead of using **one** attention mechanism, multi-head attention **splits the input into multiple heads**,  applies attention separately, and concatenates results. Each head **focuses on different relationships** in the sentence.
- **Expands learning capacity** by running multiple self-attention mechanisms in parallel. Reduces information bottleneck (splitting across heads = better learning).
- **Divides embeddings into multiple subspaces**, each learning different aspects of linguistic structure and captures multiple perspectives (e.g., short-term & long-term dependencies).

**Example:**
- If the embedding dimension is **512**, splitting it into **8 heads** means each head operates on **64-dimensional subspaces**.
- This enables different attention heads to capture **various language attributes** simultaneously.

**Takeaway:** **Multi-head attention enables Transformers to process complex patterns and relationships in parallel**, improving both accuracy and efficiency.



## 4. Cross-Attention: Connecting Input & Output 
Cross-attention **helps different inputs interact**—used in **encoder-decoder models** like **T5, BART, and multimodal AI**.  It is esential in multimodal models, retrieval-augmented generation (RAG), and personalized AI chatbots.

- Q (Query) comes from the decoder (the response being generated)
- K, V (Key, Value) come from the encoder (the input sequence).

#### **Examples of Cross-Attention in Applications:**

| **Application** | **Query (Q)** | **Key (K)** | **Value (V)** |
|----------------|--------------|-------------|---------------|
| **Multi-turn Dialogue** | Current user input | Previous conversation history | Relevant past responses |
| **Retrieval-Augmented Generation (RAG)** | User query | Retrieved documents | Contextualized knowledge from retrieval |
| **Chatbots (Task-Oriented AI)** | User intent (e.g., "Book a flight") | Available API functions (e.g. flight and pricing API response) | Retrieved structured data (flight details) |
| **Personalization & Memory** | User preferences | Past interactions | Customized response based on prior behavior |

Takeaway: Cross-attention integrates external knowledge and conversation history, making AI interactions more relevant and context-aware.

---

## 5. FlashAttention:  Making Attention Faster
The **biggest problem with self-attention** is that it’s **slow and memory-intensive**. FlashAttention **solves this by optimizing how softmax is computed**.

### **Why is Regular Attention Slow?**
- **Stores a huge matrix \\( QK^T \\) in memory** → Too big for large models.  
- **Quadratic Complexity \\( O(n^2) \\)** → Longer sentences slow everything down.  

### **How FlashAttention Works**
** From the original softmax **
$$
    softmax(x_i) = \frac{e^{x_i}}{\sum_{j=1}^n e^{x_j}}
$$

Where:
- The output \\( \mathbf{s} \\) is a probability distribution, meaning \\( 0 \leq s_i \leq 1 \\) and \\( \sum_{i=1}^n s_i = 1 \\).
- The softmax function emphasizes larger values in \\( \mathbf{x} \\) due to the exponential function, making it suitable for classification tasks.

** Softmax approximation in Flash attenstion**
$$
    softmax(x_i ) \approx \frac{e^{x_i -max(x)}}{\sum_{j=1}^n e^{x_j -max(x)}}
$$

1. **Computes Softmax in smaller chunks** → Saves memory.  
    - Recomputes softmax block-by-block instead of storing the full  QK^T  matrix. Subtract the maximum score from each row before exponentiating to avoid overflow/underflow during softmax computation.
2. **Uses GPU-friendly tiling** → Runs faster on hardware.  
    - Uses tiling (sparse updates) to avoid redundant memory access.
3. **Reduces memory usage from \\( O(n^2) \\) to \\( O(n) \\)** → Handles longer text sequences.  
    - Fuses matrix operations to run efficiently on GPUs (CUDA optimized).

**Takeaway:** FlashAttention **makes LLMs more efficient, allowing them to process longer text inputs**.


## Learn with Code
```python

import numpy as np

def softmax(x):
    """
    Compute the softmax of each row of the input matrix.

    Args:
        x (numpy.ndarray): Input matrix of shape (N, M)

    Returns:
        numpy.ndarray: Softmax output of shape (N, M)
    """
    exp_x = np.exp(x - np.max(x, axis=-1, keepdims=True))  # Stability fix
    return exp_x / np.sum(exp_x, axis=-1, keepdims=True)

def attention (query, key, value):
    """
    Compute attention output as a weighted sum of the value matrix.

    Args:
        query (numpy.ndarray): Query matrix of shape (N, d_k)
        key (numpy.ndarray): Key matrix of shape (M, d_k)
        value (numpy.ndarray): Value matrix of shape (M, d_v)

    Returns:
        numpy.ndarray: Attention output of shape (N, d_v) 
    """
    # Compute scaled dot-product attention
    d_k = query.shape[-1]  # Dimensionality of the key
    scores = np.dot(query, key.T) / np.sqrt(d_k)  # Scaled dot product
    weights = softmax(scores)  # Apply softmax to get attention weights
    output = np.dot(weights, value)  # Weighted sum of value vectors
    return output

# Example usage
if __name__ == "__main__":
    # Example matrices
    Q = np.array([[1, 0, 1]])  # Query vector (1 x d_k)
    K = np.array([[1, 0, 1], [0, 1, 0], [1, 1, 0]])  # Key vectors (M x d_k)
    V = np.array([[1, 2], [0, 3], [1, 1]])  # Value vectors (M x d_v)

    # Attention output
    output = attention(Q, K, V) # N x d_v
    print("Attention Output:\n", output) #  [[0.83205655 1.86878374]]
    
```


---

# Final Takeaways
- Attention helps models focus on important words.  
- Self-attention captures contextual relationships. 
- Multi-head attention allows different perspectives.  
- Cross-attention is useful for chatbots and multimodal AI. 
- FlashAttention optimizes speed and memory for large models. 


---
🤖 Disclaimer: This post is inspired by [*Educative.io AI* learning course](https://www.educative.io/explore?aff=BwW8), and generated with AI-assisted but reviewed and refined by [Dr. Rebecca Li](https://xiaoyang-rebecca.github.io/), blending AI efficiency with human expertise for a balanced perspective.