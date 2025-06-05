---
tags:
    - Nanopore
    - DRS
    - Ninetails
---

# Ninetails

---

!!! warning "Work in progress"

    This site is currently under construction, many parts still have an unfinished look or are yet to be written.<br>
    For a full view of currently available pages check out the [tag directory](../../tags.md).<br>
    ~Erik

---
## What is Ninetails?
Ninetails is a R-package that can identify non-A nucleotides (C, G, U) in poly(A)-tails and their position on signal level.
Find the paper [here](https://doi.org/10.1038/s41467-025-57787-6).
Their GitHub page can be found [here](https://github.com/LRB-IIMCB/ninetails).
Eriks JournalClub presentation can be found in the AGDepledge Drive.
<br>

---
## Installation

In principle, the installation guide can be found [here](https://github.com/LRB-IIMCB/ninetails/wiki/2.-Installation), but some additional steps might/might not need to be taken. Because of this it is advised to follow the instructions below first.
Ninetails could theoretically be run on different devices, but the installation proves to be difficult in some cases (although it is quite straight forward, once you know how to). Different options are listed below. Erik recommends using Ninetails via an HPC interactive RStudio session.
<br><br>

<div class="annotate" markdown>

=== "Recommended: MHH-HPC - interactive RStudio [Ubuntu]"
    This works.<br>
    If you only want to use Ninetails (and not install it) you can ignore the RStudio container section (as this has already been done, see `/project/sysviro/R/01_RStudio/`).
    <br><br>
    The HPC has two ways to use an interactive RStudio session, 'Simple' and 'Advanced'. <br><br>
    **Do not try to install Ninetails in a Simple interactive RStudio Session.<br>It will only bring you misery.**
    <br><br>
    Additionally, Erik strongly advises against trying to install the additional requirements manually (as described in the Ninetails Wiki on GitHub) and instead use conda. Using conda should eliminate the need to install any of these additional requirements manually.
    <br><br>

    #### Setting up the RStudio container
    On the HPC, do the following:

    1. Create RStudio Container settings file with the following content (adjustments can be made if needed):
    ``` linenums="1" title="ninetails_RStudio_container.def"
    # Select Container Template
    Bootstrap: docker
    From: rocker/rstudio:4.4

    %post
    # Create Important HPC Directory Environment
    mkdir -p /leinesw
    mkdir -p /software
    mkdir -p /project
    mkdir -p /etc/ssh

    # Install Packages
    apt update && apt install -y python3 python3-pip python3-venv libhdf5-dev zlib1g-dev

    # Install R Modules
    Rscript -e "install.packages('BiocManager')"
    Rscript -e "install.packages('devtools')"
    Rscript -e "BiocManager::install('rhdf5')"
    Rscript -e "devtools::install_github('LRB-IIMCB/ninetails')"

    %labels
    Author: Erik Fuhrmann
    Version: 1.0
    R-Version: 4.4

    ```

    2. Build container
    ``` bash
    apptainer build ninetails_RStudio_container.sif ninetails_RStudio_container.def
    ```

    #### Setting up the conda environment
    ``` bash
    wget https://raw.githubusercontent.com/LRB-IIMCB/ninetails_processing/refs/heads/main/ninetails_docker_build/r-ninetails.yml
    conda env create -f r-ninetails.yml
    ```

    #### Running the interactive RStudio session
    1. Visit the [MHH OnDemand site](https://leineood.mh-hannover.local)
    2. Click on 'CPU - Software' - 'RStudio - Advanced'
    3. Enter '/project/sysviro/R/01_R_Studio/ninetails_RStudio_container.sif' (or an alternative build if you would like to use another) for path
    4. Decide a password
    5. Fill out the specs.
        1. Cores: Ninetails parallelises with cores which makes them the main bottleneck, so more cores = faster. Keep in mind however that the interactive RStudio session runs on the leinecpu_interactive partition. This partition is also used for interactive terminal sessions and doesn't have too many cores in total (96). 16 cores is reasonable.
        2. Memory: Unless you only want to look at a subset of data, 64GB is reasonable. 32GB will probably already run you out of memory. [Note: 64GB might still not be enough]
        3. Job Time: One run will take up to ~5 hours. 12 hours is reasonable.
    6. Click Launch and connect to the session. Type your username and the password you have decided on.
    7. In R:
    ``` R
    # activate conda environment (necessary after every restart)
    Sys.setenv(RETICULATE_CONDA="/project/usr/apps/software/zen2/software/Anaconda3/2022.05/bin/conda")
    reticulate::use_condaenv('r-ninetails')
    ```
    6. Ninetails should now be operational. You can use the build in test data set and this command to verify:
    ``` R
    results <- ninetails::check_tails(
    nanopolish = system.file('extdata',
                            'test_data',
                            'nanopolish_output.tsv',
                            package = 'ninetails'),
    sequencing_summary = system.file('extdata',
                                    'test_data',
                                    'sequencing_summary.txt',
                                    package = 'ninetails'),
    workspace = system.file('extdata',
                            'test_data',
                            'basecalled_fast5',
                            package = 'ninetails'),
    num_cores = 2, # Use number of cores applicable to your device. N-1 recommended.
    basecall_group = 'Basecall_1D_000',
    pass_only=TRUE,
    save_dir = '~')
    ```

=== "Local on own PC - Docker (RStudio server) [Windows]"
    This works. (But it's slower and less convenient than running it on the HPC.) If you want to install this on a MHH-PC you will ned admin permissions (Erik hasn't tried this route).

    1. Download and install docker
    2. Use the docker terminal to get the Ninetails image:
    ```
    docker pull ghcr.io/nemitheasura/ninetails-docker:latest
    ```
    3. You can see your available images with:
    ```
    docker image list
    ```
    This also shows you your images ImageID.
    4. Run: (1)
    ```
    docker run -it -p 8787:8787 -e PASSWORD=123 --rm --mount type=bind.src=</path/to/local/direc/you/want/to/access>,dst=/home/rstudio r-ninetails
    ```
    5. Enter `localhost:8787` in your browser and login with username: rstudio and password:123.
    6. In RStudio, run:
    ``` R
    # necessary after every restart
    reticulate::use_condaenv("r-ninetails")
    ```
    7. Ninetails should now be operational. You can use the build in test data set and this command to verify:
    ``` R
    results <- ninetails::check_tails(
    nanopolish = system.file('extdata',
                            'test_data',
                            'nanopolish_output.tsv',
                            package = 'ninetails'),
    sequencing_summary = system.file('extdata',
                                    'test_data',
                                    'sequencing_summary.txt',
                                    package = 'ninetails'),
    workspace = system.file('extdata',
                            'test_data',
                            'basecalled_fast5',
                            package = 'ninetails'),
    num_cores = 2, # Use number of cores applicable to your device. N-1 recommended.
    basecall_group = 'Basecall_1D_000',
    pass_only=TRUE,
    save_dir = '~')
    ```


=== "Locally PC (MHH or own) in RStudio [Windows]"
    This works. (But it's slower and less convenient than running it on the HPC.) You do *not* need admin permissions to install this on an MHH-PC.

    1.  Install conda
    ``` cmd
    curl https://repo.anaconda.com/archive/Anaconda3-2024.10-1-Windows-x86_64.exe --output .\Downloads\Anaconda3-2024.10-1-Windows-x86_64.exe
    ```
    Run the exe and follow the installing instructions. If you install on an MHH-PC you will need to install for your user only (unless you have admin privileges).
    2. Start the Anaconda Prompt and look for your installation path
    ``` cmd
    where conda
    ```
    Copy that path for step 5.
    3. Build the Ninetails-conda environment
    Get this YAML-file (**not** the same as the available file on GitHub, this one has been modified to fit to Windows):
    ``` yml linenums="1" title="r-ninetails_windows.yml"
    name: r-ninetails
    channels:
    - conda-forge
    - defaults
    dependencies:
    - bzip2=1.0.8
    - ca-certificates=2022.12.7
    - libffi=3.4.2
    - libsqlite=3.40.0
    - openssl=3.0.7
    - pip=22.3.1
    - python=3.8.15
    - setuptools=65.6.3
    - tk=8.6.12
    - wheel=0.38.4
    - zlib=1.2.13
    - pip:
        - absl-py==1.4.0
        - astunparse==1.6.3
        - cachetools==5.2.1
        - certifi==2022.12.7
        - charset-normalizer==3.0.1
        - flatbuffers==23.1.4
        - gast==0.4.0
        - google-auth==2.16.0
        - google-auth-oauthlib==0.4.6
        - google-pasta==0.2.0
        - grpcio==1.51.1
        - h5py==3.7.0
        - idna==3.4
        - importlib-metadata==6.0.0
        - keras==2.11.0
        - libclang==15.0.6.1
        - markdown==3.4.1
        - markupsafe==2.1.1
        - numpy==1.24.1
        - oauthlib==3.2.2
        - opt-einsum==3.3.0
        - packaging==23.0
        - protobuf==3.19.6
        - pyasn1==0.4.8
        - pyasn1-modules==0.2.8
        - requests==2.28.2
        - requests-oauthlib==1.3.1
        - rsa==4.9
        - six==1.16.0
        - tensorboard==2.11.2
        - tensorboard-data-server==0.6.1
        - tensorboard-plugin-wit==1.8.1
        - tensorflow==2.11.0
        - tensorflow-estimator==2.11.0
        - tensorflow-io-gcs-filesystem==0.29.0
        - termcolor==2.2.0
        - typing-extensions==4.4.0
        - urllib3==1.26.14
        - werkzeug==2.2.2
        - wrapt==1.14.1
        - zipp==3.11.0
    ```
    And this command:
    ``` cmd
    conda env create -f r-ninetails_windows.yml
    ```
    4. (Close R, then) Download and install Rtools for your required R version from [here](https://cran.r-project.org/bin/windows/Rtools/).
    5. Setup Ninetails in R in R:
    ``` R
    # Install required packages
    if(!requireNamespace("BiocManager", quietly = TRUE))
        install.packages("BiocManager")
    if(!requireNamespace("devtools", quietly = TRUE))
        install.packages("devtools")
    if(!requireNamespace("reticulate", quietly = TRUE))
        install.packages("reticulate")
    if(!requireNamespace("rhdf5", quietly = TRUE))
        BiocManager::install('rhdf5')
    if(!requireNamespace("rhdf5", quietly = TRUE))
        devtools::install_github('LRB-IIMCB/ninetails')

    # activate conda environment (necessary after every restart)
    Sys.setenv(RETICULATE_CONDA="/path/to/conda.bat")
    reticulate::use_condaenv('r-ninetails')
    ```
    6. Ninetails should now be operational. You can use the build in test data set and this command to verify:
    ``` R
    results <- ninetails::check_tails(
    nanopolish = system.file('extdata',
                            'test_data',
                            'nanopolish_output.tsv',
                            package = 'ninetails'),
    sequencing_summary = system.file('extdata',
                                    'test_data',
                                    'sequencing_summary.txt',
                                    package = 'ninetails'),
    workspace = system.file('extdata',
                            'test_data',
                            'basecalled_fast5',
                            package = 'ninetails'),
    num_cores = 2, # Use number of cores applicable to your device. N-1 recommended.
    basecall_group = 'Basecall_1D_000',
    pass_only=TRUE,
    save_dir = '~')
    ```

=== "MHH-HPC - R [GNU-Linux]"
    Probably doesn't work at the moment (mainly because 'zlib1g-dev' (which seems to be mandatory) is missing in the included extensions for the available R modules), but would be super nice.

</div>
1.  In case that doesn't work try
```
docker run -it -p 8787:8787 -e PASSWORD=123 --rm --mount type=bind.src=</path/to/local/direc/you/want/to/access>,dst=/home/rstudio <ImageID>
```
<br>

---
## Pipeline
The postprocessing section is WIP. Check [here](https://github.com/LRB-IIMCB/ninetails/wiki/5.-Data-postprocessing) for more info.

``` mermaid
---
title: Ninetails data workflow
---
flowchart TD
    A{SQK-RNA002 data<br>.fast5s} ---|basecalling<br>Guppy, pre v.6.3.2,<br>--fast5_out!| B[ ]:::empty;
    subgraph Guppy output;
    C[.fastqs];
    D[output .fast5s];
    E[sequence_summary.txt];
    end;
    B --> C;
    B --> D;
    B --> E;
    C  -->|cat| F[.fastq];
    subgraph reference
    G{.fasta};
    H(.fai);
    end;
    G -->|samtools<br>faidx| H;
    E --- I[ ]:::empty;
    G --- I;
    H -.- I;
    F --- K[ ]:::empty;
    G --- K[ ]:::empty;
    H -.- K[ ]:::empty;
    K ---|minimap2/samtools<br>align, filter, sort, index| L[ ]:::empty;
    subgraph alignment data
    M[.sorted.bam];
    N(.bai);
    end;
    L ---> M;
    L ---> N;
    F --- O[ ]:::empty;
    G --- O[ ]:::empty;
    H -.- O[ ]:::empty;
    J -.- O[ ]:::empty;
    M --- O[ ]:::empty;
    N -.- O[ ]:::empty;
    I -->|nanopolish<br>index| J(.index.*);
    O -->|nanopolish<br>polya| P[polya .tsv];
    D --- Q[ ]:::empty;
    E --- Q[ ]:::empty;
    P --- Q[ ]:::empty;
    Q -->|ninetails<br>check tails| R[output tables];
    R -->|post processing| S[WIP];
    classDef empty width:1px,height:1px,fill:transparent,stroke:none;
```

Associated scripts can be obtained by demand (ask Erik).
