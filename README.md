# Assemble_bedadeti_plastome
Description of method for assembling the plastome for Ensete ventricosum landrace Bedadeti

All steps carried out in dir
```
/data/scratch/mpx469/assemble_bedadeti_plastome
```

All scripts located in the following dir
```
/data/scratch/mpx469/assemble_bedadeti_plastome/scripts
```

#### Download fastq data for Bedadeti
```
# set dir
mkdir /data/scratch/mpx469/assemble_bedadeti_plastome/fastq_dump
cd    /data/scratch/mpx469/assemble_bedadeti_plastome/fastq_dump

# cp script
cp /data/scratch/mpx469/assemble_bedadeti_plastome/scripts/script_fastq_dump.sh .

# run fastq dump
qsub script_fastq_dump.sh
```

#### De-interleave fastq data using bbmap/
```
# set dir
mkdir /data/scratch/mpx469/assemble_bedadeti_plastome/bbmap
cd    /data/scratch/mpx469/assemble_bedadeti_plastome/bbmap

# cp script
cp /data/scratch/mpx469/assemble_bedadeti_plastome/scripts/script_bbmap.sh .

# run fastq dump
qsub script_bbmap.sh 
```

#### Assemble plastome using novoplasty
```
# set dir
mkdir /data/scratch/mpx469/assemble_bedadeti_plastome/novoplasty
cd    /data/scratch/mpx469/assemble_bedadeti_plastome/novoplasty

# JX517741.1.fasta downloaded from https://www.ncbi.nlm.nih.gov/nuccore/JX517741.1

# cp config file for novoplasty
# remove description at bottom of config
head -n 36 /data/home/mpx469/software/NOVOPlasty/config.txt > config_Bedadeti1.txt

# edit config file
sed -i -e 's/= Test/= Bedadeti1/g' \
    -e 's/= mito/= chloro/g' \
	-e 's/= 12000-22000/= 120000-200000/g' \
	-e 's:/path/to/reads/reads_1.fastq:/data/scratch/mpx469/assemble_bedadeti_plastome/bbmap/Bedadeti1_r1.fastq:g' \
	-e 's:/path/to/reads/reads_2.fastq:/data/scratch/mpx469/assemble_bedadeti_plastome/bbmap/Bedadeti1_r2.fastq:g' \
	-e 's///g' \
	-e 's:/path/to/seed_file/Seed.fasta:JX517741.1.fasta:g' \
	-e 's:/path/to/reference_file/reference.fasta (optional)::g' \
	-e 's:/path/to/chloroplast_file/chloroplast.fasta (only for "mito_plant" option)::g'  \
	-e 's/Max memory            = /Max memory            = 20/g' config_Bedadeti1.txt

# cp script
cp /data/scratch/mpx469/assemble_bedadeti_plastome/scripts/script_novoplasty.sh .

# run novoplasty
qsub script_novoplasty.sh
```


#### Assemble plastome using fast plast
```
# set dir
mkdir /data/scratch/mpx469/assemble_bedadeti_plastome/fast_plast
cd    /data/scratch/mpx469/assemble_bedadeti_plastome/fast_plast

# cp script
cp /data/scratch/mpx469/assemble_bedadeti_plastome/scripts/script_fast_plast.sh .

# run fast plast
qsub script_fast_plast.sh
```


#### Annotate and select correct orientation using chlorobox

Annotated manually using chlorobox 
https://chlorobox.mpimp-golm.mpg.de/geseq.html

```
# set dir
mkdir /data/scratch/mpx469/assemble_bedadeti_plastome/chlorobox
cd    /data/scratch/mpx469/assemble_bedadeti_plastome/chlorobox 

# cp plastomes to dir
cp /data/scratch/mpx469/assemble_bedadeti_plastome/novoplasty/Option_* .

# HF677508.1.gb downloaded from https://www.ncbi.nlm.nih.gov/nuccore/HF677508

Manual checked orientation
Option 1 is in the correct orientation with respect to SSC of HF677508.1.gb
```
