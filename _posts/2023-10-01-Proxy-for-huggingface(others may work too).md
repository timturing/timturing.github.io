As a researcher in China, I have to use proxy to access the huggingface and other websites for studying. This is a simple way to do it.
```python
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