

## Retrieval-Augmented Machine Translation with Unstructured Knowledge
This repository contains the resources for our paper ["Retrieval-Augmented Machine Translation with Unstructured Knowledge"](https://arxiv.org/abs/2412.04342)


### 1. Overview

In this work, we bring the idea of RAG into machine translation (MT), and study how to enhanced MT models with retrieved unstructured knowledge (documents). To this end, we introduce:

- **One benchmark dataset**: We propose RAGtrans dataset that contains 79K MT samples, each of which consists a source English sentence, a (relevant or noisy) document (in English, Chinese, German, French or Czech) and the corresponding Chinese translation (obtained via GPT-4o or professional human translators).
- **One training framework**: We propose CSC multi-task training method with three designed objectives to enhance models' retrieval-augmented MT ability.


### 2. RAGtrans

We plan to release the data before Christmas.

<p>
    <br>
    <img src="./figs/UBR_20241124.jpg" height="600"/>
    <br>
</p>
Universal Studios, Beijing, Nov. 24, 2024