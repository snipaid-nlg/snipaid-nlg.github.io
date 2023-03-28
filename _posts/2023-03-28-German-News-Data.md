---
layout: post
title: "German News Data: A Comprehensive Guide to Public Datasets and Considerations for Private Dataset Creation"
author:
- Hannah Greven
- Benjamin Rech 
---

## German News Data: A Comprehensive Guide to Public Datasets and Considerations for Private Dataset Creation

![Huge pile of newspaper articles](https://user-images.githubusercontent.com/36483428/228284251-7a874c47-ba91-47c7-8461-e924e7f6bfd7.png)

News data is an important resource for language models that are used to process and generate text in the news domain. News articles provide a rich source of information from a wide range of topics and can provide insights into the language, topics and trends of a specific region. Beyond the content, news data also provides insights into language patterns and structures commonly found in news articles.

Training AI models on news data, e.g. parameter-efficiently fine-tuning a large language model, helps the model to better understand and generate text in the news domain. Getting good news data can be challenging though. There are only few German news datasets publicly available due to copyrights. In this blog post, we first evaluate public datasets and then explore options and considerations for creating a private dataset.

### Option 1: Use one of the Public German News Datasets

Excellent places to start looking for public news datasets are the [Huggingface Datasets Library](https://huggingface.co/datasets) and the [Kaggle Dataset Library](https://www.kaggle.com/datasets). Some news datasets can also be found on GitHub or other websites. We found and evaluated the following public German news datasets.

#### Ten Thousand German News Articles Dataset (10kGNAD)

Created by Timo Block in 2019, the [Ten Thousand German News Articles Dataset (10kGNAD)](https://tblock.github.io/10kGNAD/) consists of 10.273 german language news articles from an Austrian online newspaper (DER STANDARD). 10kGNAD includes a column for the full text of the article and a column for the assigned news category ranging from 0-9 and is therefore well suited for News Topic / Category Classification.

Challenges and Limitations: This dataset does not provide columns for specific news snippets such as title and teaser. In some cases the first sentence of a text (up to the first period) is a headline, but not in all, which is why this dataset is not suitable for training i.e. a news headline generation system.

License and Access: 10kGNAD is licensed under creative commons license (CC BY-NC 4.0). You can download the dataset from [GitHub](https://tblock.github.io/10kGNAD/).

#### MLSUM

MLSUM is a multilingual dataset that was presented as one of the first efforts in making larger-scale training sets available for multiple languages that also include German
as a language. MLSUM is constructed by extracting news articles and associated summary sections as generation targets. We use the German subset in this work with 242.982 samples (train 220.887, test 11.394, validation 10.701). 

Challenges and Limitations: Despite its popularity, issues in the quality of samples have gone unnoticed until early 2022, when [Philip May was the first to report on problems](https://may.la/blog/2022/02/23/anomalies-in-the-mlsum-dataset/). In 126,270 (more than half) of them the summary is completely included in the text. This is very bad for training abstractive summarization models on this dataset. A model trained on this data learns to extract sentences instead of generating an abstractive summary. In [further dataset exploration](https://colab.research.google.com/github/snipaid-nlg/datasets/blob/main/evaluation/mlsum-dataset-exploration.ipynb), we found that all samples derived from Sueddeutsche Zeitung between January and May 2019. Evaluating the distribution of topics, there is an signific overrepresentation in sports, business and politics. Also summary text lengths might be problematic, as the longest summary in train split is 1389 characters long.

License and Access: Usage of MLSUM is restricted to non-commercial research purposes only. If this suits your use case, you can download the dataset from [Huggingface](https://huggingface.co/datasets/mlsum) or from [GitHub](https://github.com/ThomasScialom/MLSUM).

#### German News Dataset

Steven Mi crawled 162.991 German news articles from 48 news publishers between 31.03.2020 and 18.11.2020. Each record contains title, text, authors, publishing date, tags and the publisher.

Challenges and Limitations: We found some [issues within the German News Dataset](https://colab.research.google.com/github/snipaid-nlg/datasets/blob/main/evaluation/german-news-dataset-by-steven-mi.ipynb) and recommend some preprocessing before using the dataset. There are 7.681 records without a fulltext. 11.477 title duplicates and 11.058 text duplicates. Also we found some leftover html artifacts and unrelated text strings in the title-column. For example there are 3.762 records with an unencoded quotation mark (&#034;), 5.014 records with the string *** BILDplus Inhalt *** attached to the title, 267 records containing angle brackets (<) indicating HTML-Code and 799 records with an indicator of the news article source (heise+).

License and Access: Usage of the “German News Dataset” is limited to non-commercial use. You can download the data from [Kaggle](https://www.kaggle.com/datasets/pqbsbk/german-news-dataset).

#### German News Articles (webz)

The Web-Crawling Service Webz (formerly Webhose) made a dataset of 398.840 german news articles from 2015 publicly available.

Challenges and Limitations: After combining the single JSON files to one dataset, we found duplicates and uncleaned title and text columns (breadcrumbs and titles in text, crawling errors). Since the data would require a lot of cleaning, we do not recommend using it.

License and Access: Nevertheless, usage is not limited by any license and you can download the dataset directly from [webz](https://webz.io/free-datasets/german-news-articles/).

#### German news headlines

The dataset consists of 8.377 records of german politics and economics news headlines from 7 different sources. For each record a headline and a sub-headline are provided.

License and Access: Usage of the dataset is limited to non-commercial use (CC BY-NC 4.0). You can download the dataset from [Kaggle](https://www.kaggle.com/datasets/matthiasse/german-news-headlines-politics-and-economics).

#### Other datasets in German language 
Apart from these datasets dedicated to news articles, there are several german-language datasets that are used for training language models. Among the mentioned datasets there are also multilingual datasets that support German as one of the languages. 

- [GC4 Corpus](https://german-nlp-group.github.io/projects/gc4-corpus.html): German colossal, cleaned Common Crawl corpus
- [OSCAR](https://oscar-project.org/) is an Open Source project aiming to provide web-based multilingual resources and datasets on a large scale
- [GeWiki](https://github.com/domfr/GeWiki), [WikiNewsSum](https://github.com/airKlizz/WikinewsSum), [Wiki Lingua](https://huggingface.co/datasets/GEM/wiki_lingua) for summarization based on Wikipedia
- [Klexikon](https://huggingface.co/datasets/dennlinger/klexikon) for text simplification
- [Tatoeba](https://huggingface.co/datasets/Helsinki-NLP/tatoeba_mt) for de-de Paraphrasing
- [GermanQuAD](https://huggingface.co/datasets/deepset/germanquad) for factual Q&A
- Multiple Translation Datasets, e.g. [tatoeba-mt](https://huggingface.co/datasets/Helsinki-NLP/tatoeba_mt), [wmt19](https://huggingface.co/datasets/wmt19), [OPUS-100](https://huggingface.co/datasets/opus100)

### Option 2: Establish your own Private Dataset

#### Crawling news data from online sources
One alternative approach is to use web crawling to collect news articles directly from news websites. Crawling does come with its own set of challenges. For one, it can be time-consuming and technically complex and tedious to set up a web crawling system. There are also legal and ethical considerations to be aware of, particularly around data privacy, intellectual property and copyright.

Before starting to build your own news dataset, some preliminary considerations should be made:
- Choose a set of news publishers: One way to determine a set of X publishers may be via the number of monthly visits. The “Informationsgemeinschaft zur Feststellung der Verbreitung von Werbeträgern” is issuing visit metrics for most german news publishers. A list of German newspapers can be found on Wikipedia. The more different news publishers are to be crawled, the more specific rules for extraction have to be coded.
- Think about the dataset size: The size of the data set depends on the intended method for training or tuning a model. Roughly estimated, it will take you around 10.000 to 100.000 records. Depending on the size, you may need to use a database to efficiently store and access the data. It’s recommended to save the page html for reproducibility.
- Sample your data by publication date: Ideally, the dataset should cover a long period of time to achieve the most diverse distribution of topics. The most known sampling methods are random sampling and quota sampling, i.e. so-called "artificial weeks", formed by selecting a different day of the week from each week contained in the collected set of news.
- Balance your data by news category distribution: Most news articles occur in the categories of politics, business and sports. To achieve a wide variety of topics, the dataset should be balanced by news category.
- Define needed values for extraction: Extracting and parsing structured data from mostly unstructured HTML is the most time-consuming task while building your own news dataset. The more different values are to be extracted from HTML, the more specific rules have to be coded. 
- Clean your data: Remove unrelated text from the article text, strip news publishers name from titles, check for teasers contained in the article text and remove them, check for missing values and duplicates.
- Verify the accuracy and consistency of the data. Manually check a sample of the articles to ensure that the extracted data is accurate and consistent across different sources.
- Choose a web crawling tool that is efficient and can handle large volumes of data. Some popular web crawling tools are [Scrapy](https://scrapy.org/), [Beautiful Soup](https://beautiful-soup-4.readthedocs.io/en/latest/), and [Selenium](https://www.selenium.dev/). There are some specific libraries available to assist you with crawling and extraction: [Newspaper](https://github.com/codelucas/newspaper), [News-please](https://github.com/fhamborg/news-please) and [Newspipe](https://github.com/steven-mi/newspipe).
- Think about hard and metered paywalls and cookie notices!

#### Extracting a Language and Domain specific Subset from a bigger Text Corpus

Commoncrawl provides an extensive, free-to-use archive of news articles from small and major publishers world wide. To extract german news articles from commoncrawl.org's news archive, you can follow the [instructions from the news-please](https://github.com/fhamborg/news-please#news-archive-from-commoncrawlorg) library. 

Use a News API to establish a Dataset

Apart from crawling your own data and using public German News Datasets, there are paid APIs like [newsAPI](https://newsapi.org/) and [NewsCatcher](https://newscatcherapi.com/) you can use to collect news data.

TL;DR: News data can improve language model capabilities for usage in the news domain. There are few publicly available datasets of non-English news articles and the available German news datasets need to be pre-processed before use. However, there is no dataset that covers news and several types of news snippets. Consequently, it is desirable to create a custom dataset. Options for creating such a custom dataset include crawling online sources or extracting and combining news and snippets from other datasets or (paid) APIs.

*“German News Data: A Comprehensive Guide to Public Datasets and Considerations for Private Dataset Creation” is part seven of our series News Snippet Generation. A Learning Journey on Open Source Large Language Models and how to assist journalists in generating headlines and teasers in German with AI.*
