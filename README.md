# Crystallography Companion Agent (xca)

## Overview
The following is a permanent repository of published examples using XCA, available on the [arXiv](https://arxiv.org/abs/2008.00283). 
XCA is a psuedo-unsupervised learning approach for handling spherically integrated (powder) X-ray diffraction data.
The approach depends on accurate data synthesis that encompasses the multitude of abberations which can impact a diffraction pattern. 
The magnitude of influence of these aberrations is meant to be informed by experience given an experimental design (*e.g.* 
well prepared powders will experience significantly less texturing than epitaxially grown thin films.)
The dataset synthesis is accomplished using the [cctbx](https://cctbx.github.io/), starting from `.cif` files of potential phases. 
From synthetic datasets an ensemble of feed forward convolutional neural networks can be trained, and subsequently used
to predict phase existence in experimental data. 

# System requirements
## Hardware requirements
`xca` package requires only a standalone computer with enough RAM to support the in-memory operations.
For advanced use, a CUDA enabled GPU is recommended. 

## Software requirements
### OS requirements
This package is supported for macOS and Linux. The package has been tested on the following systems:
- macOS: Catalina (10.15.7)
- Linux: Ubuntu 18.04

### Python dependencies 
`xca` Dataset generation makes extensive use of the [cctbx](https://cctbx.github.io/), 
which is currently best installed into a conda environment. 

The machine learning depends on a scientific tensorflow stack: 
```
tensorflow >= 2.1.0
# tensorflow-gpu will be installed if a gpu is available 
numpy
scikit-learn
scipy
``` 

## Installation guide
Due to the current unavailability of the cctbx on PyPi channels, we recommend first setting up a 
conda environment for the cctbx. The remaining dependencies can be installed via pip. 
The scripts here depend on a specific tag of XCA from the time of publication. 
XCA is under active development, and the most up-to-date version may not be compatible with these examples.
```
conda create -n xca -c conda-forge cctbx-base jupyter jupyterlab pandas python=3.7
conda activate xca
git clone -b pub https://github.com/maffettone/xca
cd xca
python -m pip install .
cd ../
git clone https://github.com/bnl/pub-Maffettone_2020_08
cd pub-Maffettone_2020_08
``` 


# Getting started
## A simple demonstration 
A simple example of the full training pipeline is demonstrated in the
[simple_example.py script](example_scripts/simple_example.py). 
Executing this will do the following in a tmp directory:  
1. Synthesize 100 example patterns for each phase of the three experimental systems presented in the paper below.
2. Convert those patterns into a tfrecords object. 
3. Train an ensemble model, print the results, and save the full model.  

```
cd example_scripts
python simple_example.py
```

This will take a few minutes to run for each example, approximately 10 minutes in total. 
The output will print the path to each `cif` file as it generates the relevant set of synthetic data. This includes
4 files for BaTiO, 5 files for ADTA, and 31 files for Ni-Co-Al. The output will then print the ensemble model summary. 
The time for each epoch will be displayed on with 8 epochs for each model training. 
Lastly results dictionary will be sloppily printed to show the loss, training, and validation metrics. 


Details of generic synthesis and training can be found in 
[example_synthesis.py](example_scripts/example_synthesis.py) and 
[example_training.py](example_scripts/example_training.py).  

## Literature details 
The application of this package is published in [*Nature Computational Science*](https://doi.org/10.1038/s43588-021-00059-2), 
with a preprint at [aXiv:2008.00283](https://arxiv.org/abs/2008.00283).
To reproduce the models presented in this paper, the dataset synthesis should be scaled (use of `multiprocessing` is 
encouraged) to produce 50,000 patterns per phase using the same parameterization presented in 
[example_synthesis.py](example_scripts/example_synthesis.py). 

For increased accessibility, these procedures are also included as [jupyter notebooks](example_notebooks),
and the experimental data for comparison is given in [example_data](example_data).

**ABSTRACT:** The discovery of new structural and functional materials is driven by phase identification, often using X-ray diffraction (XRD). Automation has accelerated the rate of XRD measurements, greatly outpacing XRD analysis techniques that remain manual, time consuming, error prone, and impossible to scale. With the advent of autonomous robotic scientists or self-driving labs, contemporary techniques prohibit the integration of XRD. Here, we describe a computer program for the autonomous characterization of XRD data, driven by artificial intelligence (AI), for the discovery of new materials. Starting from structural databases, we train an ensemble model using a physically accurate synthetic dataset, which output probabilistic classifications --- rather than absolutes --- to overcome the overconfidence in traditional neural networks. This AI agent behaves as a companion to the researcher, improving accuracy and offering unprecedented time savings, and is demonstrated on a diverse set of organic and inorganic materials challenges. This innovation is directly applicable to inverse design approaches, robotic discovery systems, and can be immediately considered for other forms of characterization such as spectroscopy and the pair distribution function.

**Code DOI:** [10.11578/dc.20210316.6](https://doi.org/10.11578/dc.20210316.6)

**Paper DOI:**[10.1038/s43588-021-00059-2](https://doi.org/10.1038/s43588-021-00059-2)
