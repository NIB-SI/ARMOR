# ARMOR workflow <img src="img/ARMOR.png" width="200" align="right" />
![snakemake-run](https://github.com/csoneson/ARMOR/workflows/snakemake-run/badge.svg)

**ARMOR** (**A**utomated **R**eproducible **MO**dular **R**NA-seq) is a [Snakemake workflow](https://snakemake.readthedocs.io/en/stable/index.html), aimed at performing a typical RNA-seq workflow in a reproducible, automated, and partially contained manner. It is implemented such that alternative or similar analysis can be added or removed. 

ARMOR consists of a `Snakefile`, a [`conda`](https://conda.io/docs/) environment file (`envs/environment.yaml`) a configuration file (`config.yaml`) and a set of `R` scripts, to perform quality control, preprocessing and differential expression analysis of RNA-seq data. The output can be combined with the [`iSEE`](https://bioconductor.org/packages/iSEE/) `R` package to generate a `shiny` application for browsing and sharing the results.  

By default, the pipeline performs all the steps shown in the [diagram](img/dag_nice3.png) below. However, you can turn off any combination of the light-colored steps (e.g `STAR` alignment or `DRIMSeq` analysis) in the `config.yaml` file. 

*Advanced use*: If you prefer other software to run one of the outlined steps (e.g. `DESeq2` over `edgeR`, or `kallisto` over `Salmon`), you can use the software of your preference provided you have your own script(s), and change some lines within the `Snakefile`. If you think your "custom rule" might be of use to a broader audience, let us know by opening an issue.

## Using the ARMOR workflow

Download the repo from GitHub:
```
git clone https://github.com/NIB-SI/ARMOR.git
```
The workflow is defined in `Snakefile` (you probably won't need to edit), and the `config.yaml` file (where you input your data paths and parameters). To run the workflow, the command `snakemake` is run in the directory with these files. 

To install the Snakefile dependencies (snakemake and pandas) use:
```
cd ARMOR
conda env create  -n ARMOR -f envs/armor_root.yaml
```

The Snakefile cotains a number of *rules* (workflow steps) that call applications. These applications are managed in conda enviromnents: one for shell applications (defined in ./envs/environment_shell.yaml) and one for R (defined in ./envs/environment_R.yaml). Each new `snakemake --use-conda` call in a new working directory will recreate these environments. To prevent this waste of resources (and installation time), use a common directory for the conda environments with the `--conda-prefix` each time (e.g. /home/armor-envs/ ). This way, only when the environment yaml files are edited will the environments need to be recreated. 

To run the example dataset use:
```
conda activate ARMOR
snakemake --use-conda -j<num cores>
```

When complete run `conda deactivate`to exit the ARMOR environment. 

### Command summary:
```
git clone https://github.com/NIB-SI/ARMOR.git
cd ARMOR
conda env create  -n ARMOR -f envs/armor_root.yaml
conda activate ARMOR
snakemake --use-conda -j<num cores>
```

To use the ARMOR workflow on your own data, follow the steps outlined in the [wiki](https://github.com/csoneson/ARMOR/wiki).

## Workflow graph
![DAG](img/dag_nice5.png)  
Blue circles are rules run in `R`, orange circles from software called as shell commands. Dashed lines and light-colored circles are optional rules, controlled in `config.yaml`

## Contributors
Current contributors include:

- [Ruizhu Huang](https://github.com/fionarhuang)
- [Katharina Hembach](https://github.com/khembach)
- [Stephany Orjuela](https://github.com/sorjuela)
- [Mark D. Robinson](https://github.com/markrobinsonuzh)
- [Charlotte Soneson](https://github.com/csoneson)
