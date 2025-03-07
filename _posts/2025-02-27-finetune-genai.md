---
title: "Fine-Tune GenAI Models"
date: 2025-02-27
permalink: /posts/2025/02/finetune-genai/
tags:
  - GenAI Basics
  - AI explains AI

---

Fine-tuning **Generative AI (GenAI) models** can be categorized into two main approaches:  
1. **Non-Parametric Fine-Tuning**: Modifying model behavior **without changing its parameters** (e.g., **ICL, RAG**).  
2. **Parametric Fine-Tuning**: Updating the model’s **internal parameters** (e.g., **Full Fine-Tuning, LoRA**).  

---

## **Comparison: Black-Box vs. White-Box Fine-Tuning**  

| **Method**        | **Type**  | **Changes Model Weights?** | **Training Cost** | **Storage Cost** | **Use Case** |
|------------------|----------|----------------|--------------|--------------|--------------------|
| **In-Context Learning (ICL)** | Non-Parametric | ❌ No  | Low  | None  | Few-shot prompting |
| **Retrieval-Augmented Generation (RAG)** | Non-Parametric | ❌ No | Medium  | Moderate  | Dynamic knowledge retrieval |
| **Full Fine-Tuning** | Parametric | ✅ Yes | High | High | Adapting to new tasks |
| **LoRA (Low-Rank Adaptation)** | Parametric | ✅ Yes (small updates) | Low | Low | Efficient fine-tuning |

---

# **1. Non-Parametric Fine-Tuning**  

Non-parametric methods **do not modify model weights**. Instead, they manipulate the **input or external retrieval process** to achieve better results.  

## **1.1 In-Context Learning (ICL)**  
In-Context Learning (ICL) allows a model to learn **new tasks on the fly** by providing **examples within the prompt**.  
- The model **does not require retraining**.  
- Works best when the task is **aligned with pretraining data**.  
- **Example:** Providing **few-shot examples** before asking a question.  

### **Example: Few-Shot Prompting**
Translate the following English phrases into French:
1.	“Good morning” → “你好”
2.	“Thank you” → “谢谢”
3.	“Where is the train station?” → ？

The model **infers the pattern** and completes the response as **"车站在哪儿 ?"**.  

---

## **1.2 Retrieval-Augmented Generation (RAG)**  
Retrieval-Augmented Generation (RAG) combines **pretrained LLMs** with **external knowledge retrieval** to improve accuracy.  

- **How it works:**  
  1. **Query Expansion:** Convert the input into a query.  
  2. **Document Retrieval:** Search a knowledge base (e.g., FAISS, Pinecone).  
  3. **Response Generation:** The retrieved documents **condition** the LLM's response.  

- **Advantages:**  
  - Reduces hallucinations by **grounding responses in factual sources**.  
  - Enables dynamic updates **without retraining the model**.  

- **Example:**  
  - Instead of asking **"Who is the CEO of OpenAI?"** to an LLM,  
  - The system retrieves **recent articles** and feeds the information into the model before generating a response.  

---

# **2. Parametric Fine-Tuning**  

Parametric fine-tuning **updates model weights** to improve performance on specific tasks.  

## **2.1 Full Fine-Tuning**  

Full fine-tuning is typically used when the original model domain and the target domain differ substantially, and there is sufficient labeled data for effective retraining. However, it comes with the risk of catastrophic forgetting, where the model loses previously learned general knowledge. In specialized fields like law or medicine, this can be beneficial, as the model needs to prioritize domain-specific expertise over general or noisier knowledge.

- Updates all model parameters, allowing complete adaptation to a new domain.
-	High computational cost and storage requirements due to the large number of parameters being modified.
-	Best suited for domains with significant differences from the original training data, such as legal, medical, or scientific LLMs.

---

## **2.2 LoRA (Low-Rank Adaptation)**  

### **LoRA Formula:**  
Instead of updating the full weight matrix \\( W \\), LoRA **adds a low-rank update**:

$$
W = W_0 + \lambda \Delta W , where \Delta W  \approx  A \times B
$$
 
- \\( W_0 \\) is the **pretrained weight matrix**.  
- \\( A \) and \\( B \) are **low-rank matrices** of dimensions **\( d \times k \)** and **\( k \times n \)**.  
- \\( k \\) is the **rank** (significantly smaller than \\( d, n \\)).  


This **approximates** the weight update, reducing **trainable parameters significantly**.

---

## **Why Does LoRA Reduce Parameters?**  

Instead of updating **all** parameters in \\( W \\), LoRA **approximates the weight changes** using much smaller matrices \\( A \\) and \\( B \\).  

### **Example: Decomposing \\( W \\) into \\( A \times B \\)** 
A simple example of k = 2

$$
A_{4\times 2}=
\begin{bmatrix}
a_{1,1} & a_{1,2} \\
a_{2,1} & a_{2,2} \\
a_{3,1} & a_{3,2} \\
a_{4,1} & a_{4,2}
\end{bmatrix}
$$

$$
B_{2\times 5} =
\begin{bmatrix}
b_{1,1} & b_{1,2} & b_{1,3} & b_{1,4} & b_{1,5} \\
b_{2,1} & b_{2,2} & b_{2,3} & b_{2,4} & b_{2,5}
\end{bmatrix}
$$

The multiplication \( A \times B \) produces:

$$
\Delta W_{4\times 5} =
\begin{bmatrix}
a_{1,1} b_{1,1} + a_{1,2} b_{2,1} & a_{1,1} b_{1,2} + a_{1,2} b_{2,2} & \dots & a_{1,1} b_{1,5} + a_{1,2} b_{2,5} \\
a_{2,1} b_{1,1} + a_{2,2} b_{2,1} & a_{2,1} b_{1,2} + a_{2,2} b_{2,2} & \dots & a_{2,1} b_{1,5} + a_{2,2} b_{2,5} \\
a_{3,1} b_{1,1} + a_{3,2} b_{2,1} & a_{3,1} b_{1,2} + a_{3,2} b_{2,2} & \dots & a_{3,1} b_{1,5} + a_{3,2} b_{2,5} \\
a_{4,1} b_{1,1} + a_{4,2} b_{2,1} & a_{4,1} b_{1,2} + a_{4,2} b_{2,2} & \dots & a_{4,1} b_{1,5} + a_{4,2} b_{2,5}
\end{bmatrix}
$$

If \\( W \\) is **10,000 x 10,000**, fine-tuning **all** parameters requires **100M parameters**.  
With LoRA, if we choose a **rank \\( k = 32 \\)**:  
- \\( A \) is **10,000 × 32**  
- \\( B \) is **32 × 10,000**  
- **Total parameters = 640K** instead of **100M**  

This leads to **99% fewer parameters to update**, making fine-tuning **much cheaper** while still adapting the model effectively.  



---

# **Final Takeaways**  
- **Non-Parametric Fine-Tuning (ICL, RAG)** keeps model weights **unchanged** but improves adaptability.  
- **Parametric Fine-Tuning (Full, LoRA)** modifies model parameters, with LoRA being **efficient and low-cost**.  
- LoRA achieves similar performance to **full fine-tuning** with a **fraction of the computational cost**.  


---
🤖 Disclaimer: This post was generated with the help of AI but reviewed, refined, and enhanced by [Dr. Rebecca Li](https://xiaoyang-rebecca.github.io/), blending AI efficiency with human expertise for a balanced perspective.