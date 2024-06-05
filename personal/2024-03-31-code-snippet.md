### watch for free gpus

```python
import GPUtil
import os
import time
def watch_gpus():
    gpus = GPUtil.getGPUs()
    avaiable_gpu_list = []
    for gpu in gpus:
        used_memory_ratio = gpu.memoryUsed / gpu.memoryTotal
        if used_memory_ratio <= 0.10:
            avaiable_gpu_list.append(gpu.id)
    
    pcie_pair = [(0,9),(2,4),(5,6),(7,8)]
    for pair in pcie_pair:
        if pair[0] in avaiable_gpu_list and pair[1] in avaiable_gpu_list:
            os.system('export CUDA_VISIBLE_DEVICES=' + str(pair[0]) + ',' + str(pair[1])+ ' && cd sft && bash bash/run_acc_ppo.sh')
            exit()


while True:
    watch_gpus()
    time.sleep(60*10) # check periodically
```

### generate manully

```python
from transformers import LlamaForCausalLM, LlamaTokenizer
import torch

path = '/home/tangtianyi/meta-llama/Llama-2-7b-hf'
model = LlamaForCausalLM.from_pretrained(path)
tokenizer = LlamaTokenizer.from_pretrained(path)

prompt = "Q: 我怎样才能提高我的时间管理技能？请用中文回答。\nA:"
input_ids = tokenizer.encode(prompt, return_tensors='pt')

for i in range(10):
    # Generate the next token
    hidden_states = model(input_ids, output_hidden_states=True).hidden_states
    last_hidden_state = hidden_states[-1][:, -1, :]
    output_dist = model.lm_head(last_hidden_state)

    # Get the top 5 tokens and their probabilities
    topk = torch.topk(output_dist, 5)
    topk_tokens = topk.indices[0]
    topk_probs = topk.values[0]

    # Print the tokens and their probabilities
    for token, prob in zip(topk_tokens, topk_probs):
        print(tokenizer.decode(token.item()), prob.item())
    
    print("*" * 100)

    # Append the most probable token to the input sequence
    next_token = topk_tokens[0].unsqueeze(0).unsqueeze(0)  # Adding batch and sequence dimensions
    input_ids = torch.cat((input_ids, next_token), dim=1)

    # Update the prompt with the newly generated token
    prompt += tokenizer.decode(next_token[0])
    
    print(prompt)

```

