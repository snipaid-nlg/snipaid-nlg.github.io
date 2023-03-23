---
layout: post
title: "The power of Large Language Models: History and Concepts"
author:
- Hannah Greven
- Benjamin Rech 
---

## The power of Large Language Models: History and Concepts

![Powerful tree with roots spreading out](https://user-images.githubusercontent.com/36483428/227188899-7814cd19-ce9f-4e24-ad24-8e0f5ab33dd4.png)

Advances in the field of Natural Language Processing (NLP) lead to Large Language Models (LLMs) with strong generative capabilities. Trying to assist journalists with snippet generation requires building a basic knowledge of NLP and LLMs to automate the process.

NLP is a relatively new field of computer science that has developed rapidly from its beginnings in the 1950s. In this blog post, we'll explore how the field evolved from simple rule-based algorithms to the large pretrained language models we know today.

### Simple definition of a Language Model

A language model is an (artificially intelligent) model that has been trained to predict the next word or words in a text based on the preceding words. More formally, a language model is a probability distribution over sequences of words.

### A short history on how Language Models evolved

- **1947-1950:** Alan Turing first mentions the term "computer intelligence" and develops the "Turing Test," which becomes a foundation for research in NLP.
- **1950s-1960s:** Researchers experiment with rule-based systems for language translation and information retrieval, but these systems have limitations in handling the complexity and ambiguity of natural language. Due to exaggerated expectations, interest in language models declines for almost 20 years.
- **1980s-1990s:** The introduction of statistical techniques and machine learning algorithms, along with more powerful computers, leads to breakthroughs in NLP, including natural language understanding systems.
- **1990s-2000s:** The rise of the internet and the availability of vast amounts of text data lead to the development of corpus-based approaches to NLP, which rely on statistical analysis of large datasets to improve accuracy in tasks such as sentiment analysis and machine translation. Simultaneously research evolves on how to represent language using embeddings (dense vectors of numbers) for efficient processing.
- **2010s-present:** Advances in deep learning techniques through pretrained word embeddings, neural networks, self-attention and the transformer model architecture, together with the availability of big data, lead to significant improvements in NLP applications. Unsupervised learning on vast amounts of data enables researchers to train models of ever increasing size and capability.

### The Transformer Architecture

A lot happened in those last 20 years. Modern LLMs like GPT-3 or BLOOM are decoder-only transformer models. So let’s take a closer look at the latest significant architectural model advancement to see what that means: The Transformer.

The Transformer model architecture was introduced by Google researchers in 2017 [Source] and was a significant breakthrough in the field of NLP. The transformer model is based on a mechanism called self-attention.

Self-attention allows the model to focus on the most relevant parts of the input when generating an output. The model calculates an attention score for each word in the input based on its relationship with other words in the input. Words that are more important for generating the output are given higher attention scores.

The original transformer architecture consists of two main components: the encoder and the decoder. Each layer in the encoder and decoder consists of multi-head self-attention and feed-forward neural network sub-layers. The encoder processes the input and generates a representation of the input in the form of embeddings. The decoder then uses these embeddings to generate the output.

![Sketch of the transformer architecture](https://machinelearningmastery.com/wp-content/uploads/2021/08/attention_research_1.png)

This was a significant advancement from previous SOTA RNNs with LSTM or GRU, as the Transformer model architecture is highly parallelizable and can process long sequences effectively without loss of information.

### Generative Pretrained Transformers

> *Definitely check out [this great Medium Article](https://towardsdatascience.com/language-model-scaling-laws-and-gpt-3-5cdc034e67bb) written by Cameron R. Wolfe if you are interested in further detail and explanations. We will summarize the essentials from "language modeling at a glance" in this section.*

LMs are trained on large corpora of text using a causal language modeling objective that  
1. samples some text from the training corpus and  
2. tries to predict the next word that occurs.

This is a form of self-supervised learning, as the ground truth next word is right there as next word in the corpus.

> ![The language model pre-training process](https://miro.medium.com/v2/resize:fit:1100/format:webp/0*nu3wAvis1lFNbkvQ.png)
> *The language model pre-training process (created by Cameron R. Wolfe, [Source](https://towardsdatascience.com/language-model-scaling-laws-and-gpt-3-5cdc034e67bb))*

Modern LMs are decoder-only transformer models. Using only the decoder prevents them from looking ahead at the next token during training, as the decoder uses masked-attention compared to the bidirectional-attention in the encoder.

Beyond the decoder-only layers, the LM architecture contains embedding layers that store vectors corresponding to all the tokens within a fixed-size vocabulary. Tokens can be words or parts of a word, sometimes also single characters or punctuation. The embedding layers are essential for converting raw text into a model-ingestible input matrix.

#### Steps from raw Input Text to Matrix input for decoder

1. Tokenization: Text is split into tokens.
2. Vector Lookup: Tokens are mapped to corresponding vector representations.
3. Vector concatenation: Token vectors are concatenated to form a matrix representation of the input text.
4. Additional embeddings: E.g. add positional embedding to each token.

> ![unnamed](https://user-images.githubusercontent.com/36483428/227184789-72aeb637-aa6a-4160-83c5-b42cf4bb906f.png)
> *Converting text into token embedding matrix model input (adapted from Cameron R. Wolfe, [Source](https://towardsdatascience.com/language-model-scaling-laws-and-gpt-3-5cdc034e67bb))*

This Transformers architecture provides a very efficient utilization of compute. It enables language model pre-training at massive scale, leading to large language models with billions of parameters that can accurately predict the next token given a sequence of tokens as context. But how can one use these pretrained models for a specific task such as our use case of news snippet generation?

### Paradigm shift: From Finetuning to Prompting

Before the advent of LLMs, deep learning models were designed to perform a single, specific task and were comparatively compact in size. The standard approach for adapting these models to new tasks was to fine-tune them. For LLMs however, the quality of the learned representations improves with the size of the pre-trained LM. In many cases, LLMs can be utilized to tackle multiple tasks by leveraging the model's general text-to-text input-output structure, by providing task-specific textual "prompts" to the model.

#### Different paradigms and methods for model adaptation

| Paradigm | Finetuning Paradigm | Prompting Paradigm |
|---|---|---|
| *Definition* | *Adapting the model to a specific downstream task by iteratively learning the task from examples in a task-specific dataset through gradient updates.* | *Adapting the model to a specific downstream task by providing it with a textual description of the task and maybe also one or few examples.* |
| Specific Methods| **Multitask Finetuning:** A method to finetune a model on a dataset with mixed tasks to learn multiple tasks at once. | **Zero-Shot Prompting:** Prompting the model with some textual task “prompt” additional to the input. E.g. “Summarize the following news article: <article> =>” |
| | **Adapter based Finetuning:** A method that introduces adapters to reduce the number of parameters to adapt during the fine-tuning process. | **One-Shot/Few-Shot Prompting:** Providing the model with one or a few examples in addition to the input. |
| | **Instruction tuning:** A method finetuning a model on a task dataset with task instructions and expected output to make the model better at following human-like instruction. | **Prompt Learning/Prompt Tuning:** Automatically learning a prompt as soft token embeddings for a specific task. |

LLMs like GPT-3 and BLOOM have billions of parameters (GPT-3 175B, BLOOM 176B) rendering the finetuning approach ineffective at this scale. Instead, those models are pre-trained with a Causal Language Modeling objective, followed by instruction tuning to improve prompting capabilities. 

With prompt engineering, researchers can tailor input prompts to LLMs for specific outputs, resulting in significant breakthroughs in various NLP tasks. This approach is more resource-efficient, as a single LLM can be used for multiple tasks by simply engineering or learning a new prompt, with no need for additional training.

TL;DR: The field of NLP has developed rapidly over the last 20 years. The transformer architecture paved the way for Large Language Models. LLMs learn to model language causally from huge amounts of textual data and are quite resource intensive. But they come with a great new capability: prompting. With LLMs, the task adaptation paradigm shifts from fine-tuning to prompt-tuning, as the latter is more flexible and resource-efficient.

*“The power of Large Language Models: History and Concepts” is part six of our series News Snippet Generation. A Learning Journey on Open Source Large Language Models and how to assist journalists with generative AI in German.*
  
---
  
### Appendix: List of links to get started

**Transformers:** Easily download and train state-of-the-art pretrained language models  
https://huggingface.co/docs/transformers/index 

**How to generate**: Using different decoding methods for language generation with transformers  
https://huggingface.co/blog/how-to-generate

**Low Resource Training:** Resources needed to train language models and how to reduce them  
https://huggingface.co/blog/hf-bitsandbytes-integration

**Adapter based Finetuning:** Adding Adapters to PyTorch language models with adapter-transformers  
https://github.com/adapter-hub/adapter-transformers

**Parameter-Efficient Fine-Tuning (PEFT):** A framework for LoRA, Prefix Tuning, P-Tuning and Prompt Tuning  
https://github.com/huggingface/peft

**Prompt tuning:** OpenPrompt - Open Source Framework for Prompt Learning  
https://github.com/thunlp/OpenPrompt 
