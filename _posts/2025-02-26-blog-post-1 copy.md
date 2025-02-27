---
title: 'From Text Transformer to Vision Transformer'
date: 2025-02-24
permalink: /posts/2025/02/blog-post-1/
tags:
  - ML Basic
---

From Text Transformer to Vision Transformer
======

## Introduction
Tranformer model is widely used in large lanuage model develepoment, and with the more need for muilmodal model usage 

---

## Transformal Network for text generation
The tranformer model mainly consist of the following components:
1. Text Tokendization and Encoding
   Conver each token (a word or phrase) into a fixed length of vector (embedding e.g. with the size of 512)
   The purpose for word  embedding is to group the word with simialr meaning to close distance in the vetor space rather than their wording differnce. e.g. "cat" and "car" are close from each other in the text space, where as "cat" and "dog" are closer to each other vector space because they have the similar attribute of pets thus their vector space is closer. 
   *Extension: Text encoding is also a common preprocessing  method for search and recommendation. It can also used pre-existed models 
2. Positional Encoding:
   Sinc cos function to prepresend the contextual infromation of sentence , i.e.g the posotion of the toklens (< formular and the visual graph of positional encoding>). E.g. "the cat lies on sofa", and "soft lies on cat" would only make difference after positional encoding
   Output of it is would be the numberical summiation of text embedding + Positional Encoding
3. Attension mechanism
   The goals is to measures the similarity and importance of elements which allows models to selectively focus on crucial inputs leading to informed predictions and decisions. The way to measure the importance of the encndoers is called 

$$
Attension(Value, Key(index of the value), Query) = Softmax(\frac{Q K^T}{\sqrt {d_k}} ) V
$$
- Q: input embedding
- K: contextual meaning
- V: actual value

 - Take home: attention mechanism is like a guiding light that helps it understand and respond coherently in conversations like ChatGPT.

   3.1. Self Attension
     Measure the similarity score of each words with other works .  Helps the model understand relationships between words in a sentence (e.g., resolving coreferences, maintaining sentence context).
     - Q: word embeddinge. 512 of works
     - K: word itself as a key in dictional
     - V: the infromation to determine, inm self attension also the same work embedding
    3.2. Muil head attension: Enable the model to learn long sentence , and enable parellel processing to imrpve the effiicency 
    e.g. for 6 heads in a 512 embedding, then each head woudl be 64

    3.3 Cross attension
    Get the Importance socoer to anohiiter set of data. e.g for translation  purpose. Helps the model incorporate external information (e.g., previous conversation turns, knowledge bases, user intent).
    - Q: output sentence , e.g. 
    - K: query key ( e.g. word, position index)
    - V: innput sentence

    example on Rag, mutli conversatrion 

    # QKV Roles in Conversational AI
    | **Application**           | **Query (Q)**                          | **Key (K)**                                       | **Value (V)**                                      |
    |--------------------------|--------------------------------------|------------------------------------------------|------------------------------------------------|
    | **Self-Attention (Single Turn Understanding)** | Current word embedding                | Other word embeddings in the same sentence    | Contextualized representation of words         |
    | **Cross-Attention (Multi-Turn Dialogue)**      | Current user input                     | Previous conversation history                 | Relevant past responses for context            |
    | **Cross-Attention (Knowledge Retrieval - RAG)** | User question                          | Pre-indexed knowledge chunks (e.g., Wikipedia, KBs) | Factual information from retrieved sources      |
    | **Cross-Attention (Task-Oriented Chatbot)**   | User request (e.g., "Book a flight")   | Available API functions or database queries   | Retrieved task-related information (e.g., flight details) |
    | **Cross-Attention (Personalization & Memory)** | User query                             | Past interactions & user preferences         | Customized response based on prior behavior    |

## Vision Transformer
The main difference of the Vision transformer to text transformer is in the 1 and the 2 step of how to prepare the uanges,
1. Image embedding
   Rather than tokenziing the texts, image embedding used patch embedding that flatten the 2D pixeles into 1D vector rahter than linear project.
   | | |
   -----
   | | |
2. Positional Encoding used 2 dimentions to represent the tiles, e..g (0,0), (0,1), (1,0), (1,1) represent the top left, top right and bottom left, bottom right tiles when devide the image into 4 tiles.


---


---

## 5️⃣ Summary

