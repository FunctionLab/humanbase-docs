=================
DeepSEA Analysis
=================

Introduction
------------

DeepSEA is a deep learning framework that predicts genomic variant effects with single nucleotide sensitivity on a wide range of regulatory features: transcription factors binding, DNase I hypersensitive sites, and histone marks in multiple human cell types. Importantly, this framework is trained without using any variant data, allowing it to predict the chromatin impact of any variant, including rare or previously unseen ones.


DeepSEA-based Methods
---------------------

The following analysis tools and methods in HumanBase are built upon the DeepSEA framework:

* :doc:`sei` - Extended regulatory and chromatin effects of variants (2021)
* :doc:`beluga` - Chromatin effects of variants (2019)
* :doc:`seqweaver` - Post-transcriptional variant effects
* :doc:`expecto` - Tissue-specific gene expression effects of variants
* :doc:`expectosc` - Cell-type-specific gene expression effects for mutations
* :doc:`in-silico-mutagenesis` - Discover regulatory features of sequences
* :doc:`asdbrowser` - Predicted effects of ASD proband mutations