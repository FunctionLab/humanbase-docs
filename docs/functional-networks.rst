Tissue-specific Networks
===========================
In order to leverage the vast collections of raw, noisy genomic data, they must be integrated, summarized, and presented in a biologically informative manner. We provide a means of mining tens of thousands of whole-genome experiments by way of functional interaction networks. Each interaction network represents a body of data, weighted and integrated, focused on a particular tissue, cell, or process context. 

It is important to consider gene relationships within a tissue or cell type as the precise actions of genes are frequently dependent on their context, and human diseases result from the disordered interplay of tissue- and cell lineage–specific processes. These factors combine to make the understanding of tissue-specific gene functions, disease pathophysiology and gene-disease associations particularly challenging.

Tissue-specific network construction is described in the following publication: Greene, C. S., Krishnan, A., Wong, A. K., Ricciotti, E., Zelaya, R. A., Himmelstein, D. S., ... & Troyanskaya, O. G. (2015). `Understanding multicellular function and disease with human tissue-specific networks <https://www.nature.com/articles/ng.3259>`_. Nature Genetics.

Method
---------------------------
Briefly, functional integration relies on the construction of process-specific functional relationship networks. These are interaction networks in which each node represents a gene, each edge a functional relationship, where an edge between two genes is a probability based on experimental evidence relating to those genes. We integrate evidence from many data sets, with each data set weighted in a process-specific manner. 

For GIANT, one naïve Bayesian classifier is trained per biological area of interest (e.g. a tissue, or a specific biological process), using the appropriate gold standard for the biological context in addition to one global process-unaware classifier trained using the complete gold standard. Each classifier consisted of a class node predicting the binary presence or absence of a functional relationship (FR) between two genes and n nodes conditioned on FR, each representing the value of a data set.

Parameter regularization is performed as described in Steck and Jaakkola (2002) using mutual information between data sets to estimate a strength of prior belief for each data set. While a large amount of shared information does not guarantee a redundant data set, since the same subset of information could be shared many times, it provides a valuable quantitative estimate of data set uniqueness.

MAGE constructs networks in two stages.
In stage 1 (representation learning), each dataset is converted into a gene graph with edges derived from coexpression or protein/gene interactions. MAGE trains a masked graph autoencoder that hides a fraction of edges and learns to reconstruct them using information from neighboring genes in the graph. The decoder outputs a reconstruction probability for each gene pair, which serves as dataset-level evidence for functional relatedness.

In stage 2 (context-specific integration), MAGE learns a tissue- or cell-type-specific mapping from dataset-level evidence to a functional relationship probability. This supervised model is trained using a tissue- or cell-type-specific functional gold standard derived from Gene Ontology biological process relationships together with tissue expression patterns. The output is a tissue- or cell-type-specific functional network where each edge weight is the predicted probability that two genes participate in shared biological processes in that context. 

Data integration
---------------------------
GIANT integrates 987 genome-scale data sets encompassing approximately 38,000 conditions from an estimated 14,000 publications including both expression and interaction measurements. To integrate these data, we automatically assess each data set for its relevance to each of 144 tissue- and cell lineage-specific functional contexts. The resulting functional maps provide a detailed portrait of protein function and interactions in specific human tissues and cell lineages ranging from B lymphocytes to the renal glomerulus and the whole brain. This approach allows us to profile the specialized function of genes in a high-throughput manner, even in tissues and cell lineages for which no or few tissue-specific data exist.

MAGE integrates 7,463 genome-scale datasets representing more than 250,000 experiments across multiple data types. These include protein–protein interaction resources, transcription factor binding motif information, perturbation and microRNA target profiles, and large collections of gene expression studies. Each dataset is processed into a graph representation, and the full collection of dataset-level edge evidence is then integrated into 289 tissue and cell-type networks. 

* Gene co-expression: All gene expression data sets are from NCBI's Gene Expression Omnibus (GEO) for GIANT and refine.bio for MAGE. Genes with more than 30% of values missing were removed, and remaining missing values were imputed using ten nearest neighbors. Non-log-transformed data sets were log transformed. Expression measurements were summarized to Entrez identifiers, and duplicate identifiers were merged. The Pearson correlation was calculated for each gene pair, normalized with Fisher's z transform, mean subtracted and divided by the standard deviation. 

* Protein-interaction: Interaction data are collected from BioGRID, IntAct, MINT, and MIPS.

* TF regulation: To estimate shared transcription factor regulation between genes, we collected binding motifs from JASPAR. Genes were scored for the presence of transcription factor binding sites using the MEME software suite. Motif matches were treated as binary scores (present if P < 0.001). The final score for each gene pair was obtained by calculating the Pearson correlation between the motif association vectors for the genes.

* MSigDB perturbations and miRNA: Chemical and genetic perturbation (c2:CGP) and microRNA target (c3:MIR) profiles were downloaded from the Molecular Signatures Database (MSigDB). Each gene pair's score was the sum of shared profiles weighted by the specificity of each profile.


Evidence
---------------------------
For GIANT:
The "evidence" for an edge is measured as the contribution or "influence" of each dataset on the posterior classification probability. Each dataset contribution is calculated as the posterior probability of a functional relationship given only that dataset, minus the prior probablility.

Contribution of dataset D to an edge functional relationship prediction (FR)::

   contribution(D) = P(FR | D) - P(FR)

Note that the contributions will not sum to 1.0, as each contribution is measured separately. Generally, individual gene expression datasets will not contribute much to the posterior probability but cumulatively can make a significant contribution.

For MAGE:
In each tissue- or cell-type-specific MAGE network, an edge between genes *u* and *v* is assigned a single score produced by the stage 2 (context-specific integration) gradient-boosting integration model (XGBoost). Each gene pair is represented by a 7,463-dimensional feature vector (one feature per dataset) derived from the stage 1 (representation learning) masked-edge reconstruction probabilities, and the boosting model maps these features to a predicted score between 0 and 1, where the score represents the probability of a functional relationship in that context.

The final network edge weight is the predicted score:
edge_weight(u, v) ∈ [0, 1]

Higher values indicate a higher predicted probability that the two genes participate in a functional relationship in the selected tissue or cell type.  


Example
---------------------------

IL1B in blood vessel
~~~~~~~~~~~~~~~~~~~~~~~~~
We examined and experimentally verified the tissue-specific molecular response of blood vessel cells to stimulation by IL-1β (IL1B), a pro-inflammatory cytokine. We anticipated that the genes most tightly connected to IL1B in the blood vessel network would be among those responding to IL-1β stimulation in blood vessel cells. We tested this hypothesis by profiling the gene expression of human aortic smooth muscle cells (HASMCs; the predominant cell type in blood vessels) stimulated with IL-1β.

Examination of the genes whose expression was significantly upregulated at 2 h after stimulation showed that 18 of the 20 IL1B network neighbors were among the top 500 most upregulated genes in the experiment (P = 2.07 × 10−23). The blood vessel network was the most accurate GIANT tissue network in predicting this experimental outcome; none of the other 143 GIANT tissue-specific networks or the tissue-naive network performed as well when evaluated by each network's ability to predict the result of IL-1β stimulation on the cells.

Reproducing legacy results
--------------------------

The table of GO Enriched functions on GIANT network pages used a larger set of genes and annotations, including non-human relevant, between February 2024 and May 2026. To reproduce values shown during that window, see :doc:`Reproducing legacy results <reproducibility>`.
