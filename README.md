## RNA-seq workflow

**Note: This is work in development, and is provided "as-is", without guarantees of correctness or optimality. Please use responsibly.**

This RNA-seq workflow consists of a `Makefile` and a set of R scripts to perform quality control, preprocessing and differential expression analysis of RNA-seq data. The output can be combined with the [`iResViewer`](https://github.com/csoneson/iResViewer) R package to generate a shiny application for browsing and sharing the results.

To use the RNA-seq workflow on your own data, you need to first follow the steps below to set up necessary variables and files:

- Put the gzipped fastq files in the `FASTQ` directory. The `Makefile` assumes that these files are named according to the pattern `<sample-name>.fastq.gz` (or `<sample-name>_R1.fastq.gz` and `<sample-name>_R2.fastq.gz` for paired-end data), if this is not the case you need to rename the files or modify the `Makefile` accordingly.
-  Create a metadata text file. This file should have at least one column, named `ID`, which contains all the values of `<sample-name>` from the fastq files. In addition, any number of columns can be included and used later in the analysis. Include the path to this file in the `Makefile` at the requested line.
-  Modify the `Makefile` by adding the list of `<sample-name>` to either the `PEsamples` or `SEsamples` variable, depending on whether you have paired-end or single-end data. Leave the other variable empty (but don't remove it).
-  Modify the `Makefile` by adding the readlength for your RNA-seq reads.

- Modify the `Makefile` to add paths to reference files. Note that you will also have to add desired paths for indexes etc that will be generated by the workflow. The following reference files are used in the workflow, and can be downloaded for your organism e.g. from [Ensembl](https://www.ensembl.org/info/data/ftp/index.html):
	- Genome fasta file
	- Corresponding GTF file
	- cDNA fasta file
	- ncRNA fasta file

- Modify the `Makefile` to add paths to software binaries. The following software is used by the workflow:
	- [R](https://www.r-project.org/)
	- [Salmon](https://combine-lab.github.io/salmon/)
	- [FastQC](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/)
	- [TrimGalore!](https://www.bioinformatics.babraham.ac.uk/projects/trim_galore/)
	- [Cutadapt](http://cutadapt.readthedocs.io/en/stable/guide.html)
	- [STAR](https://github.com/alexdobin/STAR)
	- [samtools](http://www.htslib.org/)
	- [MultiQC](http://multiqc.info/)
	- [bedtools](http://bedtools.readthedocs.io/en/latest/)
	- bedGraphToBigWig (select your operating system from [this page](http://hgdownload.soe.ucsc.edu/admin/exe/) and download the executable)

- Modify `scripts/run_dge_edgeR.R` to perform the proper differential expression analysis. As a minimum, define the design and the contrast(s) you would like to use. If you make additional modifications, make sure that the output of the script follows the requirements outlined in `scripts/run_dge_edgeR.R`. 