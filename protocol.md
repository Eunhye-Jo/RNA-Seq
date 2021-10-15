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

	cd ref
	wget https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/007/814/115/GCF_007814115.1_ASM781411v1/GCF_007814115.1_ASM781411v1_genomic.fna.gz
	
[FastQC](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/)에서 설치파일 다운로드
	
	cd ..
	cd tools
	wget https://www.bioinformatics.babraham.ac.uk/projects/fastqc/fastqc_v0.11.9.zip
	unzip fastqc_v0.11.9.zip
	chmod 755 *

[Trimmomatic](http://www.usadellab.org/cms/?page=trimmomatic)에서 설치파일 다운로드

	wget http://www.usadellab.org/cms/uploads/supplementary/Trimmomatic/Trimmomatic-0.39.zip
	unzip Trimmomatic-0.39.zip

[STAR](https://github.com/alexdobin/STAR)설치

	wget https://github.com/alexdobin/STAR/archive/2.7.9a.tar.gz
	tar -xzf 2.7.9a.tar.gz
	cd STAR-2.7.9a

[RSEM](https://github.com/deweylab/RSEM)에서 제공하는 설치는 리눅스 전용이라서 맥 환경에서 설치하는 방법을 [찾아서](https://anaconda.org/bioconda/rsem) 이용함

	conda install -c bioconda rsem
	.
	.
	Proceed?(y/n) y
	


  
## FastQC 부터 해보자

	cd data
	mkdir fastqc
	../tools/FastQC/fastqc -o fastqc/ KS1039_3h_0_1_1.fastq.gz

result (3분 소요)

	Started analysis of KS1039_3h_0_1_1.fastq.gz
	Approx 5% complete for KS1039_3h_0_1_1.fastq.gz
	Approx 10% complete for KS1039_3h_0_1_1.fastq.gz
	Approx 15% complete for KS1039_3h_0_1_1.fastq.gz
	Approx 20% complete for KS1039_3h_0_1_1.fastq.gz
	Approx 25% complete for KS1039_3h_0_1_1.fastq.gz
	Approx 30% complete for KS1039_3h_0_1_1.fastq.gz
	Approx 35% complete for KS1039_3h_0_1_1.fastq.gz
	Approx 40% complete for KS1039_3h_0_1_1.fastq.gz
	Approx 45% complete for KS1039_3h_0_1_1.fastq.gz
	Approx 50% complete for KS1039_3h_0_1_1.fastq.gz
	Approx 55% complete for KS1039_3h_0_1_1.fastq.gz
	Approx 60% complete for KS1039_3h_0_1_1.fastq.gz
	Approx 65% complete for KS1039_3h_0_1_1.fastq.gz
	Approx 70% complete for KS1039_3h_0_1_1.fastq.gz
	Approx 75% complete for KS1039_3h_0_1_1.fastq.gz
	Approx 80% complete for KS1039_3h_0_1_1.fastq.gz
	Approx 85% complete for KS1039_3h_0_1_1.fastq.gz
	Approx 90% complete for KS1039_3h_0_1_1.fastq.gz
	Approx 95% complete for KS1039_3h_0_1_1.fastq.gz
	Analysis complete for KS1039_3h_0_1_1.fastq.gz

이거를 모든 샘플에서 다 진행한 후 output file로 생성된 html을 열어서 sequencing quality check

## Trimmomatic으로 잘라보자

생성된 파일 저장할 폴더 생성

	mkdir trimmed
	

	java -jar /Volumes/T7/SSA/tools/trimmomatic/Trimmomatic-0.39/trimmomatic-0.39.jar PE -phred33 
	KS1039_3h_0_1_1.fastq.gz 
	KS1039_3h_0_1_2.fastq.gz
	SSA_3h_0_1_f_trim.fastq.gz 
	SSA_3h_0_1_f_unpaired.fastq.gz 
	SSA_3h_0_1_r_trim.fastq.gz 
	SSA_3h_0_1_r_unpaired.fastq.gz 
	ILLUMINACLIP:/Volumes/T7/SSA/tools/trimmomatic/Trimmomatic-0.39/adapters/TruSeq3-PE-2.fa:2:30:10 SLIDINGWINDOW:4:15 LEADING:10 TRAILING:10 MINLEN:50
	




