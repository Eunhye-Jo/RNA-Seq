# RNA-Seq analysis for bacterial gene expression

This is updated at 2021.10.14

Tools for RNA-Seq analysis

* [FastQC](../examples/Notebook/Notebook%20Basics.ipynb)
* [Trimmomatic](http://www.usadellab.org/cms/?page=trimmomatic)
* [STAR](https://github.com/alexdobin/STAR)
* [RSEM](https://github.com/deweylab/RSEM)


Basic steps
1. Downloading annotation, reference genome, raw data
2. Installing Programs (Fastqc, Trimmomatic, STAR, RSEM)
3. Indexing the genome
4. Mapping the data
5. Quantification: Calculating TPM or FPKM

## Downloading annotation, reference genome, raw data
프로젝트 파일 생성

	mkdir SSA

하위에 raw data, reference genome, analysis tools를 저장할 'data', 'ref', 'tools' 폴더 생성

[NCBI](https://www.ncbi.nlm.nih.gov/genome/)에서 bacterial genome sequence download

	cd tools
	wget https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/007/814/115/GCF_007814115.1_ASM781411v1/GCF_007814115.1_ASM781411v1_genomic.fna.gz
	
  
