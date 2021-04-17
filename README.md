# COLLABERT
COLLABERT is a biomedical Repository
# COLLABERT: A Collaborative Bio-BERT For Biomedical Named Entity Recognition


This project provides a neural network (BioBERT) approach for biomedical Named Entity Recognition.  
Our implementation is based on the Tensorflow library on python.  
  
* __TITLE__  :  COLLABERT: A Collaborative Bio-BERT For Biomedical Named Entity Recognition
 
* __AUTHOR__ :  Ashutosh Kumar<sup>1</sup> and Aakanksha Sharaff<sup>2</sup>,
    * __Author details__  
    <sup>1</sup> Department of Computer Science and Engineering, National Institute of Technology Raipur, Raipur Chhattisgarh, India  
    <sup>2</sup> Department of Computer Science and Engineering, National Institute of Technology Raipur, Raipur Chhattisgarh, India  
    <sup>!</sup> Equal contributor  


## Quick Links

- [Requirements](#requirements)
- [Model](#model)
- [Data](#data)
- [Usage](#usage)
- [Performance](#performance)

## Requirements
At least one CUDA compatible GPU (NVIDIA GTX) device is strongly recommanded for execution of this project codes.  
python 3.4 
numpy 1.20.0  
tensorflow-gpu 2.4.1  
### License
The code is distributed under NIT Raipur

## Model
**[LEFT]** Character level word embedding using CNN and overview of Bidirectional LSTM with Conditional Random Field (BiLSTM-CRF).  
**[RIGHT]** Structure of CollaboNet when Gene model act as a role of target model. Rhombus represents the CRF layer. Arrows show the flow of information when target model is training. Dashed arrows mean that information is not flowing when target model is under training.  
![Model](http://wonjin.info/file/model_tot.png)

## Data
### Train, Test Data
We used datasets collected by Crichton et al.  
These datasets by Crichton et al. are available [here](https://github.com/cambridgeltl/MTL-Bioinformatics-2016).  
We found that the JNLPBA dataset from Crichton et al. contains sentences which were incorrectly split.  
So we re-generated the dataset from the original corpus by [Kim et al.](http://www.nactem.ac.uk/tsujii/GENIA/ERtask/shared_task_intro.pdf).  

The details of each dataset is showed below:  


|               Corpora               |  Entity type  | CorporaSize  |  #annotations |     #sentence    |
|:-----------------------------------:|:-------------:|:------------:|:---------------:|:----------------:|
|  BC2GM (Akhondi et al., 2014)       | Gene/Proteins | 20,000 Sentences    |     24,583      |   20,510  |
|      JNLPBA (Kim et al., 2004)      | Gene/Proteins |    2,404 abstracts    |      35,336     |  22,562 |
|    NCBI-Disease (Dogan et al., 2014)|    Diseases   |    793 abstracts    |      6,881     |  7,639  |
|       BC5CDR (Li et al., 2016)      |    Diseases   |    1,500 articles    |      12,852     |  14,228  |
| BC4CHEMD (Krallinger et al., 2015a) |   Chemicals   |    10,000 abstracts    |      84,310     | 86,679 |
|     BC5CDR (Li et al., 2016)        |   Chemicals   |    1,500 articles    |      15,935    | 14,228 |

The datasets are publicly available by executing [download.sh](./download.sh).<!-- and we recommend downloading the datasets to run our code.  -->

### Pre-trained Embeddings
We used pre-trained word embeddings from [Pyysalo et al.](http://bio.nlplab.org/) which is trained on PubMed, PubMed Central(PMC) and Wikipedia text. It will be automatically downloaded by executing [download.sh](./download.sh). 

## Usage
### Download Data
```
bash download.sh
```

### Single Task Model [STM] (6 datasets)
__Preperation phase (Phase 0) of CollaboNet__  
```
python run.py --ncbi --jnlpba --bc5_chem --bc5_disease --bc4 --bc2 --lr_pump --lr_decay 0.05
```
You can also refer to [stm.sh](./stm.sh) for detailed usage.

### CollaboNet (6 datasets)
__You should produce pre-trained STM model by executing Preperation phase before running CollaboNet.__  
```
python run.py --ncbi --jnlpba --bc5_chem --bc5_disease --bc4 --bc2 --lr_pump --lr_decay 0.05 --pretrained STM_MODEL_DIRECTORY_NAME(ex 201806210605)
```
You can find STM_MODEL_DIRECTORY_NAME from ./modelSave folder.  
You can also refer to [collabo.sh](./collabo.sh) for detailed usage. 


## Performance
### STM
|           Model          |          | NCBI-disease | JNLPBA | BC5CDR-chem | BC5CDR-disease | BC4CHEMD | BC2GM | Average |
|:------------------------:|:--------:|:------------:|:------:|:-----------:|:--------------:|:--------:|:-----:|:-------:|
| Habibi et al. (2017) STM | F1 Score |     84.44    |  77.25 |    90.63    |      **83.49**     |   86.62  | 77.82 |  83.38  |
|  Wang et al. (2018) STM  | F1 Score |     83.92    |  72.17 |    *89.85   |     *82.68     |   **88.75**  | **80.00** |  82.90  |
|          **Our STM**         | F1 Score |     **84.69**    |  **77.39** |    **92.74**    |      82.61     |   88.40  | 79.27 |  **84.03**  |
* Scores in the asterisked (\*) cells are obtained in the experiments that we conducted; these scores are not reported in the original papers.   
* The best scores from these experiments are in bold.  

### CollaboNet
|                        |          | NCBI-disease |   JNLPBA  | BC5CDR-chem | BC5CDR-disease |  BC4CHEMD |   BC2GM   | Average |
|:----------------------:|:--------:|:------------:|:---------:|:-----------:|:--------------:|:---------:|:---------:|:-------:|
| Wang et al. (2018) MTM | F1 Score |     86.14    |   73.52   |    *91.29   |     *83.33     | **89.37** | **80.74** |  84.07  |
|   **Our CollaboNet**   | F1 Score |   **86.36**  | **78.58** |  **93.31**  |    **84.08**   |   88.85   |   79.73   |  **85.15**  |
* Scores in the asterisked (\*) cells are obtained in the experiments that we conducted; these scores are not reported in the original papers.   
* The best scores from these experiments are in bold.
