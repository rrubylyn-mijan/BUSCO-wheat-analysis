BUSCO 5.7.1 Genome Completeness Guide
======================================

This is a beginner-friendly guide to run BUSCO v5.7.1 (Benchmarking Universal Single-Copy Orthologs)
for genome completeness analysis and visualization.

Created by: Ruby Mijan


STEP 1: Load Conda on HPC
--------------------------
Conda is a tool that helps you install software and manage environments without causing version conflicts.  
It lets you keep your tools organized for each project.

ml miniconda3/24.3.0


STEP 2: Create and Activate a Mamba Environment
-----------------------------------------------
Mamba is a faster version of Conda. It works the same way but installs packages much quicker,  
which is very helpful when setting up tools like BUSCO that have many dependencies.

conda create --prefix /home/YOUR_USERNAME/miniconda_envs/mamba_env -c conda-forge mamba
conda activate /home/YOUR_USERNAME/miniconda_envs/mamba_env


STEP 3: Install BUSCO Environment Using Mamba
---------------------------------------------
mamba create --prefix /home/YOUR_USERNAME/miniconda_envs/busco_env -c conda-forge -c bioconda busco=5.7.1


STEP 4: Initialize and Activate BUSCO
-------------------------------------
eval "$(mamba shell hook --shell bash)"
mamba activate /home/YOUR_USERNAME/miniconda_envs/busco_env
mamba shell init --shell bash --root-prefix=~/.local/share/mamba
source ~/.bashrc
mamba activate /home/YOUR_USERNAME/miniconda_envs/busco_env


STEP 5: Run BUSCO on a Genome
-----------------------------
busco -i /path/to/genome.fasta \
      -l poales_odb10 \
      -o busco_output_genome \
      -m genome


STEP 6: Visualize Results for a Single Genome
---------------------------------------------
mkdir /your_path/BUSCO_summaries_ggplot_genome

scp /your_path/busco_output_genome/run_poales_odb10/short_summary.txt \
    /your_path/BUSCO_summaries_ggplot_genome/

cd /your_path/BUSCO_summaries_ggplot_genome
mv short_summary.txt short_summary.generic.poales_odb10.YourGenome.txt

python3 /home/YOUR_USERNAME/miniconda_envs/busco_env/bin/generate_plot.py \
        -wd /your_path/BUSCO_summaries_ggplot_genome/

# Optional: Transfer to local computer (Windows example)
scp first.last@atlas-login.hpc.msstate.edu:/your_path/BUSCO_summaries_ggplot_genome/busco_figure.png \
    C:\Users\first.last\Documents\BUSCO_figures


STEP 7: Combine and Visualize Multiple Genomes
----------------------------------------------
mkdir /your_path/BUSCO_combined_genomes

# Copy all short summary files
scp /your_path/short_summary.generic.poales_odb10.Genome1.txt \
    /your_path/BUSCO_combined_genomes/

scp /your_path/short_summary.generic.poales_odb10.Genome2.txt \
    /your_path/BUSCO_combined_genomes/

scp /your_path/short_summary.generic.poales_odb10.Genome3.txt \
    /your_path/BUSCO_combined_genomes/

# Generate combined plot
python3 /home/YOUR_USERNAME/miniconda_envs/busco_env/bin/generate_plot.py \
        -wd /your_path/BUSCO_combined_genomes/

# Download to local PC
scp first.last@atlas-login.hpc.msstate.edu:/your_path/BUSCO_combined_genomes/busco_figure.png \
    C:\Users\first.last\Documents\BUSCO_figure


BONUS: Customize Plot Using R
-----------------------------
# Copy R script and summary files
scp /your_path/BUSCO_combined_genomes/busco_figure.R \
    C:\Users\first.last\Documents\BUSCO_figures

scp /your_path/BUSCO_combined_genomes/short_summary.generic.poales_odb10.*.txt \
    C:\Users\first.last\Documents\BUSCO_figures


Example Directory Structure
---------------------------
BUSCO_project/
├── Genome FASTA files
├── BUSCO_combined_genomes/
│   ├── short_summary.generic.poales_odb10.Genome1.txt
│   ├── short_summary.generic.poales_odb10.Genome2.txt
│   ├── short_summary.generic.poales_odb10.Genome3.txt
│   └── busco_figure.png
├── BUSCO_summaries_ggplot_genome/
│   └── short_summary.generic.poales_odb10.YourGenome.txt
└── busco_env/


Maintainer: Ruby Mijan (rrubylyn.mijan@gmail.com)

