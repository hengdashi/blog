---
title: "The Training Methodology Behind ChatGPT"
description: "A read through of the InstructGPT paper."
date: 2023-01-16
categories: ["llm"]
draft: true
---


[ChatGPT](https://openai.com/blog/chatgpt/) has brought so many media attentions and is making waves in the machine learning community.
Even though the ChatGPT paper has not come out yet, the training methodology of ChatGPT is actually the same as [InstructGPT](https://openai.com/blog/instruction-following/#birds-not-real).
I read through the InstructGPT paper, and found it very interesting with connections to ideas in the IR field.

There are probably many tutorials online explaining how InstructGPT works in plain English.
But in this blog post, I will try it explain the model as they were humans to help making sense of it easily.

{{< alert >}}
**Warning!** This blog post is somewhat math-oriented, and has no means to serve as a ChatGPT tutorial to folks with no machine learning background.
{{< /alert >}}


## Preliminary

**ChatGPT** is the latest model in the GPT series.
It is a specialized conversational model trained on their newest language model GPT 3.5.

In NLP, language model has a special meaning, it is not just a model trained on human language.
Simply put, language models are instructed to achieve exact one goal: **predict some words given some context of the passage.**

In the case of GPT series, these models are instructed to **predict the next word in the passage given all words before.**
Another big series of model developed by Google -- BERT, are instead instructed to **predict 15% of the words in a passage given all words that are not masked.**

This task is so simple that these models can be trained in unsupervised fashion.

{{< alert >}}
Using the analogy as if the GPT models are humans.
This is exactly as if **we as parents are watching our child (the model) tirelessly reading and learning every document on the Internet.**
{{< /alert >}}

Below is a more formal definition of the standard language model loss (used by the GPT series):

Given the tokens set ( \\(\mathcal{U} = \\{ u_1, \dots, u_n \\}\\) ), where \\( n \\) is size of the vocabulary, below is the loss function for GPT series:
$$
\begin{equation}
  \nonumber
  L_1(\mathcal{U}) =
    \sum_{i} \log P ( u_i \mid u_{i-k}, \dots, u_{i-1}; \Theta )
\end{equation}
$$

This loss calculates the log likelihood of predicting the token \\( u_i \\) given all tokens appeared previously from \\( u_{i-1} \\) to \\( u_{i-k} \\).
This conditional probability \\( P \\) is modeled by the giant neural networks in GPT series and the networks weights are represented by symbol \\( \Theta \\).


## Methodology

One problem with even a huge model of 175 billion parameters like GPT-3 is that these models can output useless or even toxic responses.

Given how the model is trained, it is intuitive to understand a child who has read every single book in the world may be heavily influenced by toxic and wrong contents they may have read.

In the **InstructGPT** paper, they call this an alignment problem where the responses of the language model does not align with our expectation that the model should be always helpful and friendly in conversations.

The core question is then, how to solve this problem?

The simplest solution is to tell the model what they should have responded to a human.
And the simplest way to do it is to gather human responses to the questions and use it to train the model with the language model loss by concatenating the question and the human response.

However, it is usually not enough given the amount of manual effort and budget needed to train such model, because having a human labeler to read each question and give answer is very time consuming.

Maybe, it would be easier for the labeler to tell which answer is simply better than other answers?

Given the nature of next token prediction, it is in fact very easy to have the model outputting more than one answer to the question.

{{< katex >}}

![chatgpt_training_overview](./images/ChatGPT_Diagram.svg "ChatGPT Training Overview. source: https://openai.com/blog/chatgpt/")

$$
\begin{equation}
  \nonumber
  \mathrm{loss}(\theta) =
    - \frac{1}{K \choose 2} E_{(x, y_w, y_l) \sim D}
    \Bigg[\log{\bigg(
      \sigma\Big(
        r_{\theta}(x, y_w) - r_{\theta}(x, y_l)
      \Big)
    \bigg)}\Bigg]
\end{equation}
$$

abc

$$
\begin{aligned}
  \mathrm{objective}(\phi)
    =&
    E_{(x, y) \sim D_{\pi_\phi^{RL}}}
    \Bigg[ r\_\theta(x, y) - \beta \log{ \bigg( \frac{\pi_\phi^{RL}(y \mid x)}{\pi^{SFT}(y \mid x)} \bigg) } \Bigg] + \\\
    &
    \gamma E_{x \sim D_{\mathrm{pretrain}}} \Bigg[ \log{ \bigg(\pi_\phi^{RL}(x) \bigg) } \Bigg]
\end{aligned}
$$

