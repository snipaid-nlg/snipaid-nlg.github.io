---
layout: post
title: "Learning from comparable tasks: Insights from News Headline Generation and Text Summarization"
author:
- Hannah Greven
- Benjamin Rech 
---

## Learning from comparable tasks: Insights from News Headline Generation and Text Summarization

![IMAGE Robot on a treadmill](https://user-images.githubusercontent.com/36483428/226585357-12739d7c-8585-48fa-99e4-4f666468b14c.jpg)

We want to learn from comparable tasks in the field of NLP and explore different ways to solve the task of News Snippet Generation. In this Post, we will learn from two NLP tasks comparable to the task of Snippet Generation: News Headline Generation and Text Summarization.

### The Task of News Headline Generation

Headline generation is a task of generating an appropriate headline for a given news article.

In terms of semantics, the headline of a news report is determined by the top levels of the report's thematic macrostructure. It conveys the most significant or relevant information, expressing the intended highest macro-proposition. As a result, the headline plays a crucial cognitive role in guiding the reading and comprehension processes by directing the reader's attention towards the most important information (see [Van Dijk's "News Schemata"](https://discourses.org/wp-content/uploads/2022/07/Teun-A.-van-Dijk-1985-Structures-of-news-in-the-press.pdf)).

This use case of an assistive technology was worked on early (i.e. [Dorr et al. 2003](https://doi.org/10.3115/1119467.1119468)), moving from an extractive to an abstractive paradigm (see [PapersWithCode](https://paperswithcode.com/task/headline-generation/latest)).

- **Extractive:** Taking the most important phrases or sentences from the text.
- **Abstractive:** Convey the same meaning with new phrases and terms that differ from the text (rephrase).

In 2019 Christian Schäfer and Dat Tran developed [Headliner](https://github.com/as-ideas/headliner). Headliner is a sequence modeling library that eases the training and in particular, the deployment of custom sequence-to-sequence (seq2seq) models: a basic encoder-decoder, an encoder-decoder with Luong attention, a transformer and a transformer on top of a pre-trained BERT-model.

In October 2022 the AIAUTOMATIONLAB from Bayerischer Rundfunk (BR; "Bavarian Broadcasting") - a German public-service radio and television broadcaster - released [german_news_title_gen](https://huggingface.co/aiautomationlab/german-news-title-gen-mt5). They finetuned a mt5 model on a corpus of 90k news articles from BR24 published between 2015 and 2021.

We tested german-news-title-gen-mt5 on the 10kGNAD dataset and found promising results in terms of model size and inference speed. Although we observed some issues with fluency in German ([Notebook](https://github.com/snipaid-nlg/models/blob/main/news-headline-generation/news-headline-generation-with-german-news-title-gen-mt5.ipynb)).

#### Original Headline and Teaser

> **Boris Johnson im Brexit: Der wütende Hulk**  
> Der britische Premier Boris Johnson hält daran fest, sein Land am 31. Oktober aus der EU führen zu wollen. Auf Kritik reagiert er mit drastischen Vergleichen.

**Generated Headlines**  
> Britischer Premier Johnson <ins>unterstricht</ins> Brexit-Entschlossenheit  
> Boris Johnson: Der Ton wird <ins>rauher</ins> in Großbritannien  
> Britischer Premierminister Boris Johnson <ins>unterstricht</ins> Brexit-Entschlossenheit

*Example of generated headings by german-news-title-gen-mt5 on 10kGNAD (ID 6334)*

### The Task of Text Summarization

Summaries are the verbalization of the underlying semantic macrostructure of a text: They follow the structure of the text and give a quick overview of the most important points within each section. In this way, they provide an overview of the content, but also information about the structure of the text. This creates an expectation among readers, which makes it easier to understand when reading the fulltext. 

Summarizing text is a viable task for many text domains and is therefore well researched for the last few years and also has a strong open source community for english language

Recently, [Aumiller et al](https://arxiv.org/abs/2301.07095) tested the state of the art open source abstractive summarization systems available for German language based on Transformer models from the Model families of BERT, MT5 and T5.

![Evaluation scores of current abstractive summarization models with german language capabilities](https://user-images.githubusercontent.com/36483428/226586921-ec355888-6d54-471b-97fe-69c6ef11b7e5.png)


We tested a [summarization model developed by Phillp May](https://huggingface.co/T-Systems-onsite/mt5-small-sum-de-en-v2) on the 10kGNAD dataset and observed that the model consistently produced short summaries with only minor language issues. The model was trained on Google's mt5-small, which has 300 million parameters and is surprisingly fast. View the notebook: [Text Summarization with mt5-small-sum-de-en-v2 on gnad10](https://github.com/snipaid-nlg/models/blob/main/text-summarization/news-summarization-with-mt5-small-sum-de-en-v2-on-gnad10.ipynb)

#### Generated Examples
> Der 21-Jährige erlitt beim 0:4 gegen Mödling einen Teilriss des Innenbandes im linken Knie. Der 21-Jährige <ins>erlitt</ins> eine Schiene, muss aber nicht operiert werden.  

> The Forbidden Room ist ein Two-Reeler von 1913/14, der <ins>der</ins> arbeitswütige US-Regisseur Allan Dwan gedreht hat. <ins>Der Film enthält die Fragmente eines Kinos, an das wir uns nicht mehr erinnern können.</ins>  

> Die Zeiten des harten Management-Stils sind bei Google vorbei. Das belegen zahlreiche ehemalige Mitarbeiter und Geschäftspartner.

### Conclusion

MT5 has no german language capabilities out of the box, but has been tuned to text summarization in english. Fine-tuning to German language for Text Summarization and News Headline Generation works well, but you would have to tune and host one model per task in production.

TL;DR: mt5 is a strong multilingual summarizer and can be fine-tuned to generate german news headlines and summaries efficiently. There are some open source text summary models that are able to summarize german news articles out of the box. Working with such task specific models would require to find or finetune and host a model for each snippet task.

*"Learning from comparable tasks" is part five of our series News Snippet Generation. A Learning Journey on Open Source Large Language Models and how to assist journalists with generative AI in German.*
