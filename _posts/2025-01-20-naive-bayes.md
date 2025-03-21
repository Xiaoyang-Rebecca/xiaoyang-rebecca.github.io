---
title: 'Naive Bayes Theory and Example on Spam Email Detection'
date: 2025-01-20
permalink: /posts/2025/01/naive-bayes/
tags:
  - ML Basics
  - Learn with Code
  - AI explains AI

---

In this blog post, we’ll explore Naive Bayes, a simple yet powerful algorithm used for classification tasks like **spam detection**. We’ll break down the theory, provide intuitive examples, and show you how to implement it from scratch in Python. Whether you're new to machine learning or preparing for an interview, this guide will help you understand Naive Bayes in a simple and concise way.

---

## **1. What is Naive Bayes?**

Naive Bayes is a probabilistic machine learning algorithm based on **Bayes' Theorem**. It’s called "naive" because it makes a strong assumption: all features (words, in the case of text) are **independent** of each other given the class. Despite this simplification, Naive Bayes works surprisingly well for many real-world problems, especially text classification.

---

## **2. Bayes' Theorem**

Bayes' Theorem is the foundation of Naive Bayes. It describes how to update the probability of an event based on new information. Here’s the formula:

$$
P(C | X) = \frac{P(X | C) \cdot P(C)}{P(X)}
$$

Where:

- \\( P(C\|X)\\): The probability of class \\( C \\) given the features \\( X \\) (called the **posterior probability**).
- \\( P(X\|C)\\): The probability of features \\( X \\) given class \\( C \\) (called the **likelihood**).
- \\( P(C) \\): The probability of class \\( C \\) (called the **prior probability**).
- \\( P(X) \\): The probability of features \\( X \\) (acts as a normalizing constant).

---

### **Intuitive Example**

Imagine you’re trying to classify emails as **spam** or **ham** (not spam). Bayes' Theorem helps you answer:  
*"Given the words in this email, what’s the probability it’s spam?"*

---

## **3. Naive Bayes Assumption**

Naive Bayes assumes that all features (words) are **independent** given the class. This means:

$$
P(X | C) = P(x_1 | C) \cdot P(x_2 | C) \cdot \ldots \cdot P(x_n | C)
$$

Where:
- \\( x_1, x_2, \ldots, x_n \\): The words in the email.


Each word \\( x_i \\) contributes independently to the probability of the class \\( C \\).

---

## **4. Math Behind Naive Bayes**

**Posterior Probability**

For classification, we compute the posterior probability for each class and choose the one with the highest probability:

$$
P(C | X) \propto P(C) \cdot \prod_{i=1}^n P(x_i | C)
$$

Since \\( P(X) \\) is the same for all classes, we ignore it and focus on the numerator.

---

**Log-Probability**

To avoid numerical underflow (multiplying many small probabilities), we use **logarithms**:

$$
\log P(C | X) \propto \log P(C) + \sum_{i=1}^n \log P(x_i | C)
$$

---

**Prior Probability (\\( P(C) \\))**

The prior probability \\( P(C) \\) is the probability of a class \\( C \\) in the training data:

$$
P(C) = \frac{\text{Number of emails in class } C}{\text{Total number of emails}}
$$

---

**Likelihood (\\( P(x_i\|C) \\))**

The likelihood \\( P(x_i\|C) \\) is the probability of a word \\( x_i \\) given the class \\( C \\). We use **Laplace smoothing** to handle unseen words:

$$
P(x_i| C) = \frac{\text{Count of } x_i \text{ in class } C + 1}{\text{Total words in class } C + \text{Vocabulary size}}
$$

---

## **5. Learn with Coding**

