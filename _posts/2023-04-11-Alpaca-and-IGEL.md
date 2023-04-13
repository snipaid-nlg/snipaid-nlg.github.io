---
layout: post
title: "Alpaca and IGEL: Supporting German Language through Multitask Prompted Fine Tuning"
author:
- Hannah Greven
- Benjamin Rech 
---

## Alpaca and IGEL: Supporting German Language through Multitask Prompted Fine Tuning

![Hedgehog](https://user-images.githubusercontent.com/36483428/231294738-fe69eca0-f23a-4abc-8d4d-eb38781d2474.jpg)

During our research on adapting BLOOM to another language we came across the possibility to [add language support for another language to BLOOMZ](https://arxiv.org/abs/2212.09535) by means of instruction finetuning. This approach requires adding instructions in the new language to the crosslingual task mixture of xP3 and finetune BLOOM with this data. 

Around the same time, researchers at Stanford trained a small language model with great capabilities, learning from a [52k English instruction dataset](https://huggingface.co/datasets/yahma/alpaca-cleaned) generated with GPT-3. Derived from Meta Model LLaMA, Alpaca is able to compete with its teacher. As the instruct dataset is in English, this model has no German capabilities. Yet it is remarkable and has a lot to do with acquiring a German Instruct Model capable of News Snippet Generation.

### What makes Alpaca remarkable

1. The model is 25x smaller than GPT-3, yet comparably capable.
2. The Original Training Cost is as low as ~600$:
    - $500 for Self Instruct Data generation via GPT-3 API
    - $100 for LoRA Adapter Fine Tuning on 8 A100 GPUs
3. The release of the model spurred further development.

A month after its release:
- Parameter Efficient Fine Tuning is possible on a single RTX 4090 for ~3$ in just a few hours.
- Other Open Source Instruct Models have been trained on the Alpaca Instruct Dataset.
- The dataset has been translated to German and successfully yielded the first German Instruct models.

At the time of this Blog Post being released, these German Instruct Models are only a few weeks old. So here is a short introduction.

### German Alpaca
A Version of the Stanford Alpaca Model continuously fine-tuned on the 52k instruction dataset machine translated to German language.

#### German Alpaca Ressources:
- [Code at GitHub](https://github.com/thisserand/alpaca-lora-finetune-language)
- [Model Weights](https://huggingface.co/thisserand/alpaca_lora_german)
- [Jupyter Notebook](https://github.com/snipaid-nlg/models/blob/main/Alpaca_LoRa_DE.ipynb)

### IGEL (Instruction-tuned German large Language Model)
A LLM family developed for German language. The first version of IGEL is built on top of BLOOM, adapted to German by Malte Ostendorff. 

#### IGEL Ressourcen:
- [Model Weights](https://huggingface.co/philschmid/instruct-igel-001)
- [Playground](https://huggingface.co/spaces/philschmid/igel-playground)
- [Jupyter Notebook](https://github.com/snipaid-nlg/models/blob/main/IGEL_001_LoRA.ipynb)

Both Models have been trained in a modular and parameter efficient way instruction finetuning LoRA adapters for pretrained language models. The difference is the base model: For German Alpaca the base model is LLaMA with Stanford Alpaca LoRA Adapters. For IGEL the base model is BLOOM CLP German (6.4 B Parameters). While the BLOOM Base model comes with strong German capabilities, Stanford Alpaca is an English Base model.

### Snippet Generation capabilities

Compare German Alpaca and IGEL we find IGEL more capable. IGEL has stronger German skills. Comparing the base models this seems only reasonable, as IGEL is built on a German base model while German Alpaca builds on an English base model.

A short test of the out of the box snippet generation capabilities of both german instruct models concluded as follows.

| Snippet/Model | German Alpaca | IGEL |
|:---|---|---|
| Titel | ✅ | ✅ |
| Teaser | ✅ | ✅ |
| Summary | ❌ | ✅ |
| Keywords | ❌ | ✅ |
| SERP (Title-Tag, Meta-Description) | ❌ | ❌ |
| Tweet | ❌ | ❌ |

TL;DR: Stanford Alpaca accelerated development towards german instruction finetuned language models. With German Alpaca and IGEL, the first German instruct language models have arrived. Putting both to the test, we find IGEL more capable.

*“Alpaca and IGEL: Supporting German Language through Multitask Prompted Fine Tuning” is part eleven of our series News Snippet Generation. A Learning Journey on Open Source Large Language Models and how to assist journalists in generating headlines and teasers in German with AI.*

