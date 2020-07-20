# AWS CI/CD Pipeline For a Sample Web application

## Summary
This repository contains CloudFormation templates to create a continuous integration and deployment pipeline using AWS CodeCommit, CodeBuild, ECR, ECS and CodePipeline.

## Getting Started

#### (1) Create a Git repository with CodeCommit
Use the [code-repo.yaml](part1/cft-templates/code-repo.yaml) to create a git repository in AWS CodeCommit. 

> Validate and deploy the yaml file with awscli
```sh
aws cloudformation validate-template --template-body file://code-repo.yaml
aws cloudformation deploy --template-file code-repo.yaml --stack-name web-app-repo-stack --parameter RepositoryName=web-app
```

#### (2) Clone the repository and push this sample code to the repo
- _**Note:**_: If you are yet to setup ssh keys for CodeCommit, follow the steps described [here](https://docs.aws.amazon.com/codecommit/latest/userguide/setting-up-ssh-unixes.html)
- You can either push an application you have or use the sample [web-app](part1/web-app) in this repository
- If you choose to use your own application, add [buildspec.yml](part1/web-app/buildspec.yml) to your repository to build a docker image and push it to ECR

#### (3) Create a cluster to deploy and run docker container
Use the [ecs-cluster.yaml](part1/cft-templates/ecs-cluster.yaml) to create a cluster that will run your application

- Modifty the ContainerDefinitions (line 44) section of the yaml file to include the name, entrypoint and image of your application

> Validate and deploy the yaml file with awscli
```sh
aws cloudformation validate-template --template-body file://ecs-cluster.yaml
aws cloudformation deploy --template-file ecs-cluster.yaml --stack-name ecs-cluster-stack --parameter SubnetId=<ENTER SUBNET ID> --capabilities CAPABILITY_NAMED_IAM
```

#### (4) Create CodePipeline to continuously build and deploy your application
Use the [code-pipeline.yaml](part1/cft-templates/code-pipeline.yaml) to create a pipeline. 

- **_Note_**: If you created a your cluster with a different name or service, update the _ClusterName_ (line 142) and _ClusterService_ (line 143) in the file

> Validate and deploy the yaml file with awscli. Pass your _repository name_
```sh
aws cloudformation validate-template --template-body file://code-pipeline.yaml
aws cloudformation deploy --template-file code-pipeline.yaml --stack-name code-pipeline-stack --parameter RepositoryName=web-app --capabilities CAPABILITY_NAMED_IAM
```