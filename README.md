# CK-Debias: Absorbing Commonsense Knowledge from LLMs for Improving Social Fairness in Pre-trained Language Models 


## Environment Setup

Create environment:

```bash
conda create -n debias python=3.8.5
conda activate debias
```
Install pytorch and python packages:

```bash
conda install -n pt2 pytorch==1.7.1 torchvision==0.8.2 torchaudio==0.7.2 cudatoolkit=11.0 -c pytorch
cd CK-Debias
pip install -r requirements.txt
```

We provide python scripts to replicate our findings. Our environment is:

* Ubuntu servers with NVIDIA GeForce RTX 3090 (24G) GPUs
* cuda 11.4
* packages with certain versions

We conduct experiments on the pre-trained model including bert-base-uncased, albert-base-v2 and roberta-base from [HuggingFace](https://huggingface.co/).
With the `transformers` package installed, you can import the off-the-shelf model like so: 
```python
from transformers import AutoTokenizer, AutoModelForMaskedLM
tokenizer = AutoTokenizer.from_pretrained(args.model_name_or_path)
model = AutoModelForPreTraining.from_pretrained(args.model_name_or_path)
```

## Data

We've already included word lists for attributes in the `./data` folder, so there is no need to acquire them from other resources. As for larger corpora, The corpus extracted from GPT is placed in the `./data/gpt_data` directory.


## Experiments

Convert sentences to embeddings and divide the data into XT with a collision effect and XNT without a collision effect :

```
python preprocess_data.py \
--debias_type   gender or race or religion
--model_type   bert or albert or roberta
--model_name_or_path  bert-base-uncased, etc
python save_onemask.py

```
Filter out the most biased prompts from the XNT data :

```
python generate_biased_prompts.py \
--debias_type   gender or race or religion
--model_type   bert or roberta or albert
--model_name_or_path  bert-base-uncased, etc

```
Debias language models 
```
python ck_debias.py
--debias_type    gender or race or religion
--model_type   bert or roberta or albert
--model_name_or_path  bert-base-uncased, etc
```
