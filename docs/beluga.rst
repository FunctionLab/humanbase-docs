=======
Beluga (DeepSEA)
=======

Introduction
------------

DeepSEA is a deep learning-based algorithmic framework for predicting the chromatin effects of sequence alterations with single nucleotide sensitivity. DeepSEA can accurately predict the epigenetic state of a sequence, including transcription factors binding, DNase I sensitivities and histone marks in multiple cell types, and further utilize this capability to predict the chromatin effects of sequence variants and prioritize regulatory variants. Importantly, this framework is trained without using any variant data, allowing it to predict the chromatin impact of any variant, including rare or previously unseen ones. The 2019 version of DeepSEA, nicknamed '**Beluga**', can predict **2002** chromatin features.

Beluga is described in: Jian Zhou, Chandra L. Theesfeld, Kevin Yao, Kathleen M. Chen, Aaron K. Wong, and Olga G. Troyanskaya, `Deep learning sequence-based ab initio prediction of variant effects on expression and disease risk <https://www.nature.com/articles/s41588-018-0160-6>`_. Nature Genetics (2018).


DeepSEA is originally described in the following manuscript: Jian Zhou, Olga G. Troyanskaya. `Predicting the Effects of Noncoding Variants with Deep learning-based Sequence Model <https://www.nature.com/articles/nmeth.3547>`_ Nature Methods (2015).

To determine if certain features (ie. transcription factors, marks, or cell types) are present/accounted for in the model, refer to the `supplemental feature table <https://s3-us-west-2.amazonaws.com/humanbase-dev/deepsea/examples/41588_2019_420_MOESM9_ESM.csv>`_ which has all the profiles used to train Beluga.


Input
-----

Beluga predicts genomic variant effects on a wide range of chromatin features at the variant position (Transcription factors binding, DNase I hypersensitive sites, and histone marks in multiple human cell types). 

.. |bp_length| replace:: 2000
.. |bed_example| replace:: ``chr5 134871851 134871852``

.. include:: _includes/common-input-formats.rst

.. include:: _includes/common-submission-info.rst

Output
------

Regulatory feature scores
~~~~~~~~~~~~~~~~~~~~~~~~~
* **diffs**: The difference between the the predicted probability of the reference allele and the alternative allele for a regulatory feature (:math:`p_{alt} -p_{ref}`).
* **e-value**: E-value is defined as the expected proportion of SNPs with a larger predicted effect. We calculate an 'e-value' based on the empirical distribution of that feature's effect (:math:`abs(p_{alt} -p_{ref})`) among gnomAD variants. For example, a feature e-value of 0.01 indicates that only 1% of gnomAD variants have a larger predicted effect.
* **z-score**: A scaled score where the feature diff score (:math:`p_{alt} -p_{ref}`) is divided by the root mean square of the feature diff score across gnomAD variants. Note that this is "sign-preserving", i.e. a negative z-score indicates that a mutation **decreases** the probability of a regulatory feature.

Variant scores
~~~~~~~~~~~~~~

* **Disease Impact Score (DIS)**: DIS is calculated by training a logistic regression model that prioritizes likely disease-associated mutations on the basis of the predicted transcriptional or post-transcriptional regulatory effects of these mutations (See `Zhou et. al, 2019 <https://www.nature.com/articles/s41588-019-0420-0>`_). The predicted DIS probabilities are then converted into 'DIS e-values', computed based on the empirical distributions of predicted effects for gnomAD variants. The final DIS score is:

  .. math::
      -log10(DIS evalue_{feature})

* **Mean -log e-value (MLE)**: For each predicted regulatory feature effect (:math:`abs(p_{alt}-p_{ref}`)) of a variant, we calculate a 'feature e-value' based on the empirical distribution of that feature's effects among gnomAD variants (see above Regulatory feature scores: e-value). The MLE score of a variant is

  .. math::
      \sum -log10(evalue_{feature}) / N

