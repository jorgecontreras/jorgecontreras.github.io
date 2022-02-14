# Gitlab EKS Integration

## Overview

For this demo, we will create two small services in ngnix representing the two micro-services that will conform our system. The code of the projects will be hosted on GitLab. A commit to the repository will trigger the CI/CD pipeline which will execute our tests, generate a Docker image and finally deploy it to the EKS cluster.

## Prerequisites

- AWS account.
- Dockerhub account.
- GitLab. I have setup a Gitlab instance running in my local machine.
- Kubernetes. Container orchestration platform.
- Kaniko. This tool facilitates the creation of Docker images.
- Helm. Package manager for Kubernetes applications.

## Part I: Create the micro services.

Check the dockerfile of the project here:

## Part II: Define the CI/CD pipeline.
In GitLab, the pipeline is defined in a file that you create and name `.gitlab-ci.yml`, which must be located at the root of the project. Gitlab pipelines are divided into __Stages__, and each stage can have one or many __Jobs__. These jobs are picked and executed by __Runners__. This demo assumes that you have access to a Gitlab instance with a runner properly configured. 
Here is the `.gitlab-ci.yml` file used in this project:

```yaml
stages:
  - Build
  
build:
  stage: Build
  tags:
    - eks
  image:
    name: gcr.io/kaniko-project/executor:debug
    entrypoint: [""]
  script:
    - mkdir -p /kaniko/.docker
    - echo "{\"auths\":{\"https://index.docker.io/v1/\":{\"auth\":\"$(echo -n ${DOCKER_USER}:${DOCKER_PASSWORD} | base64)\"}}}" > /kaniko/.docker/config.json
    - /kaniko/executor --context $CI_PROJECT_DIR --dockerfile $CI_PROJECT_DIR/Dockerfile --destination $DOCKER_USER/api:$CI_COMMIT_SHA
```

This pipeline contains only the Build stage, and inside the build stage, there is a task called build. This task will make use of Kaniko to build a Docker image and upload it the Docker registry. Why Kaniko, you ask? To avoid the difficulties associated with running Docker inside Docker (DinD). In this example, I'm uploading to Dockerhub, but other destinations are also supported, like Amazon ECR or Google's GCR.

If you look at the script in the yml file, you'll notice a few variables used:

- `$CI_PROJECT_DIR`. Predefined by GitLab, it corresponds to the project directory.
- `$CI_COMMIT_SHA`. Also available by default, it contains the SHA of the commit.
- `$DOCKER_USER`. User defined variable. You need to create this variable in Gitlab. This is done under Settings > CICD > Variables. This variable should contain the username of your own Dockerhub account.
- `$DOCKER_PASSWORD`. Similarly, you need a variable to store your password. I'd recommend to create a personal token in Dockerhub and use that value here.

Committing any change to the repository will trigger the pipeline and create a new image every time. Of course, in a real world scenario you will not want to create an image on every commit. You can add rules in the gitlab pipeline to define when to execute this job, for example, when creating a tag, or for commits on a specific branch.

## Part III: Integrate GitLab with EKS.

Up until this point, what we have is a Laravel project and a CI/CD pipeline that builds and uploads a Docker image on every commit. The next thing we want to do is deploy our application to EKS.

> **Important!** The next step requires an AWS account and it may incur costs. Keep this in mind if you're following along and remember to delete any unnecessary resources afterwards.

## Part IV: Define the Kubernetes manifests.

## Part V: Deployment to Production.