```python
import re
from collections import defaultdict, Counter
import math

class NaiveBayesSpamClassifier:
    def __init__(self):
        self.prior = {}  # Prior probabilities for each class
        self.likelihood = {}  # Likelihood of each word given the class
        self.vocab = set()  # Vocabulary (unique words in the training data)

    def preprocess(self, text):
        """
        Preprocess the text: lowercase, remove punctuation, and tokenize.
        """
        text = text.lower()  # Convert to lowercase
        text = re.sub(r"[^a-zA-Z0-9\s]", "", text)  # Remove punctuation
        tokens = text.split()  # Tokenize
        return tokens

    def train(self, emails, labels):
        """
        Train the Naive Bayes classifier.
        """
        # Count the number of spam and ham emails
        num_emails = len(emails)
        num_spam = sum(labels)
        num_ham = num_emails - num_spam

        # Calculate prior probabilities
        self.prior["spam"] = num_spam / num_emails
        self.prior["ham"] = num_ham / num_emails

        # Initialize word counts for each class
        spam_word_counts = Counter()
        ham_word_counts = Counter()
        self.vocab = set()

        # Count words in spam and ham emails
        for email, label in zip(emails, labels):
            tokens = self.preprocess(email)
            self.vocab.update(tokens)  # Add words to vocabulary
            if label == 1:  # Spam
                spam_word_counts.update(tokens)
            else:  # Ham
                ham_word_counts.update(tokens)

        # Calculate likelihoods using Laplace smoothing
        total_spam_words = sum(spam_word_counts.values())
        total_ham_words = sum(ham_word_counts.values())
        vocab_size = len(self.vocab)

        self.likelihood["spam"] = {}
        self.likelihood["ham"] = {}

        for word in self.vocab:
            # Laplace smoothing: add 1 to numerator and vocab_size to denominator
            self.likelihood["spam"][word] = (spam_word_counts.get(word, 0) + 1) / (total_spam_words + vocab_size)
            self.likelihood["ham"][word] = (ham_word_counts.get(word, 0) + 1) / (total_ham_words + vocab_size)

    def predict(self, email):
        """
        Predict whether an email is spam or ham.
        """
        tokens = self.preprocess(email)
        spam_score = math.log(self.prior["spam"])
        ham_score = math.log(self.prior["ham"])

        for word in tokens:
            if word in self.vocab:
                #	If word is not found, return 1e-10 (a very small value close to zero). Avoid log(0)
                spam_score += math.log(self.likelihood["spam"].get(word, 1e-10))  
                ham_score += math.log(self.likelihood["ham"].get(word, 1e-10))

        # Return the class with the higher score
        return 1 if spam_score > ham_score else 0

    def evaluate(self, emails, labels):
        """
        Evaluate the classifier's accuracy.
        """
        correct = 0
        for email, label in zip(emails, labels):
            prediction = self.predict(email)
            if prediction == label:
                correct += 1
        accuracy = correct / len(emails)
        return accuracy

```

# Example dataset
```python
emails = [
    "Congratulations! You've won a $1000 gift card. Click here to claim.",
    "Hi John, let's meet for lunch tomorrow.",
    "Get cheap meds online, no prescription needed!",
    "Reminder: Your meeting is scheduled for 10 AM.",
    "Earn money fast with this one weird trick!",
    "Can you send me the report by EOD?"
]

# Labels: 1 for spam, 0 for ham
labels = [1, 0, 1, 0, 1, 0]

# Train the classifier
classifier = NaiveBayesSpamClassifier()
classifier.train(emails, labels)

# Test the classifier
test_emails = [
    "Win a free iPhone! Click now!",
    "Hey, are we still on for the meeting?"
]

predictions = [classifier.predict(email) for email in test_emails]
print("Predictions:", predictions)  # Output: [1, 0]

# Evaluate accuracy
accuracy = classifier.evaluate(emails, labels)
print("Accuracy:", accuracy)  # Output: 1.0 (100% accuracy on training data)

```
## **6. Key Takeaways**
1. Naive Bayes is simple, fast, and works well for text classification.
2. It uses Bayes' Theorem and assumes feature independence.
3. Laplace smoothing handles unseen words.



---
🤖 Disclaimer: This post is inspired by [*Educative.io AI* learning course](https://www.educative.io/explore?aff=BwW8), and generated with AI-assisted but reviewed and refined by [Dr. Rebecca Li](https://xiaoyang-rebecca.github.io/), blending AI efficiency with human expertise for a balanced perspective.