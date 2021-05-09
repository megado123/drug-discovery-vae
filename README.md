CS-598 Deep Learning for Health Care Final Project
==================================================

As part of our CS-598 Final Project, we are exploring methods for drug discovery
using Variational Autoencoders. Our objective is to measure the ratio of
syntactically valid molecules that the models can produce to determine which
adjustments to the baseline model produce the largest improvements.

Sources and Inspiration
-----------------------

The inspiration and sources for this repository come from:

\* [Automatic Chemical Design Using a Data-Driven Continuous Representation of
Molecules](https://arxiv.org/abs/1610.02415)

\* <https://github.com/aspuru-guzik-group/chemical_vae>

\* <https://github.com/deepchem/deepchem>

\* <https://github.com/molecularsets/moses>

\*
<https://docs.microsoft.com/en-us/azure/machine-learning/quickstart-create-resources>

\* <https://github.com/wengong-jin/icml18-jtnn>

<https://en.wikipedia.org/wiki/ZINC_database>

<http://zinc15.docking.org/>

<https://github.com/wengong-jin/icml18-jtnn/tree/master/data/zinc>

https://github.com/joeym-09/Leveraging-VAE-to-generate-molecules/blob/master/VAE_model_250k.ipynb

Journey
-------

### 1. Exploring the Data

First we explored a dataset of 250K molecules to understand the dataset a little
better. None of us have a background in chemistry or biology so this was fairly
essential. The exploration can be reproduced by running `Data_Exploration.ipynb`
in a jupyter notebook environment.

### 2. Baseline Model using Deepchem

After exploring the data, we leveraged the deepchem library to train an instance
of the AspuruGuzikAutoEncoder using our 250K molecule dataset. This work can be
reproduced by running the notebook (inside an Azure ML Workspace):

\- `Approach1_BaselineModel.ipynb`

### 3. AspuruGuzikAutoEncoder with KL Annealing

KL Annealing is used to prioritize reconstruction loss early in the training
process and then later incorporate more of the Kullback–Leibler (KL) loss. The
baseline model provided by deepchem and we wanted to test the impact of turning
it off. Those results can be reproduced by running

\-Approach2_DisablingCostAnnealing.ipynb.

### 4. Teacher Forcing

Teacher forcing is a technique to help the decoder along when doing training so
that the decoding of the characters later in a sequence get a better chance to
train. For example, if I'm trying to reconstruct
`C[NH+](C/C=C/c1ccco1)CCC(F)(F)F` and my decoder messes up the first couple of
characters, the teacher-forcing model will propagate the correct character at
that iteration of the decoder so that the following characters have a better
chance of also being correct. This allows for the whole sequence being
trained/learned at the same time instead of depending on the initial decoding to
be correct in order to get the remaining characters. We attempted to add this
technique to the deepchem library, but ultimately our efforts were unsuccessful.
Ultimately we were able to use a VAE model with teacher forcing enabled using
the moses library. That implementation can be reproduced by running
`moses_vae.ipynb`

The results can be reproduced by running:

\- Approach3_Moses.ipynb

### 5. SELFIES

Up to this point, our models has used the SMILES representation to represent a
molecule as a string. We were able to experiment with a different string
representation of a molecule called SELFIES. Those results can be reproduced by
running `04_RunOnRemoteClusterSelfies2.ipynb`.

Conculsion
----------

Hopefully these notebooks are useful to you in your drug discovery journey!
