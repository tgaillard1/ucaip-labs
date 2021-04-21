# Code labs for Cloud AI Platform (Unified)

## Getting started

1. [Create a GCP Project](https://cloud.google.com/resource-manager/docs/creating-managing-projects#console), [enable billing](https://cloud.google.com/billing/docs/how-to/modify-project), and [create a GCS bucket](https://cloud.google.com/storage/docs/creating-buckets).
2. [Enable the required APIs](https://cloud.google.com/endpoints/docs/openapi/enable-api).
3. Generate [API Key](https://cloud.google.com/docs/authentication/api-keys) to use for submitting AI Platfrom Managed Pipeline jobs.
4. [Create an AI Notebook instance](https://cloud.google.com/ai-platform/notebooks/docs/create-new).
5. Open the JupyterLab then open a new Terminal
6. Clone the repository to your AI Notebook instance:
```
git clone https://github.com/ksalama/ucaip-labs.git
cd ucaip-labs
```
7. Open [00-env-setup.ipynb](00-env-setup.ipynb) and run the cells to tnstall requirements

## Data Analysis and Preparation

The [Chicago Taxi Trips](https://pantheon.corp.google.com/marketplace/details/city-of-chicago-public-data/chicago-taxi-trips) dataset is one ofof [public datasets hosted with BigQuery](https://cloud.google.com/bigquery/public-data/), which includes taxi trips from 2013 to the present, reported to the City of Chicago in its role as a regulatory agency. The task is to predict whether a given trip will result in a tip > 20%.

The [01-data-analysis-and-prep](01-data-analysis-and-prep.ipynb) notebook covers:
1. Performing exploratory data analysis on the data in BigQuery.
2. Creating managed AI Platform Dataset using the Python SDK.
3. Generating the schema for the raw data using [TensorFlow Data Validation](https://www.tensorflow.org/tfx/guide/tfdv).


## Experimentation

We experiment with creating two models: [AutoML](https://cloud.google.com/ai-platform-unified/docs/training/training) and [Custom Model](https://cloud.google.com/ai-platform-unified/docs/training/create-model-custom-training). 

1. The [02-1-experimentation-automl](02-1-experimentation-automl.ipynb) notebook covers:
    1. Using AutoML Tables to create a classification model.
    2. Retrieving the evaluation metrics from the AutoML model.

2. The [02-2-experimentation-keras](02-2-experimentation-keras.ipynb) notebook covers:
    1. Preparing the data using Dataflow
    2. Implementing a Keras classification model
    3. Training the keras model in AI Platform using a [pre-built container](https://cloud.google.com/ai-platform-unified/docs/training/pre-built-containers)
    4. Upload the exported model from Cloud Storahe to AI Platform as a Model.

## Model Deployment and Prediction Serving

We serve the model trained either using AutoML Tables or a custom training job for predictions and explainations.
The [03-model-serving](03-model-serving.ipynb) notebook covers:
1. Creating an AI Platform Endpoint.
2. Deploy the AutoML Tables and the custom modesl to the endpoint.
4. Test the endpoints for online prediction.
5. Getting online explaination from the AutoML Tables mode.
5. Use the uploaded custom model for batch prediciton.

## ML Training Pipeline

We build an end-to-end [TFX training pipeline](tfx_pipline) that performs the following steps:
1. Receive hyperparameters using hyperparam_gen custom python component.
2. Extract data from BigQuery using BigQueryExampleGen.
3. Validate the raw data using StatisticsGen and ExampleValidator.
4. Process the data using Transform.
5. Train a custom model using Trainer.
6. Train an AutoML Tables model using automl_trainer custom python component.
7. Evaluat the custom model using ModelEvaluator.
8. Validate the custom model against the AutoML Tables model using a custom python component.
9. Save the blessed to model registry location using using Pusher.
10. Upload the model to AI Platform using aip_model_pusher custom python component.

We have the following notebooks for the ML training pipeline:
1. The [04-tfx-interactive](04-tfx-interactive.ipynb) notebook covers testing the pipeline components interactively.
2. The [05-tfx-local-run](05-tfx-local-run.ipynb) notebook covers running the end-to-end pipeline locally.
3. The [06-tfx-kfp-deploy](06-tfx-kfp-deploy.ipynb) notebook covers compiling and deploying the pipeline to AI Platform Hosted KFP.
4. The [07-tfx-managed-run](07-tfx-managed-run.ipynb) notebook covers compiling and running the pipeline to AI Platform Managed Pipelines.


## (TODO) Model Monitoring

## (TODO) ML Metadata Tracking




