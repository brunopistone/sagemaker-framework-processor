# SageMaker Framework Processor custom container

In this repo, we are showing how to extend an Amazon SageMaker built-in image for SKLearn and use it in a `FrameworkProcessor` for 
running a SageMaker Processing Job.

## Build Docker Image

### Create ECR Repository

```
aws ecr create-repository --repository-name <REPOSITORY_NAME>
```

* REPOSITORY_NAME: ECR Repository name

Example:

```
aws ecr create-repository --repository-name sagemaker-processing-sklearn
```

### Login to Spark Image repository

This step is required for pulling the SageMaker built-in images defined in the `FROM` section of the Dockerfile

```
aws ecr get-login-password --region <REGION> | docker login --username AWS --password-stdin <SPARK_IMAGE_URI>
```

* REGION: AWS Region
* SPARK_IMAGE_URI: Public ECR URI for the Spark image

Example:

```
aws ecr get-login-password --region eu-west-1 | docker login --username AWS --password-stdin 141502667606.dkr.ecr.eu-west-1.amazonaws.com/sagemaker-scikit-learn:0.23-1-cpu-py3
```

### Build custom image

```
docker build -t <ACCOUNT_ID>.dkr.ecr.<REGION>.amazonaws.com/${REPOSITORY_NAME}:${IMAGE_TAG} -f Dockerfile .
```

* ACCOUNT_ID: AWS Account ID
* REGION: AWS Region
* REPOSITORY_NAME: ECR Repository name
* IMAGE_TAG: Tag associated to the image

### Push Image to ECR

```
docker push <ACCOUNT_ID>.dkr.ecr.<REGION>.amazonaws.com/${REPOSITORY_NAME}:${IMAGE_TAG}
```

* ACCOUNT_ID: AWS Account ID
* REGION: AWS Region
* REPOSITORY_NAME: ECR Repository name
* IMAGE_TAG: Tag associated to the image
