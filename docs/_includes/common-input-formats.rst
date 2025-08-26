We support three types of input: VCF, FASTA, BED. If you want to predict effects of noncoding variants, use VCF format input. If you want to predict chromatin feature probabilities for DNA sequences, use FASTA format. If you want to specify sequences from the human reference genome, you can use BED format. See below for a quick introduction:

**VCF format** is used for specifying a genomic variant. A minimal example is ``chr1 109817590 - G T`` (if you want to copy this text as input, you will need to change spaces to tabs). The five columns are chromosome, position, name, reference allele, and alternative allele.

**FASTA format** input should include sequences of |bp_length|\ bp length each. If a sequence is different from |bp_length|\ bp:

* **Note**: The prediction is for the center base of the input sequence
* **Longer sequences**: Only the center |bp_length|\ bp will be used
* **Shorter sequences**: Sequences shorter than |bp_length|\ bp will be padded with 'N' bases evenly on both sides

  - **Important**: We do not recommend using FASTA input smaller than |bp_length|\ bp unless it is very close (only a few bp off)
  - **Note**: This padding behavior is not recommended. N's were extremely rare in training data (only appearing in assembly gaps), and the model has not been evaluated with artificially padded sequences
  - **Strong recommendation**: Always provide sequences of exactly |bp_length|\ bp by including genomic flanking sequences

**BED format** provides another way to specify sequences in human reference genome. A minimal example is |bed_example|. The three columns are chromosome, start position, and end position.

**Important BED Format Notes:**

* **Coordinate System**: BED format uses 0-indexed start positions and 1-indexed end positions (half-open intervals). This is different from VCF format which uses 1-indexed positions.

  - For equal start/end coordinates: interpreted as single position analysis (e.g., chr1:10000-10000 → center at position 10000)
  - For odd-length intervals: the center is unambiguous (e.g., chr1:100-103 has center at position 101)
  - For even-length intervals: we use the left-center position (floor division)

* **Sequence Extraction**: As stated in the `Selene SDK documentation <https://selene.flatironinstitute.org/master/predict.html#selene_sdk.predict.model_predict.ModelPredict.get_predictions_for_bed_file>`_: *"The coordinates specified in each row are only used to find the center position for the resulting sequence-- regions returned will have the length expected by the model."*

  - **Model-specific lengths**: Sequence length varies by model (Seqweaver: 1000bp, Beluga: 2000bp, Sei: 4096bp)

    + Example: 5000bp interval chr1:10000-15000 with Beluga model → only center 2000bp (chr1:11500-13500) analyzed
    + Consider using multiple smaller intervals if you need analysis of the entire large region
