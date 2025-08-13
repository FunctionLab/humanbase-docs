=======
Sei
=======

Introduction
------------

Sei is a deep-learning-based framework for systematically predicting sequence regulatory activities and applying sequence information to understand human genetics data. Sei provides a global map from any sequence to regulatory activities, as represented by 40 sequence classes. Each sequence class integrates predictions for 21,907 chromatin profiles (transcription factor, histone marks, and chromatin accessibility profiles across a wide range of cell types) from the underlying Sei deep learning model. Importantly, this framework is trained without using any variant data, allowing it to predict the regulatory impact of any variant, including rare or previously unseen ones.

Sei is described in the following manuscript: Kathleen M. Chen, Aaron K. Wong, Olga G. Troyanskaya and Jian Zhou, `A sequence-based global map of regulatory activity for deciphering human genetics <https://www.nature.com/articles/s41588-022-01102-2>`_. Nature Genetics (2022).

The Sei code repository can be found `here <https://github.com/FunctionLab/sei-framework>`_.

For older DeepSEA models see:
:doc:`beluga` (2019)


Input
-----

.. |bp_length| replace:: 4096
.. |bed_example| replace:: ``chr5 134871851 134871852``

.. include:: _includes/common-input-formats.rst

.. include:: _includes/common-submission-info.rst


Output
------

Sequence classes
~~~~~~~~~~~~~~~~~~~~~~~~~

The Sei framework predicts 40 sequence class scores, covering a wide range of regulatory activities such as cell-type-specific enhancers and promoters, as well as 21,907 chromatin profiles for any DNA sequence. Sequence class-level variant effects are computed by comparing the predictions for the reference and the alternative alleles. A positive score indicates an increase in sequence class activity by the alternative allele and vice versa. Sequence class-level scores are computed by projecting the 21,907 chromatin profile predictions for the sequence to the unit vector that represents each sequence class.A full description of how Sei sequence scores are computed can be found in the `Sei paper (2022) <https://www.nature.com/articles/s41588-022-01102-2>`_.

To help interpretation, we grouped sequence classes into groups including P (Promoter), E (Enhancer), CTCF (CTCF-cohesin binding), TF (TF binding), PC (Polycomb-repressed), HET (Heterochromatin), TN (Transcription), and L (Low Signal) sequence classes. Please refer to our manuscript for a more detailed description of the sequence classes.

Note: sequence class predictions are only available for vcf inputs.

::

  | Sequence class label |               Sequence class name | Rank by size | Group |
  |---------------------:|----------------------------------:|-------------:|------:|
  |                 PC1  |       Polycomb / Heterochromatin  |            0 |   PC  |
  |                  L1  |                       Low signal  |            1 |    L  |
  |                 TN1  |                    Transcription  |            2 |   TN  |
  |                 TN2  |                    Transcription  |            3 |   TN  |
  |                  L2  |                       Low signal  |            4 |    L  |
  |                  E1  |                        Stem cell  |            5 |    E  |
  |                  E2  |                     Multi-tissue  |            6 |    E  |
  |                  E3  |               Brain / Melanocyte  |            7 |    E  |
  |                  L3  |                       Low signal  |            8 |    L  |
  |                  E4  |                     Multi-tissue  |            9 |    E  |
  |                 TF1  |                    NANOG / FOXA1  |           10 |   TF  |
  |                 HET1 |                  Heterochromatin  |           11 |  HET  |
  |                  E5  |                      B-cell-like  |           12 |    E  |
  |                  E6  |                  Weak epithelial  |           13 |    E  |
  |                 TF2  |                            CEBPB  |           14 |   TF  |
  |                 PC2  |                    Weak Polycomb  |           15 |   PC  |
  |                  E7  |            Monocyte / Macrophage  |           16 |    E  |
  |                  E8  |                Weak multi-tissue  |           17 |    E  |
  |                  L4  |                       Low signal  |           18 |    L  |
  |                 TF3  |                FOXA1 / AR / ESR1  |           19 |   TF  |
  |                 PC3  |                         Polycomb  |           20 |   PC  |
  |                 TN3  |                    Transcription  |           21 |   TN  |
  |                  L5  |                       Low signal  |           22 |    L  |
  |                 HET2 |                  Heterochromatin  |           23 |  HET  |
  |                  L6  |                       Low signal  |           24 |    L  |
  |                   P  |                         Promoter  |           25 |    P  |
  |                  E9  |                Liver / Intestine  |           26 |    E  |
  |                 CTCF |                     CTCF-Cohesin  |           27 |  CTCF |
  |                 TN4  |                    Transcription  |           28 |   TN  |
  |                 HET3 |                  Heterochromatin  |           29 |  HET  |
  |                 E10  |                            Brain  |           30 |    E  |
  |                 TF4  |                             OTX2  |           31 |   TF  |
  |                 HET4 |                  Heterochromatin  |           32 |  HET  |
  |                  L7  |                       Low signal  |           33 |    L  |
  |                 PC4  | Polycomb / Bivalent stem cell Enh |           34 |   PC  |
  |                 HET5 |                       Centromere  |           35 |  HET  |
  |                 E11  |                           T-cell  |           36 |    E  |
  |                 TF5  |                               AR  |           37 |   TF  |
  |                 E12  |                Erythroblast-like  |           38 |    E  |
  |                 HET6 |                       Centromere  |           39 |   HET |



Regulatory feature scores
~~~~~~~~~~~~~~~~~~~~~~~~~
* **diffs**: The difference between the the predicted probability of the reference allele and the alternative allele for a regulatory feature (:math:`p_{alt} -p_{ref}`).
