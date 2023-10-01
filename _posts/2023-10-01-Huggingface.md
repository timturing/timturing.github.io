As a researcher in China, it's a little bit hard to use the huggingface and other foreign websites for studying. This post will show you how to use huggingface more conveniently.

## 1. Set proxy and cache
As we all know, the huggingface is a foreign website, so we need to set proxy to access it. Besides, for a local machine, we need to set cache to save the downloaded models and datasets. Unfortunately, the huggingface doesn't support setting proxy, and its dfeault cache path is in the C disk directory in Windows. So we need to set proxy and cache manually.
```python
import os
# Set proxy environment variables
os.environ['HTTP_PROXY'] = 'http://127.0.0.1:10809'
os.environ['HTTPS_PROXY'] = 'http://127.0.0.1:10809'
# Set huggingface cache
os.environ['HF_HOME'] = 'D:/Users/LUO/.cache/huggingface/'

# ❗ Import other packages only after setting proxy and cache
import requests
from datasets import load_dataset
from transformers import AutoTokenizer, AutoModelForSequenceClassification

imdb_dataset = load_dataset("imdb")
tokenizer = AutoTokenizer.from_pretrained("bert-base-cased-finetuned-mrpc")
model = AutoModelForSequenceClassification.from_pretrained("bert-base-cased-finetuned-mrpc")
```
❗ Keep in mind that you should import other packages only after setting proxy and cache.

## 2. Changing the dataset format so that it can be upload to the server
For most of the deep learning researchers, we need to upload our dataset to the server for training (as there will be some GPUs). However, some huggingface dataset format (e.g. .arrow file) is not supported if the server cannot access to the huggingface. So we need to change the dataset format before uploading it to the server.
```python
dataset['train'].to_json('train.jsonl', orient='records', lines=True)
```

