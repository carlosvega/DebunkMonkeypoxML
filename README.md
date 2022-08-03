# Mini review of Monkeypox ML pre-print paper

Quick replication of [arXiv:2206.01862v1](https://arxiv.org/abs/2206.01862) pre-print paper.

# Motivation

During the COVID-19 pandemic, several researchers rushed to develop solutions for the diagnosis of COVID-19 by employing X-Ray images. 
The solutions and datasets developed during the initial stages of the pandemic broke all the rules and guidelines regarding ethical data science and proper scientific methodology.
Months later, several works were published criticising and highlighting the pitfalls and mistakes committed during the pandemic.

Works such as:
- https://www.nature.com/articles/s42256-021-00307-0
- https://www.sciencedirect.com/science/article/pii/S136184152100270X

With the raise of cases of monkeypox several researchers were afraid that the same mistakes would be made again.
One of the first publications available in google scholar and arXiv includes the paper reviewed in this repository. 
Published in arXiv under the identifier [arXiv:2206.01862v1](https://arxiv.org/abs/2206.01862) in June 2022.

# Summary

The pre-print paper published in arXiv aims to identify Monkeypox patients with an accuracy of ~97%.

The researchers from the University of Oklahoma present two "studies":

- Study one: Aims to classify between Monkeypox and Chickenpox.
- Study two: Aims to classify between Monkeypox and others.

This review hypothesises that the trained classifier is not learning to identify the diseases but rather, to separate the two sets of images. Therefore, it is not learning the phenomena or any feature regarding the diseases.
To test this hypothesis the dataset was "blinded" by adding rectangles over the regions of interest in the images, which are presumed to be the blisters, pustules and rash areas.

# Methodological issues

The pre-print presents a number of methodological issues.

- The authors did not share the code but shared enough details to somewhat replicate the results. 
- The authors did share the [dataset employed](https://github.com/mahsan2/Monkeypox-dataset-2022) which available in [GitHub](https://github.com/mahsan2/Monkeypox-dataset-2022).
- The authors provide a poorly collected dataset (from google) with samples from Monkeypox, Chickenpox, Measles and Normal.
- The images provided have not been reviewed or curated by medical experts on the area.
- Class imbalance and data augmentation seem not properly handled.

## Data adquisition

The authors did not utilize any medical dataset. Rather, they collected samples from google purposedly selecting images with commercial rights.
In consequence, the dataset contains images from Getty Images, Shutterstock, Dreamstime and other stock image repositories.
The images lack any coherence regarding the framed view, modality or body parts photographed.

The employed dataset would be enough reason to reject or discard the paper.

Still, the authors, which have published other research works before, state: 

>"We hope our publicly available dataset will play an important role and provide the opportunity to the ML researcher who cannot develop an AI-based model and conduct the experiment due to data scarcity."

![chickenpox example 1](./figs/chicken12.jpg)

![chickenpox example 2](./figs/chicken13.jpg)

## Data augmentation

The paper does not clarify whether the data augmentation was done in a way that prevents data leakage of augmented instances into the valid set.

## Class imbalance

The authors acknowledge the class imbalance but do not rebalance the dataset in any way, for instance, augmenting just the imbalanced classes. They do not mention any other measure to tackle the imbalance.

## External Validation

The authors do not provide any external validation. Their valid set consists of a split of images from the same dataset which has the same generative process. A tool aimed to conduct clinical diagnosis should be thoroughly evaluated against different external datasets to increase the confidence on the model. Otherwise chances are we are just building a classifier to fit a dataset.

# Review

**Disclaimer**: This review purposedly avoids to fix the class imbalance issues to resemblance the original work.

The images were "blinded" by adding rectangles over the regions of interest of the images, which are presumed to be the blisters, pustules and rash areas. See examples below:

![blind example 1](./figs/blind_sample_a.png)

![blind example 2](./figs/blind_sample_b.png)

## Methods

Given that this is a quick review, we employed FastAI v2, using a pre-trained VGG16 with batch normalization.

## Results

The resulting model can classify the elements between the two folders in both "studies" even though the relevant areas of the images have been "blinded". Therefore, the model built by the authors is not properly modelling the phenomena they try to model, therefore it is not suited for clinical diagnosis, neither the dataset is recommendable for any research on medical machine learning.

![results](./figs/results.png)

## Remarks

**In short, researchers seem to believe that naming folder chickenpox and monkeypox and training a classifier with whatever the content is can be enough to build a solution that actually distinguishes between chickenpox and monkeypox.**

Fortunately, the authors' paper has not been published in any scientific journal or conference proceedings yet. However, it is now common to cite and reference papers published in publication repositories, pre-print servers and other non-reviewed publication services. It is important to remain sceptical and careful with non-reviewed publications (and even with peer-reviewed publications). Their dataset remains open and available for the potential misuse of the research community.

## Precedents

A similar [repository](https://github.com/ieee8023/covid-chestxray-dataset) (though with better quality) was built to gather images of X-Ray scans of COVID-19 patients. This repository suffered from several issues that were critically evaluated in some publications.

- https://arxiv.org/abs/2004.12823
- https://arxiv.org/abs/2004.05405


![precedent](./figs/precedent.png)


# Versions of employed software

- fastai 2.7.8
- tensorflow 2.5.0