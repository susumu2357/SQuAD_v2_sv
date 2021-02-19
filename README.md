# SQuAD_v2_sv

## What is SQuAD?
**S**tanford **Qu**estion **A**nswering **D**ataset (SQuAD) is a reading comprehension dataset, consisting of questions posed by crowdworkers on a set of Wikipedia articles, where the answer to every question is a segment of text, or span, from the corresponding reading passage, or the question might be unanswerable.

---

[SQuAD2.0](https://rajpurkar.github.io/SQuAD-explorer/) [Pranav Rajpurkar, Robin Jia & Percy Liang, 2018] combines the 100,000 questions in SQuAD1.1 with over 50,000 unanswerable questions written adversarially by crowdworkers to look similar to answerable ones. To do well on SQuAD2.0, systems must not only answer questions when possible, but also determine when no answer is supported by the paragraph and abstain from answering.

## What is SQuAD_v2_sv?
**SQuAD_v2_sv** is a Swedish version of SQuAD2.0. Translation was done automatically using the Google Translate API but it is not so straightforward for the following reasons.
1. The span that determines the start and end of the answer in the context may change after translation.
2. If the context and the answer are translated independently, the translated answer may not be included in the translated context.

To overcome these difficulties, we used following strategy.
1. Before the translation, insert the special marker around the answer in the context. For instance, if we have the context like this;
```
Diatomic oxygen gas constitutes 20.8% of the Earth's atmosphere. However, monitoring of atmospheric oxygen levels show a global downward trend, because of fossil-fuel burning.
```
And, if the question is *Which gas makes up 20.8% of the Earth's atmosphere?* and the answer is *Diatomic oxygen*,
we insert the special marker *[0]* around the answer, that yields
```
[0] Diatomic oxygen [0] gas constitutes 20.8% of the Earth's atmosphere. However, monitoring of atmospheric oxygen levels show a global downward trend, because of fossil-fuel burning.
```
Note that, the special marker is not limited to *[0]*, but we can use others.

2. Translate the marked context. The result will be like this;
```
[0] Diatomiskt syre [0] gas utgör 20,8% av jordens atmosfär. Övervakning av syrehalten i atmosfären visar dock en global nedåtgående trend på grund av förbränning av fossila bränslen.
```

3. Extract the marked sentence from the translated context. It will be the translated answer. The start and end of the marked sentence will be the span of the answer.

This strategy is not perfect. Some context-answer pairs were not translated properly. Therefore, the size of the dataset is about 90% of the original.

## Contents

- *squad_train_v2_sv.json* contains training dataset translated from the original SQuAD v2 train dataset.
- *squad_dev_v2_sv.json* contains dev dataset translated from the original SQuAD v2 dev dataset.

## HuggingFace Datasets
You can access SQuAD_v2_sv using the HuggingFace Datasets.

```python
from datasets import load_dataset

dataset = load_dataset('susumu2357/squad_v2_sv')
```

For details on how to use it, please refer to  [the official document](https://huggingface.co/docs/datasets/index.html).

## Fine-tuning and Evaluation
We fine-tuned [Swedish BERT](https://github.com/Kungbib/swedish-bert-models) pre-trained by the National Library of Sweden. The model is availabel in [HuggingFace model hub](https://huggingface.co/susumu2357/bert-base-swedish-squad2).

```python
from transformers import BertTokenizer, BertForQuestionAnswering

tokenizer = BertTokenizer.from_pretrained('susumu2357/bert-base-swedish-squad2')
model = BertForQuestionAnswering.from_pretrained('susumu2357/bert-base-swedish-squad2')
```
For details on how to use it, please refer to  [the official document](https://huggingface.co/transformers/model_doc/bert.html#bertforquestionanswering).

### Evaluation on SQuAD_v2_sv dev

The results of evaluation are summarized in the below table. We have evaluated [the Multilingual XLM-RoBERTa for SQuAD](https://huggingface.co/deepset/xlm-roberta-large-squad2), [the KB Lab model for SQuAD](https://huggingface.co/KB/bert-base-swedish-cased-squad-experimental), and our model that is fine-tuned on the SQuAD_v2_sv train dataset.

Our model which has about 110M parameters achieves better scores than the KB Lab model, and establishes a close F1 score compared to the XLM-RoBERTa <sub>large</sub> which has about 550 M parameters.

| Model | Exact Match | F1 |
| ------ | ------ | ------ |
| Multilingual XLM-RoBERTa <sub>large</sub> | 56.96 | **70.78** |
| Swedish BERT <sub>base</sub> (KB Lab) | 65.65 | 68.89 |
| Swedish BERT <sub>base</sub> (Ours) | **66.73** | 70.11 |

### Evaluation on Nobel Prize dataset

We have internally created the Nobel Prize dataset for evaluation purposes. This dataset contains descriptions of recent Nobel Prize as contexts, manually created questions, and manually extracted answers. The dataset contains 91 question and answer pairs, so the size is small but valuable for evaluation. 
Since this dataset was not used for fine-tuning, this result is realistic if we use the fine-tuned model without further training.

The results of evaluation are summarized in the below table.
Our model achieved better results in both exact match and F1 score.

| Model | Exact Match | F1 |
| ------ | ------ | ------ |
| Multilingual XLM-RoBERTa <sub>large</sub> | 13.19 | 60.00 |
| Swedish BERT <sub>base</sub> (KB Lab) | 32.97 | 52.41 |
| Swedish BERT <sub>base</sub> (Ours) | **46.15** | **61.54** |


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