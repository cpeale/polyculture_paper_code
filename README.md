# school-comp-exps

This repository contains code for the synthetic and semi-synthetic experiments associated with the paper *The Effects of Polyculture on Admissions Decisions*.

The code is separated into two types of experiments: fully synthetic (found in the `synthetic_experiments` folder) and semi-synthetic experiments based on data from the Educational Longitudinal Study of 2002 (found in the `school_data_exps` folder). We provide instructions on viewing and running each type below. 

## Setup for all Experiments
This project uses `uv` for dependency management, but also supports standard `pip` and `conda` workflows. Choose the method that fits your setup.

### Option 1: Using `uv` (recommended)
1) If needed, install `uv` (see https://docs.astral.sh/uv/getting-started/installation/)
2) Run `uv sync` to sync the environment
3) Run the included notebooks using your preferred approach:
    - In VSCode, click "Select Kernel," and choose the environment inside `.venv`.
    - From terminal, run `uv run jupyter lab` (or `uv run jupyter notebook`).

### Option 2: `pip` or `conda`
1) Create a virtual environment using either `pip` or `conda`.
2) Install dependencies: `pip install -r requirements.txt`.
3) Launch Jupyter either via VSCode (select your new environment as the kernel) or from terminal (`jupyter lab`).

## Synthetic Experiments (`synthetic_experiments`)
All code for the synthetic experiments can be found in the `synth.ipynb` notebook found in `synthetic_experiments`. To just plot the pre-generated data, run the notebook but skip the cells marked for data generation. To re-generate data from scratch, also run the the data generation cells. 

## ELS:2002 Semi-Synthetic Experiments (`school_data_exps`)
The code for the experiments on ELS:2002 consists of three main parts. First is data cleaning, which takes the ELS data and pares down students and features to obtain a smaller dataset with no missing entries. 

Second, we model noisy school estimates as the predicted probabilities of machine-learned models trained on subsets of the ELS data. The second part of the code focuses on training a large number of different types of models to use as estimates in later experiments. 

Lastly, once the models have been trained, we use the noisy estimates to simulate enrollment under various settings for our dataset of students via deferred acceptance, using the same method as that of the synthetic experiments. 

We explain how to run each of these steps below. We note that the model training step takes a large amount of time, so we provide instructions on how to run from scratch as well as how to run using our large corpus of pre-trained models. 

### Data Cleaning
The data cleaning step takes place in `school_data_exps/data/data_cleaning.ipynb`. The dataset output by this process can be found in `no_gpa_3_cleaned_data.ipynb` in the same folder. To run from scratch, the code requires downloading the ELS:2002 data from https://nces.ed.gov/surveys/els2002/, which cannot be included due to size constraints. Here are the instructions for downloading the correct dataset: 
1) Click this link to download the student dataset: https://nces.ed.gov/datalab/files/zip/OnlineCodebook/ELS_2002-12_PETS_v1_0_Student_CSV_Datasets.zip
2) Unzip the file, it should contain a single csv file named `els_02_12_byf3pststu_v1_0.csv`
3) Move this file to the `ELS_data` folder in this project. 

### Training Models
The model training step takes place in the `models/model_training.ipynb` notebook. Running the notebook from scratch will use the data generated in the data cleaning step to train and save 1000 models to predict whether a student's first year college GPA will be above 3. The entire training process takes roughly 100 minutes when parallelization is enabled. 

If *not* running from scratch, pretrained models can be found in the `models/pretrained_models` folder. To use these models, skip the training models step and go to the running enrollment simulations step in the next section. 

### Running Enrollment Simulations
Once the data and models have been prepared, experiments simulating enrollment can be run from `simulations/enrollment_simulations.ipynb.` Again, there is both a pre-run and from-scratch option. To just plot the pre-generated data, run the code found in `simulations/simulation_plots.ipynb`.

To generate simulation data from scratch, first generate data by running `simulations/enrollment_simulations.ipynb`, replacing the pretrained models path with newly trained models if desired, and then plot the data by modifying `simulations/simulation_plots.ipynb` to point to the location of the new data, and run as usual. 

## License
This code is released under the MIT License. See the LICENSE file for details.
