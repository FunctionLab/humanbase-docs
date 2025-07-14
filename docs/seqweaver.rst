==========
Seqweaver
==========

Introduction
------------

Seqweaver is a deep learning framework designed to predict how genetic variants affect post-transcriptional RNA-binding protein (RBP) interactions. The model is trained on RBP-RNA interaction data
obtained from CLIP-seq experiments. Seqweaver consists of 232 RBP models spanning 88 distinct RBPs, and can predict the impact of genetic variants (including variants never seen in genomic
databases) at single-nucleotide resolution.

Seqweaver is described in:
Park CY, Zhou J, Wong AK, Chen KM, Theesfeld CL, Darnell RB, Troyanskaya OG. `Genome-wide landscape of RNA-binding protein target site dysregulation reveals a major impact on psychiatric disorder risk <https://www.nature.com/articles/s41588-020-00761-3>`_. Nat Genet. 2021 Feb;53(2):166-173.


Input
-----

We support three types of input: VCF, FASTA, BED. If you want to predict effects of noncoding variants, use VCF format input. If you want to predict RBP interaction probabilities directly from transcript sequences, you can use FASTA format. If you want to specify sequences from the human reference genome, you can use BED format. See below for a quick introduction:

**VCF format** is used for specifying a genomic variant. A minimal example is ``chr8 38120276 [QKI disruption] C A +`` (if you want to copy cover this text as input, you will need to change spaces to tabs). The six columns are chromosome, position, name, reference allele, alternative allele, and strand. Note that strand must be specified for Seqweaver but not for the other deep learning models. 

**FASTA format** input should include sequences of 1000 bp length each. If a sequence is different from 1000 bp:

* **Note**: The prediction is for the center base of the input sequence
* **Longer sequences**: Only the center 1000 bp will be used
* **Shorter sequences**: Sequences shorter than 1000 bp will be padded with 'N' bases evenly on both sides

  - **Important**: We do not recommend using FASTA input smaller than 1000 bp unless it is very close (only a few bp off)
  - **Note**: This padding behavior is not recommended. N's were extremely rare in training data (only appearing in assembly gaps), and the model has not been evaluated with artificially padded sequences
  - **Strong recommendation**: Always provide sequences of exactly 1000 bp by including genomic flanking sequences

**BED format** provides another way to specify sequences in human reference genome (hg19). The BED input should specify 1000 bp-length regions. A minimal example is ``chr1 109817090 109818091 . 0 -``. The columns are chromosome, start position, end position, name, score, and strand.


Large submissions
~~~~~~~~~~~~~~~~~
We recommend using the web server if you have <10,000 variants or sequences. You will experience degraded performance when submitting a larger set of sequences. In those instances, we suggest that you split the set into multiple <10,000 submissions, or run the standalone version on your local machine, or contact our group directly.


Output
------

Variant scores
~~~~~~~~~~~~~~

**Disease impact score:** DIS is calculated by training a logistic regression model that prioritizes likely disease-associated mutations on the basis of the predicted post-transcriptional regulatory effects of these mutations (See `Zhou et. al, 2019 <https://pubmed.ncbi.nlm.nih.gov/31133750/>`_). The predicted DIS probabilities are then converted into DIS e-values, computed based on the empirical distributions of predicted effects for gnomAD variants. The final DIS score is:

.. math::
   -\log_{10}(DIS\ e-value_{feature})

**Mean -log e-value:** For each predicted regulatory feature effect (:math:`abs(p_{alt}-p_{ref})`) of a variant, we calculate a feature e-value based on the empirical distribution of that feature’s effects among gnomAD variants (see Molecular-level biochemical effects prediction: e-value). The MLE score of a variant is

.. math::
   \sum{-\log_{10}(e-value_{feature})}/N

Molecular-level biochemical effects prediction
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**z-score:** A scaled score where the feature diff score (:math:`p_{alt} - p_{ref}`) is divided by the root mean square of the feature diff score across gnomAD variants. Note that this is “sign-preserving”, i.e. a negative z-score indicates that a mutation decreases the probability of a regulatory feature.

**E-value:** E-value is defined as the expected proportion of SNPs with a larger predicted effect. We calculate an 'e-value' based on the empirical distribution of that feature’s effect (:math:`abs(p_{alt}-p_{ref})`) among gnomAD variants. For example, a feature e-value of 0.01 indicates that only 1% of gnomAD variants have a larger predicted effect.

**Probability diffs:** The difference between the the predicted probability of the reference allele and the alternative allele for a regulatory feature (:math:`p_{alt}-p_{ref}`).

See also
--------
* :doc:`sei` - Latest chromatin and regulatory impact model with 4096bp input sequences
* :doc:`beluga` - 2019 DeepSEA model with 2000bp input sequences
