We support three types of input: VCF, FASTA, BED. If you want to predict effects of noncoding variants, use VCF format input. If you want to predict chromatin feature probabilities for DNA sequences, use FASTA format. If you want to predict RBP interaction probabilities for transcript sequences, you can also use FASTA format. If you want to specify sequences from the human reference genome (GRCh37/hg19), you can use BED format. See below for a quick introduction:

**VCF format** is used for specifying a genomic variant. A minimal example is ``chr1 109817590 - G T`` (if you want to copy cover this text as input, you will need to change spaces to tabs). The five columns are chromosome, position, name, reference allele, and alternative allele. Currently, the genome position needs to be in GRCh37/hg19.

**FASTA format** input should include sequences of |bp_length|\ bp length each. If a sequence is different from |bp_length|\ bp:

* **Note**: The prediction is for the center base of the input sequence
* **Longer sequences**: Only the center |bp_length|\ bp will be used
* **Shorter sequences**: Sequences shorter than |bp_length|\ bp will be padded with 'N' bases evenly on both sides

  - **Important**: We do not recommend using FASTA input smaller than |bp_length|\ bp unless it is very close (only a few bp off)
  - **Note**: This padding behavior is not recommended. N's were extremely rare in training data (only appearing in assembly gaps), and the model has not been evaluated with artificially padded sequences
  - **Strong recommendation**: Always provide sequences of exactly |bp_length|\ bp by including genomic flanking sequences

**BED format** provides another way to specify sequences in human reference genome (hg19). The BED input should specify |bp_length|\ bp-length regions. A minimal example is |bed_example|. The three columns are chromosome, start position, and end position.
