=====================
In Silico Mutagenesis
=====================

Introduction
------------

Perform "In silico saturated mutagenesis" (ISM) analysis to discover informative sequence features within any sequence. Specifically, it performs computational mutation scanning to assess the effect of mutating every base of the input sequence on chromatin feature predictions. This method for context-specific sequence feature extraction takes advantage of DeepSEA's ability to utilize flanking context sequences information.

Note that ISM only accepts a sequence (FASTA file) as input. The input FASTA file should be 2000 base pairs long.

The chromatin impact prediction is performed using the :docs:`beluga` model.

ISM outputs effects for each of three possible substitutions of all 2000 bases, across all chromatin features.

Output
------

The effect of a base substitution on a specific chromatin feature prediction was measured by log2 fold change of odds, where P0 represents the probability predicted for the original sequence and P1 represents the probability predicted for the mutated sequence:

.. math::
   \log_2 \left(\frac{P_0}{1 - P_0}\right) - \log_2 \left(\frac{P_1}{1 - P_1}\right)

(See the `DeepSEA paper (2015) <https://www.nature.com/articles/nmeth.3547>`_)