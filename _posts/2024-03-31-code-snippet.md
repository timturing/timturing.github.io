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

