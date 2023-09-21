It's a little annoying to set up the python environment on the server. Here is a simple way to do it.

## 1. Install Anaconda
First find the Anaconda version that fit your need from [here](https://repo.anaconda.com/archive/). Then you can use wget to download it to your server. For example, if you want to install Anaconda3-2021.05-Linux-x86_64.sh, you can use the following command:
```bash
wget https://repo.anaconda.com/archive/Anaconda3-2021.05-Linux-x86_64.sh
```
Before we install it, we may need to change the permission of the file. Use the following command to change the permission:
```bash
chmod +x Anaconda3-2021.05-Linux-x86_64.sh
```
Then we can install it:
```bash
bash Anaconda3-2021.05-Linux-x86_64.sh
```


## 2. Create a virtual environment
After we install Anaconda, we can create a virtual environment. For example, if we want to create a virtual environment named "myenv", we can use the following command:
```bash
conda create -n myenv python=3.8
```
Then we can activate the virtual environment:
```bash
conda activate myenv
```
Now we can install the packages we need. For example, if we want to install pytorch, we can use the following command:
```bash
conda install pytorch torchvision torchaudio cudatoolkit=11.1 -c pytorch -c conda-forge
```
After we install all the packages we need, we can use the following command to save the environment:
```bash
conda env export > environment.yml
```
Then we can deactivate the virtual environment:
```bash
conda deactivate
```

## 3. Copy an env 

If you have a conda virtual environment like creating it by yourself above (or other's share), you can use this command to build a same one:

```bash
conda env create -f environment.yml
```

export the package list:

```bash
conda list -e > condalist.txt
```

import the package list:

```bash
conda install --yes --file condalist.txt
```

