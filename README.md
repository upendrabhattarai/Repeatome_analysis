# Repeatome_analysis
## Installing Repeatmoduler in Canon cluster
Quickest way to do it is through singularity pull of docker container
To do so login to interactive session and go to compute node
```
singularity pull --disable-cache docker://dfam/tetools:latest
```
It will create a tetools_latest.sif file which you can use to execute the commands
To run Repeatmoduler, first lets build a database, we can do this in interactive mode as it finishes in few seconds

```
singularity exec --cleanenv path/to/tetools_latest.sif BuildDatabase -name anisolabis -engine ncbi Anisolabis.renamed.onliner.fa
```
Now run repeatmoduler as below
```
#!/bin/bash
#SBATCH -p serial_requeue
#SBATCH -J repeatmod_Anisolabis
#SBATCH -c 10 
#SBATCH --mem=30gb
#SBATCH --open-mode=append
#SBATCH -t 3-00:00:00
#SBATCH -e %x_%j.err
#SBATCH -o %x_%j.out

singularity exec --cleanenv /n/home12/upendrabhattarai/bin/Dfam-TE/tetools_latest.sif RepeatModeler -engine ncbi -threads 10 -database anisolabis
```
