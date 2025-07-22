=====================
Overview
=====================

HumanBase is a resource for biological researchers interested in data-driven predictions of gene expression, function, regulation, and interactions in human, particularly in the context of specific cell types/tissues and human disease. A brief summary of some of the key tools included in HumanBase is below; for more detail on each tool, see the corresponding documentation page.

This resource is not merely a public database of primary genomics data or biological literature. The data-driven integrative analyses (i.e. algorithms that “learn” from large genomic data collections) presented in HumanBase are especially powerful because they separate signal from noise in large biological data collections to reach beyond “existing biological knowledge” represented in the biological literature to identify novel associations that are not biased toward well-studied areas of biomedical research. Carefully designed algorithms can drive the development of experimentally testable hypotheses. Thus, HumanBase is a resource for biomedical researchers to incorporate into their research workflows, which they can use to interpret their experimental results and generate hypotheses for experimental follow-up.

HumanBase is actively developed by the `Genomics group <https://www.simonsfoundation.org/flatiron-institute/simons-center-for-data-analysis/genomics/>`_ at the  `Flatiron Institute <https://www.simonsfoundation.org/flatiron-institute/>`_.

Functional gene networks
------------------------

In order to leverage the vast collections of raw, noisy genomic data, they must be integrated, summarized, and presented in a biologically informative manner. We provide a means of mining tens of thousands of whole-genome experiments by way of functional interaction networks. Each interaction network represents a body of data, probabilistically weighted and integrated, focused on a particular biological question. These questions can include, for example, the function of a gene, the relationship between two pathways, or the processes disrupted in a genetic disorder (see `Huttenhower et al., 2009 <https://genome.cshlp.org/content/19/6/1093.long>`_).

:doc:`Tissue-specific Networks <tissue-networks>`
-------------------------------------
HumanBase builds genome-scale functional maps of human tissues by integrating a collection of data sets covering thousands of experiments contained in more than 14,000 distinct publications. We automatically assess each data set for its relevance to each of 144 tissue- and cell lineage–specific functional contexts. The resulting functional gene maps provide a detailed portrait of protein function and interactions in specific human tissues and cell lineages ranging from B lymphocytes to the renal glomerulus and the whole brain. This approach allows HumanBase to profile the specialized function of genes in a high-throughput manner, even in tissues and cell lineages for which no or few tissue-specific data exist (`Greene et al., 2015 <https://www.nature.com/articles/ng.3259>`_).

:doc:`Functional Module Detection <modules>`
---------------------------
HumanBase applies community detection to find cohesive gene clusters from a provided gene list and a selected relevant tissue. Genes within a cluster share local network neighborhoods and together form a cohesive, specific functional module. Module detection enables systematic association of genes - even functionally uncharacterized genes - to specific processes and phenotypes represented in the detected modules. Functional modules are identified with tissue-specific networks, which predict gene interactions from massive data collections. Thus the discovered modules potentially capture higher-order tissue-specific function (`Krishnan et al. 2016 <https://www.nature.com/articles/nn.4353>`_).

:doc:`NetWAS <netwas>`
-------------------------------
Tissue-specific networks provide a new means to generate hypotheses related to the molecular basis of human disease. In NetWAS, the statistical associations from a standard GWAS guide the analysis of functional networks. NetWAS, in conjunction with tissue-specific networks, effectively reprioritizes statistical associations from GWAS to identify disease-associated genes. This reprioritization method is driven by GWAS discovery and does not depend on prior disease knowledge (`Greene et al., 2015 <https://www.nature.com/articles/ng.3259>`_).

:doc:`Sei <sei>` and :doc:`Beluga (DeepSEA) <beluga>`
--------------------------------
Beluga (a `2019 update <https://www.nature.com/articles/s41588-018-0160-6>`_ to the `DeepSEA framework <https://www.nature.com/articles/nmeth.3547>`_) is a deep learning-based algorithmic framework for predicting the chromatin effects of sequence alterations with single nucleotide sensitivity. Beluga can accurately predict the epigenetic state of a sequence, including transcription factors binding, DNase I sensitivities and histone marks in multiple cell types, and further utilize this capability to predict the chromatin effects of sequence variants and prioritize regulatory variants.

