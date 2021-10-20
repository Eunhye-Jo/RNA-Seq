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
	gunzip GCF_007814115.1_ASM781411v1_genomic.fna.gz
	wget https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/007/814/115/GCF_007814115.1_ASM781411v1/GCF_007814115.1_ASM781411v1_genomic.gtf.gz
	gunzip GCF_007814115.1_ASM781411v1_genomic.gtf.gz
	
	
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

	mkdir /Volumes/T7/SSA/ref/genomeIndex
	cp GCF_007814115.1_ASM781411v1_cds_from_genomic.fna GCF_007814115.1_ASM781411v1_cds_from_genomic.fa
	STAR --runMode genomeGenerate --genomeDir /Volumes/T7/SSA/ref/GenomeIndex --genomeFastaFiles /Volumes/T7/SSA/ref/GCF_007814115.1_ASM781411v1_genomic.fna --sjdbGTFfile /Volumes/T7/SSA/ref/GCF_007814115.1_ASM781411v1_genomic.gtf --sjdbOverhang 100 --runThreadN 2 --genomeSAindexNbases 8
	
genome 폴더에 index 파일들이 생성되어있음

	STAR version: 2.7.9a   compiled:  :/Users/cshl/data/STAR/STAR/source
	Oct 15 22:35:44 ..... started STAR run
	Oct 15 22:35:44 ... starting to generate Genome files
	Oct 15 22:35:44 ..... processing annotations GTF
	Oct 15 22:35:44 ... starting to sort Suffix Array. This may take a long time...
	Oct 15 22:35:44 ... sorting Suffix Array chunks and saving them to disk...
	Oct 15 22:35:46 ... loading chunks from disk, packing SA...
	Oct 15 22:35:46 ... finished generating suffix array
	Oct 15 22:35:46 ... generating Suffix Array index
	Oct 15 22:35:46 ... completed Suffix Array index
	Oct 15 22:35:46 ... writing Genome to disk ...
	Oct 15 22:35:46 ... writing Suffix Array to disk ...
	Oct 15 22:35:46 ... writing SAindex to disk
	Oct 15 22:35:46 ..... finished successfully


Generating RSEM index file


	cd ref
	mkdir RsemIndex
	rsem-prepare-reference --gtf GCF_007814115.1_ASM781411v1_genomic.gtf GCF_007814115.1_ASM781411v1_genomic.fna RsemIndex
	
gtf file과 fna file이 같이 있는 폴더 위치에서 실행할 것
result (실행과 동시에 끝남)

	rsem-extract-reference-transcripts RsemIndex 0 GCF_007814115.1_ASM781411v1_genomic.gtf None 0 GCF_007814115.1_ASM781411v1_genomic.fna
	Parsing gtf File is done!
	GCF_007814115.1_ASM781411v1_genomic.fna is processed!
	83 transcripts are extracted.
	Extracting sequences is done!
	Group File is generated!
	Transcript Information File is generated!
	Chromosome List File is generated!
	Extracted Sequences File is generated!

	rsem-preref RsemIndex.transcripts.fa 1 RsemIndex
	Refs.makeRefs finished!
	Refs.saveRefs finished!
	RsemIndex.idx.fa is generated!
	RsemIndex.n2g.idx.fa is generated!

Alignment using STAR

	STAR --genomeDir /Volumes/T7/SSA/ref/GenomeIndex/ --readFilesIn <(gunzip -c ../trimmed/SSA_3h_0_1_f_trim.fastq.gz ../trimmed/SSA_3h_0_1_r_trim.fastq.gz) --outFileNamePrefix SSA_3h_0%_1. --runThreadN 6 --outSAMtype BAM SortedByCoordinate --quantMode TranscriptomeSAM --twopassMode Basic > message.txt
	
7부 걸려서 끝났는데 error......

	BAMoutput.cpp:27:BAMoutput: exiting because of *OUTPUT FILE* error: could not create output file SSA_3h_0%_1._STARtmp//BAMsort/4/47
	SOLUTION: check that the path exists and you have write permission for this file. Also check ulimit -n and increase it to allow more open files.

	Oct 19 11:12:44 ...... FATAL ERROR, exiting

에러해결하려고 찾아보다가 --outSAMtype BAM SortedByCoordinate 제거하고 다시 시도해봄

	STAR --genomeDir /Volumes/T7/SSA/ref/GenomeIndex/ --readFilesIn <(gunzip -c ../trimmed/SSA_3h_0_1_f_trim.fastq.gz ../trimmed/SSA_3h_0_1_r_trim.fastq.gz) --outFileNamePrefix SSA_3h_0%_1. --runThreadN 6 --quantMode TranscriptomeSAM --twopassMode Basic > message.txt

7분 소요
cat TestData1.Log.final.out
Uniquely mapped reads % 확인

					 Started job on |	Oct 19 11:29:01
				     Started mapping on |	Oct 19 11:36:11
					    Finished on |	Oct 19 11:36:11
	       Mapping speed, Million of reads per hour |	nan

				  Number of input reads |	0
			      Average input read length |	0
					    UNIQUE READS:
			   Uniquely mapped reads number |	0
				Uniquely mapped reads % |	0.00%
				  Average mapped length |	0.00
			       Number of splices: Total |	0
		    Number of splices: Annotated (sjdb) |	0
			       Number of splices: GT/AG |	0
			       Number of splices: GC/AG |	0
			       Number of splices: AT/AC |	0
		       Number of splices: Non-canonical |	0
			      Mismatch rate per base, % |	nan%
				 Deletion rate per base |	0.00%
				Deletion average length |	0.00
				Insertion rate per base |	0.00%
			       Insertion average length |	0.00
				     MULTI-MAPPING READS:
		Number of reads mapped to multiple loci |	0
		     % of reads mapped to multiple loci |	0.00%
		Number of reads mapped to too many loci |	0
		     % of reads mapped to too many loci |	0.00%
					  UNMAPPED READS:
	  Number of reads unmapped: too many mismatches |	0
	       % of reads unmapped: too many mismatches |	0.00%
		    Number of reads unmapped: too short |	0
			 % of reads unmapped: too short |	0.00%
			Number of reads unmapped: other |	0
			     % of reads unmapped: other |	0.00%
					  CHIMERIC READS:
			       Number of chimeric reads |	0
				    % of chimeric reads |	0.00%

ㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋ


	STAR --genomeDir /Volumes/T7/SSA/ref/GenomeIndex/ --readFilesIn <(gunzip -c ../KS1039_3h_0_1_1.fastq.gz ../KS1039_3h_0_1_2.fastq.gz) --outFileNamePrefix SSA_3h_0%_1. --runThreadN 6 --quantMode TranscriptomeSAM --twopassMode Basic > message3.txt

이것도 안되네








	rsem-calculate-expression --paired-end --bam --estimate-rspd --append-names --no-bam-output -p 6 TestData1.Aligned.toTranscriptome.out.bam /RsemIndex path ./RSEM/TestData1.RSEM
	ls
	cd RSEM
	ls -l
	cat TestData1.RSEM.genes.results | less
	cat TestData1.RSEM.genes.results | cut -f1,6,7 | less
	cat TestData1.RSEM.genes.results | cut -f1,6,7 | sort -k3 -nr | less
	



memo
	awk -F '\t' -v OFS=, '!/^#/ {$1=$1;print}' GCF_007814115.1_ASM781411v1_genomic.gff > GCF_007814115.1_ASM781411v1_genomic_gff.csv;


