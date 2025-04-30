# NCR_Nathalie



## 1. Download data

Create a file containing address of files to download

          mkdir 01_RawReads
          echo "" > 01_RawReads/list_to_download # create empty file and copy paste links to data

Download with wget

          sbatch --partition=pshort_el8 --job-name=wget --time=0-02:00:00 --mem-per-cpu=4G --ntasks=1 --cpus-per-task=1 --output=wget.log --error=wget.err --mail-type=END,FAIL --wrap "cd /data/projects/p495_SinorhizobiumMeliloti/20_Nathalie/01_RawReads; wget -i list_to_download"


## 2. Change filename to discard unique names

      cp 127_N_L1_R1_001_WVXjl0uZX5GP.fastq.gz NCR127_R1.fastq.gz
      cp 127_N_L1_R2_001_YcsxCGcFiFBs.fastq.gz NCR127_R2.fastq.gz
      cp 16_N_L1_R1_001_bF6zPIAMRHym.fastq.gz NCR16_R1.fastq.gz
      cp 16_N_L1_R2_001_Dpr0Mk78eRzm.fastq.gz NCR16_R2.fastq.gz
      .
      .
      .
      mkdir ../02_NewName
      mv N* ../02_NewName

## 3. Extract UMI

    mkdir ../03_UMIExtract
    for FILE in $(ls *_R1.fastq.gz); do echo $FILE; sbatch --partition=pibu_el8 --job-name=$(echo $FILE | cut -d'_' -f1)_UMI --time=0-08:00:00 --mem-per-cpu=12G --ntasks=1 --cpus-per-task=1 --output=UMI_$(echo $FILE | cut -d'_' -f1).out --error=UMI_$(echo $FILE | cut -d'_' -f1).error --mail-type=END,FAIL --wrap "cd /data/projects/p495_SinorhizobiumMeliloti/20_Nathalie/02_NewName ;module load UMI-tools/1.0.1-foss-2021a; umi_tools extract --bc-pattern=NNNNNNNNNNNN --stdin=$FILE --stdout=../03_UMIExtract/$(echo $FILE | cut -d'_' -f1)_extracted_R1.fastq.gz --read2-in=$(echo $FILE | cut -d'_' -f1)_R2.fastq.gz --read2-out=../02_UMIExtract/$(echo $FILE | cut -d'_' -f1)_extracted_R2.fastq.gz"; sleep  1; done
    
