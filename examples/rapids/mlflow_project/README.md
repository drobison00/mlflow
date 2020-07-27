### Train and Publish Locally With RAPIDS and MLflow
**[RAPIDS](https://rapids.ai/)** is a suite of open source libraries for GPU-accelerated analytics. 

**[RAPIDS cuML](https://github.com/rapidsai/cuml)** matches the scikit-learn API, so it can build on MLflow's existing support for scikit-learn-like models to support
persistence and deployment."

#### Jupyter Notebook Workflow
[Jupyter Notebook](notebooks/rapids_mlflow.ipynb)

#### CLI Based Workflow
1. Download data
    1. `cd examples/rapids/mlflow_project`
        ```shell script
        # Download the file
        wget -N https://rapidsai-cloud-ml-sample-data.s3-us-west-2.amazonaws.com/airline_small.parquet
        ```
1. Set MLflow tracking uri
    1. ```shell script
        export MLFLOW_TRACKING_URI=sqlite:////tmp/mlflow-db.sqlite
       ```
1. Train the model using a single run.
    1. ```shell script
       # Launch the job
       mlflow run . -e simple\
                --experiment-name RAPIDS-CLI \
                -P max_depth=10 -P max_features=0.75 -P n_estimators=500 \
                -P conda-env=$PWD/envs/conda.yaml \
                -P fpath=airline_small.parquet
       ```
1. Train the model with Hyperopt
    1. ```shell script
       # Launch the job
       mlflow run . -e hyperopt \
                --experiment-name RAPIDS-CLI \
                -P conda-env=$PWD/envs/conda.yaml \
                -P fpath=airline_small.parquet
       ```
    1. In the output, note: "Created version '[VERSION]' of model 'rapids_mlflow'"

1. Deploy your model
    1. Deploy your model
        1. `$ mlflow models serve --no-conda -m models:/rapids_mlflow_cli/[VERSION] -p 55755`

1. Query the deployed model with test data `src/sample_server_query.sh` example script.
    1. `bash src/sample_server_query.sh`