---
layout: post
title: "The Model Safari: Embarking on a Journey of Explorative Model Comparison"
author:
- Hannah Greven
- Benjamin Rech 
---

## The Model Safari: Embarking on a Journey of Explorative Model Comparison

![A robot in a blooming meadow of flowers](https://user-images.githubusercontent.com/36483428/228897263-eee0022b-0032-4566-bda0-ad9a0b74c82e.jpg)

To find models that are capable of generating German-language snippets from news articles, we took an exploratory approach. We started by testing different prompts on the paid API from OpenAI (Model: text-davinci-003), researched pre-trained LLMs and instruction-tuned LLMs and evaluated some models of the GPT-, T5- and BLOOM-Family.

### Promising GPT-3 Prompts
Example: GPT-3 suggests the following headlines for this [News Article on Google's new management from 2015](https://www.derstandard.at/story/2000024454774/googles-neues-management-die-arschloecher-sind-weg)

![Testing GPT-3 Prompts](https://user-images.githubusercontent.com/36483428/228898077-a1dad7b7-08f5-4b52-bf24-1893e297cb7e.png)

We made our [Notebook](https://github.com/snipaid-nlg/models/blob/main/news-headline-generation-with-gpt-3.ipynb) available to test News Headline Generation with GPT-3 on 10kGNAD with text-davinci-003. As input we supply the model with the article full text followed by a prompt like this: "{article text} {prompt}" or "{article} \n {prompt}".

For further snippets the following prompts perform well with GPT-3:
- Schreibe einen kurzen Teaser für diesen Artikel, der neugierig macht:
- Schreibe eine kurze Zusammenfassung des Artikels in drei Stichpunkte:
- Zusammenfassung, so kurz wie möglich, W-Fragen: 
- Drei verschiedene Snippets zu diesem Artikel, max 280 Zeichen:
- Schreibe 3 verschiedene SERP-Snippets:
- Schreibe ein SERP-Snippet für diesen Artikel mit dem Fokus Keyword "keyword":
- Schreibe verschiedene Überschriften zu dem Artikel (faktisch, clickbaity, emotional, humorvoll, Fachjargon): 
- Schreibe einen Tweet zu diesem Artikel:
- Schreibe einen LinkedIn-Post zu diesem Artikel:

GPT-3 is a corporate model only accessible by API. In the spirit of the open source community, we ventured out to find an open source competitor for the task of German News Snippet Generation.

### Overview of currently accessible pre-trained open source LLMs

There are several open source pre-trained LLM with different parameter sizes, licenses and language capabilities released in 2022. BLOOM and LLaMA have been trained on multilingual text corpora, other models are primarily trained on English text corpora.

| Name | Size | License | Links | Training Corpus | Language | Release |
|---|---|---|---|---|---|---|
| GPT2 | 124M - 1.5B | MIT | [Github](https://github.com/tatsu-la), [Huggingface](https://huggingface.co/gpt2-xl), [Paper](https://d4mucfpksywv.cloudfront.net/better-language-models/language_models_are_unsupervised_multitask_learners.pdf) | WebText | en | 05/2019 |
| GPT-J | 6B | MIT, Weights: Apache 2.0 | [GitHub](https://github.com/kingoflolz/mesh-transformer-jax/), [Huggingface](https://huggingface.co/EleutherAI/gpt-j-6B) | The Pile | en | 09/2022 |
| OPT | 125M - 66B | OPT-LICENSE | [Github](https://github.com/facebookresearch/metaseq), [Huggingface](https://huggingface.co/facebook/opt-66b), [Paper](https://doi.org/10.48550/arXiv.2205.01068) | BookCorpus, English Wikipedia, Commoncrawl (CC-News, Stories), OpenWebText, The Pile, PushShift.io Reddit  | primarily en | 05/2022 |
| RMKV-4 | 100M - 14B | Apache 2.0 | [Github](https://github.com/BlinkDL/RWKV-LM), [Huggingface](https://huggingface.co/BlinkDL/rwkv-4-pile-14b) | The Pile | en | 09/2022 |
| GPT-NeoX | 20B | Apache 2.0 | [Github](https://github.com/EleutherAI/gpt-neox), [Huggingface](https://huggingface.co/EleutherAI/gpt-neox-20b), [Paper](https://arxiv.org/abs/2204.06745) | The Pile | en | 04/2022 |
| BLOOM | 560M - 176B | RAIL | [Huggingface](https://huggingface.co/bigscience/bloom), [Paper](https://arxiv.org/abs/2211.05100) | ROOTS | multilingual | 11/2022 |
| LLaMA | 7B - 65B | GPL-3.0 | [Github](https://github.com/facebookresearch/llama), [Paper](https://arxiv.org/abs/2302.13971v1), [Paper](https://arxiv.org/abs/2211.05100) | CommonCrawl, C4, Github, Wikipedia, Books, ArXiv, StackExchange | multilingual | 09/2022 |
| UL2 | 20B | Apache 2.0 | [Huggingface](https://huggingface.co/google/ul2), [Paper](https://arxiv.org/abs/2205.05131) | C4 | en | 05/2022 |
| GLM | 130B | Apache 2.0 | [Github](https://github.com/THUDM/GLM-130B), [Paper](http://arxiv.org/abs/2210.02414) | The Pile, Wudao Corpora, various Chinese corpora and webcrawling data | en, ch | 10/2022 |

During our research we came across possibilities to adapt existing LLM to other languages, notably: [CLP-Transfer](https://arxiv.org/abs/2301.09626) and [WECHSEL](https://aclanthology.org/2022.naacl-main.293/). Malte Ostendorff trained two monolingual German language models using the CLP-Transfer method based on BLOOM-7b1: [bloom-6b4-clp-german](https://huggingface.co/malteos/bloom-6b4-clp-german) and [bloom-1b5-clp-german](https://huggingface.co/malteos/bloom-1b5-clp-german)

For the 1.5B sized GPT2 model, there are several monolingual adaptations: [german-gpt2](https://huggingface.co/dbmdz/german-gpt2), [german-gpt2-larger](https://huggingface.co/stefan-it/german-gpt2-larger), [gpt2-wechsel-german](https://huggingface.co/benjamin/gpt2-wechsel-german), [gpt2-wechsel-german-ds-meg](https://huggingface.co/malteos/gpt2-wechsel-german-ds-meg), [gerpt2-large](https://huggingface.co/benjamin/gerpt2-large)

### Overview of currently accessible instruction-tuned open source LLM

Since the instruction-tuning paradigm gained popularity within the development of language models, several new models of this type have been released open source. Particularly noteworthy are the multilingual variants (BLOOMZ, MT-0, MT-5), as they are applicable to our task.

| Name | Size | License | Links | Language | Release |
|---|---|---|---|---|---|
| BLOOMZ | 560M - 176B | RAIL | [Github](https://github.com/bigscience-workshop/xmtf), [Huggingface](https://huggingface.co/bigscience/bloomz), [Paper](https://doi.org/10.48550/arXiv.2211.01786) | multilingual | 11/2022 |
| FLAN-T5 | 80M - 11B | Apache 2.0 | [Github](https://github.com/google-research/t5x), [Huggingface](https://huggingface.co/google/flan-t5-xxl), [Paper](https://doi.org/10.48550/arXiv.2210.11416) | en | 10/2022 |
| Galatica | 125M - 120B | Apache 2.0 | [Github](https://github.com/paperswithcode/galai), [Huggingface](https://huggingface.co/facebook/galactica-120b), [Paper](https://arxiv.org/abs/2211.09085) | en | 11/2022 |
| MT-0 | 300M -13B | Apache 2.0 | [Github](https://github.com/bigscience-workshop/xmtf), [Huggingface](https://huggingface.co/bigscience/mt0-xxl), [Paper](https://doi.org/10.48550/arXiv.2211.01786) | multilingual | 10/2022 |
| MT-5 | 300M - 13B | Apache 2.0 | [Github](https://github.com/google-research/multilingual-t5), [Huggingface](https://huggingface.co/google/mt5-xxl), [Paper](https://doi.org/10.18653/v1/2021.naacl-main.41) | multilingual | 11/2020 |
| OpenChatKit | 20B | Apache 2.0 | [Github](https://github.com/togethercomputer/OpenChatKit) | ? | 03/2023 |
| Flan-UL2 | 20B | ? | [Github](https://github.com/google-research/google-research/tree/master/ul2) | ? | 03/2023 |
| Stanford Alpaca | 7B | Apache 2.0 | [Github](https://github.com/tatsu-lab/stanford_alpaca) | en | 03/2023 |

During our search we learned that new languages can be learned through multitask prompted finetuning. The following papers should be mentioned in this regard:
- [Crosslingual Generalization through Multitask Finetuning](https://arxiv.org/pdf/2211.01786.pdf)
- [BLOOM+1: Adding Language Support to BLOOM for Zero-Shot Prompting](https://arxiv.org/pdf/2212.09535v1.pdf)

### Testing Model Capabilities on the Task of "News Headline Generation" in German

We aimed to evaluate the performance of various language models on the task of "News Headline Generation" in the German language. To achieve this, we conducted exploratory tests on the 10kGNAD German news dataset.

Among the GPT family, we tested two models: vanilla GPT-J-6B ([Notebook](https://colab.research.google.com/drive/1NKh7cbXtNhuOVVSW8Jahigeb8Evvr7nu)) and mGPT ([Notebook](https://github.com/snipaid-nlg/models/blob/main/news-headline-generation/news-headline-generation-with-mGPT.ipynb)). Despite iterative tuning of generation parameters, mGPT regularly produced repetitions and language gibberish such as special characters and URLs, and also had stopping problems. On the other hand, GPT-J-6B, despite being trained on a small amount of German language data, generated surprising and promising results. However, the model's inference time was relatively long due to its size.

We also reviewed several models from the T5 family, such as MT5-small ([Notebook](https://github.com/snipaid-nlg/models/blob/main/news-headline-generation/news-headline-generation-with-mt5-small.ipynb)), MT0-base ([Notebook](https://github.com/snipaid-nlg/models/blob/main/news-headline-generation/news-headline-generation-with-mt0-base.ipynb)), flant5-base ([Notebook](https://github.com/snipaid-nlg/models/blob/main/news-headline-generation/news-headline-generation-with-flan-t5-base.ipynb)), and a fine-tuned model for the task called "german-news-title-gen-mt5" ([Notebook](https://github.com/snipaid-nlg/models/blob/main/news-headline-generation/news-headline-generation-with-german-news-title-gen-mt5.ipynb)). As described in the fifth post of this series, the fine-tuned model performed remarkably well on the task. The vanilla MT5 model could not generate coherent sentences out-of-the-box as it was only pretrained on the span-mask filling task and not on any down-stream tasks, as noted in this [GitHub Issue](https://github.com/huggingface/transformers/issues/8704#issuecomment-1224520475). MT-0 and FLAN-T5 had no German language capabilities.

Finally, we tested three models from the BLOOM model family: BLOOMZ (Notebook), bloom-1b5-clp-german ([Notebook](https://github.com/snipaid-nlg/models/blob/main/news-headline-generation/news-headline-generation-with-bloom-1b5-clp-german.ipynb)), and bloom-6b4-clp-german ([Notebook](https://github.com/snipaid-nlg/models/blob/main/news-headline-generation/news-headline-generation-with-bloom-6b4-clp-german.ipynb)). The smaller model had a good performance on the task of News Headline Generation, despite not being fine-tuned on any specific tasks. We did not observe any significant improvements in the larger model.

### A note on German data seen in pre-training

| Model | Training Corpus | German Language Share |
|---|---|---|
| GPT-J | [The Pile](https://huggingface.co/datasets/the_pile) | some de of unknown percentage,  unintended, 97.4% en ([Source](https://pile.eleuther.ai/paper.pdf)) |
| mT5 | [mC4](https://huggingface.co/datasets/mc4), [xP3](https://huggingface.co/datasets/bigscience/xP3) | ~ 5% de intentional ([Source](https://arxiv.org/pdf/2211.01786.pdf)) during pre-training, none intentionally during instruction fine-tuning |
| BLOOMZ | [ROOTS](https://huggingface.co/bigscience-data), [xP3](https://huggingface.co/datasets/bigscience/xP3) | 0.21% de unintended during pre-training ([Source](https://arxiv.org/abs/2211.01786)); none intentionally during instruction fine-tuning |
| LLaMA/Alpaca | Pretraining on [English CommonCrawl, C4, Github, Wikipedia, Gutenberg and Books3, ArXiv](https://github.com/facebookresearch/llama/blob/main/MODEL_CARD.md#training-dataset), Instruction Tuning on [Alpaca Dataset](https://huggingface.co/datasets/yahma/alpaca-cleaned) | some de of unknown percentage during Pretraining but mainly English, instruction tuning exclusively in English |

Through the lack of German language data seen during pre-training most Open Source Language Models are deficient in their ability to handle German language.

### Interim results, learnings and candidate selection

None of the tested models are able to solve the task of news snippet generation for German news without further adaptation. As a result, our focus has shifted towards selecting models that have the potential to be adapted to solve the task at hand. We selected four candidates for further consideration:
- GPT-J as a competitor to GPT-3 Curie
- Bloomz as a multilingual human instruction zero-shot solver
- mT5 as a strong multilingual summarizer
- Alpaca as newest competitor to ChatGPT

These models have the potential to be adapted to the task, but further work is needed to make them a viable solution. In the next few posts, we'll explore how to adapt these models.

TL;DR: GPT-3 is capable of generating a variety of news snippets in German language by simply prompting the model. On the search for an Open Source competitor, the following became clear. There are competitors both pre-trained and some even instruction-tuned. However, they are deficient in their ability to handle German language. To solve the task of news snippet generation for German news further adaptation will be necessary.

*"The model safari: An Explorative model comparison" is part eight of our series News Snippet Generation. A Learning Journey on Open Source Large Language Models and how to assist journalists in generating headlines and teasers in German with AI.*
