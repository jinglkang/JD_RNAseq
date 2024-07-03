# JD_RNAseq
## Metabolic_arrest_Ricca
### Quality control
```bash
# jlkang@hpc2021 Tue Mar 26 14:57:56 /lustre1/g/sbs_schunter/Kang/JD_data/Metabolic_arrest_Ricca/raw_data
module load MultiQC
ll *.gz|perl -alne '($nm)=$F[-1]=~/(.*)\.fastq\.gz/;$nm.=".fq.gz";print "$F[-1]\t$nm";my $nm' > ../data_list.txt
# perl quality_control.pl --input data_list.txt --raw_dir raw_data --trim_dir ~/software/Trimmomatic-0.39 --kraken_lib /lustre1/g/sbs_schunter/Kang/kraken2/library --fastqc ~/software/FastQC/fastqc >quality_control.process
```

```script_QC.cmd
#!/bin/bash
#SBATCH --job-name=QC        # 1. Job name
#SBATCH --mail-type=BEGIN,END,FAIL    # 2. Send email upon events (Options: NONE, BEGIN, END, FAIL, ALL)
#SBATCH --mail-user=jlkang@hku.hk     #    Email address to receive notification
#SBATCH --partition=amd               # 3. Request a partition
#SBATCH --qos=normal                  # 4. Request a QoS
#SBATCH --nodes=1                     #    Request number of node(s)
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=32
#SBATCH --mem-per-cpu=1G
#SBATCH --time=1-10:00:00             # 7. Job execution duration limit day-hour:min:sec
#SBATCH --output=%x_%j.out            # 8. Standard output log as $job_name_$job_id.out
#SBATCH --error=%x_%j.err             #    Standard error log as $job_name_$job_id.err

# print the start time
date
module load MultiQC
perl quality_control.pl --input data_list.txt --raw_dir raw_data --trim_dir ~/software/Trimmomatic-0.39 --kraken_lib /lustre1/g/sbs_schunter/Kang/kraken2/library --fastqc ~/software/FastQC/fastqc
# print the end time
date
```

```bash
# jlkang@hpc2021 Tue Mar 26 16:00:43 /lustre1/g/sbs_schunter/Kang/JD_data/Metabolic_arrest_Ricca
sbatch script_QC.cmd
# Submitted batch job 1595241
```

## Thermal_rates
```bash
# jlkang@hpc2021 Tue Mar 26 23:52:15 /lustre1/g/sbs_schunter/Kang/JD_data/Thermal_rates/Raw_data
ll *.gz|perl -alne '($nm1, $nm2)=$F[-1]=~/(.*)\_(\d+)\.fq\.gz/;$nm="$nm1\_R$nm2.fq.gz";print "$F[-1]\t$nm";my ($nm1, $nm2, $nm)' > ../data_list.txt
module load kraken2/2.1.2
sbatch script_QC.cmd
```