Sei (`Chen et al., 2022 <https://www.nature.com/articles/s41588-022-01102-2>`_) provides a global map from any sequence to regulatory activities, as represented by 40 sequence classes by integrating predictions for 21,907 chromatin profiles.

:doc:`In-silico mutagenesis <in-silico-mutagenesis>`
---------------------
HumanBase includes an in silico mutagenesis tool to view the predicted chromatin impact of all possible variants in a given genomic region.


:doc:`Seqweaver <seqweaver>`
-----------------------------------------------
Seqweaver (`Park et al., 2021 <https://www.nature.com/articles/s41588-020-00761-3>`_) is a deep learning framework designed to predict how genetic variants affect post-transcriptional RNA-binding protein (RBP) interactions. The model can predict the impact of genetic variants (including variants never seen in genomic databases) at single-nucleotide resolution.


:doc:`ExPecto <expecto>` and :doc:`ExPectoSC <clever>`
----------------------------------------------------------------------------
ExPecto (`Zhou et al. 2018 <https://www.nature.com/articles/s41588-018-0160-6>`_) and ExPectoSC (`Sokolova et al. 2022 <https://www.cell.com/cell-reports-methods/fulltext/S2667-2375(23)00224-2>`_) make highly accurate cell-type and tissue-specific predictions of gene expression solely from DNA sequence. The cell and tissue-specific impact of gene transcriptional dysregulation can be systematically probed ‘in silico’, at a scale not yet possible experimentally. Both models leverage deep learning-based sequence models trained on chromatin profiling data, and integrated with spatial transformation and regularized linear models. ExPecto is trained with bulk RNA sequencing data and ExPectoSC leverages single cell sequencing experiments that profile all cell types in primary human tissues.

Citations
---------
Greene CS, Krishnan A, Wong AK, Ricciotti E, Zelaya RA, Himmelstein DS, Zhang R, Hartmann BM, Zaslavsky E, Sealfon SC, Chasman DI, FitzGerald GA, Dolinski K, Grosser T, Troyanskaya OG. (2015). `Understanding multicellular function and disease with human tissue-specific networks <https://www.nature.com/articles/ng.3259>`_. Nature Genetics. 10.1038/ng.3259w.

Krishnan A*, Zhang R*, Yao V, Theesfeld CL, Wong AK, Tadych A, Volfovsky N, Packer A, Lash A, Troyanskaya OG.(2016) `Genome-wide prediction and functional characterization of the genetic basis of autism spectrum disorder <https://www.nature.com/articles/nn.4353>_`. Nature Neuroscience.

Zhou J, Theesfeld CL, Yao K, Chen KM, Wong AK, and Troyanskaya OG. (2018) `Deep learning sequence-based ab initio prediction of variant effects on expression and disease risk <https://www.nature.com/articles/s41588-018-0160-6>`_, Nature Genetics.

Zhou J, Troyanskaya OG. (2015). `Predicting the Effects of Noncoding Variants with Deep learning-based Sequence Model <<https://www.nature.com/articles/nmeth.3547>`_. Nature Methods.

Chen KM, Wong AK, Troyanskaya OG, Zhou J. (2022) `A sequence-based global map of regulatory activity for deciphering human genetics <https://www.nature.com/articles/s41588-022-01102-2>`_. Nature Methods.

Park, C. Y., Zhou, J., Wong, A. K., Chen, K. M., Theesfeld, C. L., Darnell, R. B., & Troyanskaya, O. G. (2021). `Genome-wide landscape of RNA-binding protein target site dysregulation reveals a major impact on psychiatric disorder risk <https://www.nature.com/articles/s41588-020-00761-3>`_. Nature genetics.

Sokolova, K., Theesfeld, C. L., Wong, A. K., Zhang, Z., Dolinski, K., & Troyanskaya, O. G. (2023). `Atlas of primary cell-type-specific sequence models of gene expression and variant effects <https://www.cell.com/cell-reports-methods/fulltext/S2667-2375(23)00224-2>`_. Cell Reports Methods.
