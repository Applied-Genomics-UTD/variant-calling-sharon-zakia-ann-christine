ACC=AF086833
SRR=SRR1553500
R1=${SRR}_1.fastq
R2=${SRR}_2.fastq
TAG="@RG\tID:${SRR}\tSM:${SRR}\tLB:${SRR}"
REF=refs/${ACC}fa
BAM=${SRR}.bam


ref:
	mkdir -p refs

fetch:ref
	efetch -db nuccore -format fasta -id ${ACC} | seqret -filter -sid ${ACC} > ${REF}
	bwa index ${REF}
	samtools faidx ${REF}

fast:fetch
	fastq-dump -X 100000 --split-files ${SRR}

bam:fast
	bwa mem -R ${TAG} ${REF} ${R1} ${R2} | samtools sort > ${BAM}
	samtools index ${BAM}

var: bam
	bcftools mpileup -O v -f ${REF} ${BAM} | bcftools call --ploidy 1 -vm -O v >  variants1.vcf


