---
layout: post
title: "Requirements and Challenges: Understanding the Task of News Snippet Generation"
author:
- Hannah Greven
- Benjamin Rech 
---

## Requirements and Challenges: Understanding the Task of News Snippet Generation

![A news article with news snippets hovering over it and measuring tape around](https://user-images.githubusercontent.com/36483428/225734026-a95ee2c6-ad8b-4e25-8dd1-c3383d4ff260.png)

Snippet generation is a task in the field of Natural Language Processing (NLP), an interdisciplinary subfield of linguistics, computer science and artificial intelligence. 
The goal is to process a natural language document, in our case a news article, and to generate a shorter piece of text (news snippet).

![Schematic sketch of news snippet generation: A news article is input to the language model. The language model processes and outputs news snippets.](https://user-images.githubusercontent.com/36483428/225736488-58008cbd-8b7f-4fe4-b7b9-defa09553f42.png)

In this blog post we will explore different types of news snippets and their unique requirements as well as challenges in building a system that generates them automatically.

### What snippets are there

There are various text snippets for journalistic texts online. To name a few:
- **Title**, aka headline. A short phrase/few words that reflect the essence of the news.
- **Teaser**, aka hook or lede. A few sentences that spark curiosity about the news story.
- **Summary**, a brief outline with the main points of the news.
- **Keywords**, terms that either occur in the text itself or can be used to label the text.
- **Search engine result page (SERP)** snippet. A preview of a search result on a search engine's results page. Ideally follows the AIDA framework and contains/highlights keywords.
- **Newsletter** snippet. Abbreviate versions of news articles written in about one-third of space.
- **Social media** posts. One or two sentences within the maximum post length that provoke interest to read the news article.

*Note: There are basic snippets such as keywords, title and teaser and composite snippets such as SERP, newsletter and social media snippets. The latter combine several basic snippets such as keywords, title and a teaser or summary.*

### Challenge 1: Different snippets target different audiences and differ in length and style

| Snippet type | Title limits | Teaser/summary limits |
|---|---|---|
| Basic Title | mean 60, max. 120 chars | - |
| Basic Teaser | - | mean 150, max. 240 chars |
| Google SERP Snippet | 580 pixels (desktop) or 920 pixels (mobile), ~ 71 chars | 990 pixels (desktop) or 1,300 pixels (mobile), ~ 108 chars |
| Newsletter Snippet | same as basic title | 4-5 sentences, ~ 590 chars |
| Tweet | - | max. 280 chars including url |

### Challenge 2: Language matters

Our focus is on snippets for German news. Language models learn their language capabilities through the corpus on which they have been trained. Predominant language in NLP research is still English.

Those are options for generating German snippets with large language models: 
- Train a language model from scratch (costly due to the size of Large Language Models (LLMs) and would also require a large amount of task specific data in German)
- Learn German language with a pretrained model (costly due to the size of LLMs)
- Build on a language model with german language capabilities (fewer models to choose from)
- Translate (at the cost of translation errors and slight delay at inference)

### Challenge 3: Snippet =/= Summary

Summarization is a common task in NLP. Snippets express important facts or ideas about the news story in a clear and short form. Therefore, one might be tempted to think that snippet generation is a summarization task.

For example a teaser is indeed a brief summary or description of the article that should be engaging, informative, and attention-grabbing, while also providing a clear and accurate summary of the main theme or premise of the story. The ideal teaser should use language that is exciting, suspenseful, or intriguing, while also accurately representing the content of the article. Ultimately, a teaser should entice readers to read the full story.

A summary is not necessarily a teaser. If all the information can be found in the teaser, the full story is not interesting any more. There remain differences in length, structure, and language style, which we should keep in mind.

### Challenge 4: Evaluation

Last but not least, it is important to measure how good an automatic snippet generation system is e.g. in terms of fluency, conciseness, factuality and attractiveness. There are quantitative and qualitative methods to evaluate model performance on the task. 

Both types of methods come with their own set of challenges: Qualitative evaluation methods depend on human judgment, are resource intensive and not scalable. Quantitative methods, on the other hand, automatically compare the generated text to a ground truth, but cannot recognize synonyms or paraphrases.

#### Quantitative Metrics
For evaluation we need a testset of news articles and real-world snippets for those news articles. Quantitative accuracy metrics are a measure of co-occurring words or sequences of n words (so called n-grams) in the model generated snippets and the real-world reference from the testset (ground truth). The two most well known n-gram co-occurrence statistics are [ROUGE](https://aclanthology.org/W04-1013.pdf) and [BLEU](https://aclanthology.org/P02-1040.pdf). 
Generally speaking:

- ROUGE focuses on recall: how much the words or n-grams in the ground truth appear in the candidate model outputs.
- BLEU focuses on precision: how much the words or n-grams in the model outputs appear in the ground truth.

Such n-gram based methods give a sense of the overlap between ground truth and model output. However, they are incapable of measuring semantic or factual accuracy. To account for factual or semantic accuracy other quantitative metrics or qualitative expert evaluations are required.

##### Quantitative Factuality Metrics

Language models do not necessarily generate factual text (Hallucination). There are three quantitative factuality metrics used in research on summarization systems: 

- SummaC: a metric for document level inconsistency detection based on probabilities of one sentence entailing another, being neutral or contradicting it ([Paper](https://doi.org/10.1162/tacl_a_00453), [Code](https://github.com/tingofurro/summac))
- QAFactEval: a metric that uses question answering (QA) to identify factual inconsistencies ([Paper](https://doi.org/10.48550/arXiv.2112.08542), [Code](https://github.com/salesforce/QAFactEval))
- DAE: a metric for factual inconsistencies, that identifies factual inconsistencies in paraphrasing and summarization better than sentence-level methods or those based on question generation ([Paper](http://dx.doi.org/10.18653/v1/2020.findings-emnlp.322), [Code](https://github.com/tagoyal/factuality-datasets))

Because these metrics are only available for the English language, input and output texts may need to be translated to English.

#### Qualitative Expert Evaluation

Apart from blind similarity comparison between ground truth and generated texts, researchers in the field of text generation and abstractive summarization, suggest the evaluation of generated texts based on heuristics as fluency, relevance, conciseness and attraction.

[Liu and Lapata (2019)](https://aclanthology.org/D19-1387.pdf) present participants “with the output of two systems (and the original document) and ask them to decide which one is better according to the criteria of Informativeness, Fluency, and Succinctness.”

To evaluate a news headline generation system, [Jin et al. (2020)](https://arxiv.org/pdf/2004.01980.pdf) select 50 random news articles from the test set and have three native-speaker evaluators score the generated headlines based on three criteria on a scale of 1 to 10: relevance, attractiveness, and language fluency of the headlines. The scores are averaged to generate a final score after collecting the results of the human evaluation.

[Liu et al. (2022)](https://arxiv.org/pdf/2211.03305v1.pdf) randomly select 50 news articles from the test set and have three annotators rank five generated headlines and the reference headline fluency, relevance, and attractiveness, as previously done by Jin et al. (2020). Rankings are assigned with points from six to one from the top-ranked headline to the bottom-ranked headline.

TL;DR: News Snippet Generation is the NLP task of processing a news article and generating a shorter piece of text from it. The main challenges are differing snippet length and styles for different audiences and purposes, task comparability to common NLP tasks, language restrictions and finding suitable evaluation metrics.

*“Requirements and Challenges: Understanding the Task of News Snippet Generation” is part four of our series News Snippet Generation. A Learning Journey on Open Source Large Language Models and how to assist journalists with generative AI in German.*
