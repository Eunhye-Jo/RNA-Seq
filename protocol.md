# RNA-Seq analysis for bacterial gene expression

This is updated at 2021.10.14

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

[STAR](https://github.com/alexdobin/STAR) installation and Compile under Mac OS X

	wget https://github.com/alexdobin/STAR/archive/2.7.9a.tar.gz
	tar -xzf 2.7.9a.tar.gz
	cd STAR-2.7.9a
	cp /Volumes/T7/SSA/tools/STAR-2.7.9a/bin/MacOSX_x86_64/STAR /usr/local/bin/


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

Trimmomatic 실행 	

	java -jar /Volumes/T7/SSA/tools/Trimmomatic-0.39/trimmomatic-0.39.jar PE -phred33 KS1039_3h_0_1_1.fastq.gz KS1039_3h_0_1_2.fastq.gz /Volumes/T7/SSA/data/trimmed/SSA_3h_0_1_f_trim.fastq.gz /Volumes/T7/SSA/data/trimmed/SSA_3h_0_1_f_unpaired.fastq.gz /Volumes/T7/SSA/data/trimmed/SSA_3h_0_1_r_trim.fastq.gz /Volumes/T7/SSA/data/trimmed/SSA_3h_0_1_r_unpaired.fastq.gz ILLUMINACLIP:/Volumes/T7/SSA/tools/Trimmomatic-0.39/adapters/TruSeq3-PE-2.fa:2:30:10 SLIDINGWINDOW:4:15 LEADING:10 TRAILING:10 MINLEN:50
	

* Remove adapters (ILLUMINACLIP:TruSeq3-PE.fa:2:30:10)
* Remove leading low quality or N bases (below quality 3) (LEADING:3)
* Remove trailing low quality or N bases (below quality 3) (TRAILING:3)
* Scan the read with a 4-base wide sliding window, cutting when the average quality per base drops below 15 (SLIDINGWINDOW:4:15)
* Drop reads below the 36 bases long (MINLEN:36)

result (4분 소요)

	Multiple cores found: Using 4 threads
	Using PrefixPair: 'TACACTCTTTCCCTACACGACGCTCTTCCGATCT' and 'GTGACTGGAGTTCAGACGTGTGCTCTTCCGATCT'
	Using Long Clipping Sequence: 'AGATCGGAAGAGCGTCGTGTAGGGAAAGAGTGTA'
	Using Long Clipping Sequence: 'AGATCGGAAGAGCACACGTCTGAACTCCAGTCAC'
	Using Long Clipping Sequence: 'GTGACTGGAGTTCAGACGTGTGCTCTTCCGATCT'
	Using Long Clipping Sequence: 'TACACTCTTTCCCTACACGACGCTCTTCCGATCT'
	ILLUMINACLIP: Using 1 prefix pairs, 4 forward/reverse sequences, 0 forward only sequences, 0 reverse only sequences
	Input Read Pairs: 11264303 Both Surviving: 10985456 (97.52%) Forward Only Surviving: 244313 (2.17%) Reverse Only Surviving: 24982 (0.22%) Dropped: 9552 (0.08%)
	TrimmomaticPE: Completed successfully




## STAR alignment

Generating genome indexes

	STAR --runMode genomeGenerate --genomeDir /Volumes/T7/SSA/tools/STAR-2.7.9a/bin/MacOSX_x86_64/genome --genomeFastaFiles /Volumes/T7/SSA/ref/GCF_007814115.1_ASM781411v1_genomic.fa --sjdbGTFfile /Volumes/T7/SSA/ref/GCF_007814115.1_ASM781411v1_genomic.gff --sjdbOverhang 100 --runThreadN 2 --genomeSAindexNbases 8
	
	
RSEM index file
