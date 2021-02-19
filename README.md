# SQuAD_v2_sv

## What is SQuAD?
**S**tanford **Qu**estion **A**nswering **D**ataset (SQuAD) is a reading comprehension dataset, consisting of questions posed by crowdworkers on a set of Wikipedia articles, where the answer to every question is a segment of text, or span, from the corresponding reading passage, or the question might be unanswerable.

---

[SQuAD2.0](https://rajpurkar.github.io/SQuAD-explorer/) [Pranav Rajpurkar, Robin Jia & Percy Liang, 2018] combines the 100,000 questions in SQuAD1.1 with over 50,000 unanswerable questions written adversarially by crowdworkers to look similar to answerable ones. To do well on SQuAD2.0, systems must not only answer questions when possible, but also determine when no answer is supported by the paragraph and abstain from answering.

## What is SQuAD_v2_sv?
**SQuAD_v2_sv** is a Swedish version of SQuAD2.0. Translation was done automatically by using Google Translate API but it is not so straightforward because;
1. the span which determines the start and the end of the answer in the context may vary after translation,
2. tne translated context may not contain the translated answer if we translate both independently.

More details on how to handle these will be provided in another blog post.

## Contents

- *squad_train_v2_sv.json* contains training dataset translated from the original SQuAD v2 train dataset.
- *squad_dev_v2_sv.json* contains dev dataset translated from the original SQuAD v2 dev dataset.

## HuggingFace Datasets
You can also access SQuAD_v2_sv from the HuggingFace platform. 

## The fine-tuned model on this dataset
We fine-tuned [Swedish BERT](https://github.com/Kungbib/swedish-bert-models) pre-trained by the National Library of Sweden. The model is availabel in [HuggingFace model hub]().

## How to cite this work in papers
We didn't publish any paper about this work.
Please cite this repository in publications as the following:

```
@misc{squad_v2_sv,
  author = {Susumu Okazawa},
  title = {Swedish translation of SQuAD2.0},
  year = {2021},
  publisher = {GitHub},
  journal = {GitHub repository},
  howpublished = {\url{https://github.com/susumu2357/SQuAD_v2_sv}},
}
```