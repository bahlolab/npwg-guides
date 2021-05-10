# NPWG-example

## 1. Creating the workflow container

### A. The Conda Environment

* Prerequisite: [install conda](https://docs.conda.io/en/latest/miniconda.html)  
* See also: [conda cheatsheet](https://docs.conda.io/projects/conda/en/4.6.0/_downloads/52a95608c49671267e40c689e0bc00ca/conda-cheatsheet.pdf)

1. Search [Anaconda.org](https://anaconda.org/) for required packages. For this example we want 'samtools', 'bwa' and 'bcftools'. We can see these are all available from the 'bioconda'   channel.  
<img src="figs/anaconda_search.png" width="672" />  

2. Create conda 'environment.yml' file in your workflow git repository. This specifies the channel name ('npwg-example'), the conda channels to use and required packages (dependencies). It is recommended to provided precise package versions, and to include 'conda-forge' in channels list. 

    ```YAML
    name: npwg-example
    channels:
      - conda-forge
      - bioconda
    dependencies:
      - bcftools=1.12
      - bwa=0.7.17
      - samtools=1.12
    ```

3. Create the conda environment using the environment file.

    ```sh
    conda env create -f environment.yml
    ```

4. Activate and test the new environment. This can now be used in a Nextflow workflow by setting `process.conda` to the environment prefix (you can find this by running `conda env list`).

    ```sh
    conda activate npwg-example
    **run tests**
    conda deactivate
    ```

5. If additional packages are required, you can add them environment.yml file and then update the conda environment as follows:

    ```sh
    conda env update -f environment.yml
    ```

### B. The Docker Container

* Prerequisites: Github account and [Docker Hub](https://hub.docker.com/) account linked to GitHub Account

1. Create a file named 'Dockerfile' in your workflow git repository. The following is a generic dockerfile that can be used for any conda based container, the only parameter that needs to be set is `ARG NAME` to that of your conda environment.

    ```dockerfile
    FROM biocontainers/biocontainers:latest
    
    LABEL \
      author="Jacob Munro" \
      description="Example container for NPWG" \
      maintainer="Bahlo Lab"
    
    # set the conda env name
    ARG NAME='npwg-example'
    
    # Install the conda environment
    COPY environment.yml /
    RUN conda env create -f /environment.yml && conda clean -a -y
    
    # set path
    ENV PATH="/opt/conda/envs/$NAME/bin:/opt/conda/bin:${PATH}"
    ```
2
    
