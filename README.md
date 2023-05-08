# Benchmarking Deep Learning Architectures for predicting Readmission to the ICU and Describing patients-at-Risk

## Summary
This repository is a fork based on https://github.com/sebbarb/time_aware_attention. We reproduced the original authors results with different hyperparameters, as well as contributed new models to the original paper. The original repo lacked some instructions and dependency specifications, which made running the pipeline for the first time painful. We addressed those painpoints in our repositories. 

## References:

This repository contains code related to the publication: Benchmarking Deep Learning Architectures for predicting Readmission to the ICU and Describing patients-at-Risk by Sebastiano Barbieri, James Kemp, Oscar Perez-Concha, Sradha Kotwal, Martin Gallagher, Angus Ritchie, Louisa Jorm

## Setup Instructions

### Clone the Repo
Run `git clone https://github.com/tonymuu/e2e_time_aware_attention.git` to clone the git repo

### Dependencies
Run `pip install` from the `related_code` directory of the repo. This will install the following packages:
```
matplotlib==3.7.1
numpy==1.21.5
pandas==2.0.0
scikit_learn==1.2.2
scipy==1.10.1
statsmodels==0.13.5
torch==2.0.0
torchdiffeq==0.2.3
tqdm==4.65.0
```

### Data Download Instructions
1. Create a new directory “mimic3” at the same directory of cloned repo (in our case, the cloned repo is located at `~/Projects/cs598-dlh-project/mimic3`)
2. Download compressed MIMIC-III dataset from https://physionet.org/content/mimiciii/1.4/. You will need a certificate and some required training to access the data. Place the downloaded file in the above directory.
2. Run `for f in *.gz ; do gunzip -c "$f" > ~/Projects/cs598-dlh-project/mimic3/"${f%.*}" ; done`
3. This will take a while with no outputs 
4. The output folder will be in the above command

### Preprocessing
1. Create a directory `data` at the same directory of `related_code`
2. Run preprocessing tasks that converts MIMIC-3 raw data into processed pickle/csv files in the **following order** (order is important!). The general syntax is `python3 preprocessing_ICU_PAT_ADMIT.py`:
    1. preprocessing_ICU_PAT_ADMIT
    2. preprocessing_DIAGNOSES_PROCEDURES
    3. preprocessing_reduce_outputs
    4. preprocessing_reduce_charts
    5. preprocessing_merge_charts_outputs
    6. preprocessing_CHARTS_PRESCRIPTIONS
    7. preprocessing_create_arrays
3. Copy embeddings from `related_code` directory to `data_dir` directory: `cp ./related_code/embeddings/* ./data/`

### Training Pipeline
To run training pipeline, do `python3 train.py` from `related_code` directory. 
    ![Training Pipeline](/images/train.png)

### Testing/Evaluation Pipeline
1. Run `cp -R ./trained_models/ ./logdir/` from the project root directory, so the test script knows where to find `trained_models`
2. Run `python3 test.py` from `related_code` directory 
3. Each bootstrap sample is taking on average 20 seconds to complete. Default is 100. We will use 20 instead.
4. Some sample results
    ![Testing Pipeline](/images/test.png)

