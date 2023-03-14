---
layout: post
title: "Snippets: A First Touchpoint for News and how to generate them with AI"
author:
- Hannah Greven
- Benjamin Rech 
image_sliders:
- snippet_slider
---

<!-- CSS -->
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.0.0/dist/css/bootstrap.min.css" integrity="sha384-Gn5384xqQ1aoWXA+058RXPxPg6fy4IWvTNh0E263XmFcJlSAwiGgFAW/dAiS6JXm" crossorigin="anonymous"> 

## Snippets: A First Touchpoint for News and how to generate them with AI

![snipaid_news_snippet_generation_snippet_types](https://user-images.githubusercontent.com/36483428/224952599-a3d2f2d2-25a0-4581-99ae-1ea42e48ff29.jpg)

Everyone has come across Snippets online before.

When reading news online, readers are usually drawn to an article through a Snippet. We will refer to short pieces of text that, in combination with an image or a link, serve to draw attention to the full article as snippets. A snippet can be many things, a title and teaser as well as a summary and keywords. But they all have one thing in common. Snippets are the first touchpoint for every article with its audience.

Snippets help readers contextualize the content of the article and build an expectation about the information they will receive. Snippets that raise expectations an article cannot fulfill lead to disappointment - The Clickbait Effect.

### Each channel has its own requirements

To attract attention, news articles are distributed across several different online channels. Articles appear with their Snippets on websites, as Link Previews in social media feeds and messenger apps, in newsletters and on the result pages of search engines. 

Writing Snippets is a challenge for journalists and newsroom editors, as they need to create tailored Snippets for each distribution channel to capture readers' attention and convey the essence of their articles with differing requirements in length, tone and audience.

{% include slider.html selector="snippet_slider" %}

### AI to the rescue?

AI can play a role in assisting journalists with the task of Snippet Generation. By leveraging large language models, AI can analyze the content of an article and generate Snippets that are optimized for each distribution channel's specific requirements.

By accelerating parts of the editorial process, newsrooms and journalists can stay competitive in today's digital landscape and optimize the process of providing valuable content to their audience. 

There are potential concerns using AI for content automation. Language Models can produce misinformation and may reinforce existing biases. This could be detrimental to the brand credibility, why we recommend using AI technology only as writing assistance and keeping a human in the loop.

### The Problem

Modern AI language models are capable of assisting the creation of different News Article Snippets. The problem: They are not accessible for online newsrooms and editorial staff yet. That's what we want to change with SnipAId.

### The SnipAId Use Case

SnipAId is intended to be an assistant for the task of generating snippets from a news text for various distribution channels. It uses techniques from the subfield of AI that deals with natural language processing (NLP). The project aims to make open source large language models accessible for the snippet generation task in everyday editorial business.

Its application is located at the verge between production and distribution phase. Features will include:

- **Keyword extraction** for tagging content and generating better SEO headlines  
- **Natural Language Generation (NLG)** eg. for title and teaser generation  
- **Summarization** for newsletter summaries and Social Media postings  

*“Snippets everywhere” is the third part of our series News Snippet Generation. A Learning Journey on Open Source Large Language Models and how to assist journalists with generative AI in German.*

TL;DR: Snippets - short pieces of text in addition to the full text of a news article - have become indispensable in online journalism for distribution. They are the entry point to every news story. They are distributed with a link to the news story via different channels. Each channel has different requirements regarding snippet length, tone and audience. Optimizing accordingly is tedious work for journalists. AI can assist in this process. With our project SnipAId we intend to build a human in the loop system for german news snippet generation.
