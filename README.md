# MLOps on Azure
This repository contains sample code, which is used for a [MLOps](https://en.wikipedia.org/wiki/MLOps) Workshop.

In this workshop various cloud services will be used, e.g. [Azure ML Services](https://azure.microsoft.com/en-us/services/machine-learning-service/) and [Azure DevOps](https://azure.microsoft.com/en-us/services/devops/).

## What is MLOps?
**MLOps** (a compound of "[machine learning](https://en.wikipedia.org/wiki/Machine_learning)" and "operations") is a practice for collaboration and communication between [data scientists](https://en.wikipedia.org/wiki/Data_scientists) and operations professionals to help manage production ML lifecycle.
Similar to the [DevOps](https://en.wikipedia.org/wiki/DevOps) or [DataOps](https://en.wikipedia.org/wiki/DataOps) approaches, MLOps looks to increase automation and improve the quality of production ML while also focusing on business and regulatory requirements.

## How does Azure ML help with MLOps?
Azure ML contains a number of asset management and orchestration services to help you manage the lifecycle of your model training & deployment workflows.

With Azure ML + Azure DevOps you can effectively and cohesively manage your datasets, experiments, models, and ML-infused applications.
![ML lifecycle](./media/ml-lifecycle.png)



## DevOps for Machine Learning (Azure MLOps Part 3) - Train and Version Your Model

    01_CreateMetadataAndModelsFolders.sh
    mkdir metadata && mkdir models

    02_TrainModel.sh
    az ml run submit-script -g $(azureml.resourceGroup) -w $(azureml.workspaceName) -e $(experiment.name) --ct $(amlcompute.clusterName) -d conda_dependencies.yml -c train_diabetes -t ../metadata/run.json train_diabetes.py

    03_RegisterModel.sh
    az ml model register -g $(azureml.resourceGroup) -w $(azureml.workspaceName) -n $(model.name) -f metadata/run.json  --asset-path outputs/models/sklearn_diabetes_model.pkl -d "Linear model using diabetes dataset" --tag "data"="diabetes" --tag "model"="regression" --model-framework ScikitLearn -t metadata/model.json

    04_DownloadModel.sh
    az ml model download -g $(azureml.resourceGroup) -w $(azureml.workspaceName) -i $(jq -r .modelId metadata/model.json) -t ./models --overwrite

    05_CopyFilesToArtifactStagingDirectory.txt

    **/metadata/*
    **/models/*
    **/deployment/*
    **/setup/*
    **/tests/integration/*


