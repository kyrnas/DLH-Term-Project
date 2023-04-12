# DLH-Term-Project - Group 71 - Temporal Pointwise Convolutional Networks for Length of Stay Prediction in the Intensive Care Unit

Source Code: https://github.com/EmmaRocheteau/TPC-LoS-prediction

## Pre-processing Instructions

### eICU

1) To run the sql files you must have the eICU database set up: https://physionet.org/content/eicu-crd/2.0/. 

2) Follow the instructions: https://eicu-crd.mit.edu/tutorials/install_eicu_locally/ to ensure the correct connection configuration. 

3) Replace the eICU_path in `paths.json` to a convenient location in your computer, and do the same for `eICU_preprocessing/create_all_tables.sql` using find and replace for 
`'/Users/emmarocheteau/PycharmProjects/TPC-LoS-prediction/eICU_data/'`. Leave the extra '/' at the end.

4) In your terminal, navigate to the project directory, then type the following commands:

    ```
    psql 'dbname=eicu user=eicu options=--search_path=eicu'
    ```
    
    Inside the psql console:
    
    ```
    \i eICU_preprocessing/create_all_tables.sql
    ```
    
    This step might take a couple of hours.
    
    To quit the psql console:
    
    ```
    \q
    ```
    
5) Then run the pre-processing scripts in your terminal. This will need to run overnight:

    ```
    python3 -m eICU_preprocessing.run_all_preprocessing
    ```
    
## Running the models
1) Once you have run the pre-processing steps you can run all the models in your terminal. Set the working directory to the TPC-LoS-prediction, and run the following:

    ```
    python3 -m models.run_tpc
    ```
    
    Note that your experiment can be customised by using command line arguments e.g.
    
    ```
    python3 -m models.run_tpc --dataset eICU --task LoS --model_type tpc --n_layers 4 --kernel_size 3 --no_temp_kernels 10 --point_size 10 --last_linear_size 20 --diagnosis_size 20 --batch_size 64 --learning_rate 0.001 --main_dropout_rate 0.3 --temp_dropout_rate 0.1 
    ```
    
    Each experiment you run will create a directory within models/experiments. The naming of the directory is based on 
    the date and time that you ran the experiment (to ensure that there are no name clashes). The experiments are saved 
    in the standard trixi format: https://trixi.readthedocs.io/en/latest/_api/trixi.experiment.html.
    
2) The hyperparameter searches can be replicated by running:

    ```
    python3 -m models.hyperparameter_scripts.eICU.tpc
    ```
 
    Trixi provides a useful way to visualise effects of the hyperparameters (after running the following command, navigate to http://localhost:8080 in your browser):
    
    ```
    python3 -m trixi.browser --port 8080 models/experiments/hyperparameters/eICU/TPC
    ```
    
    The final experiments for the paper are found in models/final_experiment_scripts e.g.:
    
    ```
    python3 -m models.final_experiment_scripts.eICU.LoS.tpc
    ```
   
 ## Run results
 To view the run results navigate to the `models/experiments` folder. Choose the `eICU` foler to browse the training results or the `final` folder to 
 browse the testing results. 
 
