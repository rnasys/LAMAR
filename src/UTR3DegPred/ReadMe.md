# Fine-Tuning LAMAR for mRNA Half-Life Prediction
This directory contains scripts for fine-tuning LAMAR to predict mRNA half-lives using three-prime UTR (3' UTR) sequences.

## Dataset
The dataset is derived from a massive parallel reporter assay that measured the effect of 1,967 human 3' UTRs on the half-lives of eGFP mRNAs [1].

The model performance is evaluated by predicting the effect of 3' UTRs on mRNA half-lives using ten-fold cross-validation.

The examples of dataset files are located in LAMAR/UTR3DegPred/data and include:

`UTR3DegPred/data/training_set_2.csv`

`UTR3DegPred/data/testing_set_2.csv`

`UTR3DegPred/data/validation_set.csv`

## Workflow
### 1. Data Tokenization
Run `tokenize_data.ipynb` to tokenize the 3' UTR sequences into lists of tokens.

Input Files:

`UTR3DegPred/data/training_set_2.csv`

`UTR3DegPred/data/testing_set_2.csv`

Output Directory:

`UTR3DegPred/data/deg_2`

### 2. Model Fine-Tuning
Run `finetune.ipynb` to fine-tune LAMAR for mRNA half-life prediction. Here, the fine-tuned head contains two linear layers. During fine-tuning, the final embedding of [cls] token is input into the fine-tuned head.  

Input Directory:

`UTR3DegPred/data/deg_2`

Output Directory:

`UTR3DegPred/saving_model/mammalian_4096/bs8_lr5e-5_wr0.05_16epochs_2`

In output directory,  
The fine-tuned weight is stored in `pytorch_model.bin`.  
The training and testing loss are included in `trainer_state.json`.  

Hyperparameter Configuration:  
Before training, set the following hyperparameters:

`Batch size`

`Peak learning rate`

`Number of training epochs`

Here, we used the optimal hyperparameters determined through cross-validation: batch size = 8, learning rate = 1e-4, number of training epochs = 16.  

During fine-tuning, the training and testing loss will be displayed every 100 steps.

### 3. Model Evaluation
Run `evaluation.ipynb` to evaluate the model's performance.

Evaluation Metrics:

`Mean Squared Error (MSE)`

`Spearman Correlation Coefficient`

Reference
[1] Zhao W, Pollack JL, Blagev DP, Zaitlen N, McManus MT, Erle DJ. Massively parallel functional annotation of 3′ untranslated regions. Nat Biotechnol. 2014;32:387–91.
