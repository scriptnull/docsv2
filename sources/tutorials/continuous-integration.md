page_title: Continuous Integration tutorials
page_description: Learn how to implement continuous integration, including Docker based workflows
page_keywords: DevOps, continuous integration, CI/CD, Docker

# Continuous Integration tutorials

This is a rich list of resources that should help you get started with continuous integration on Shippable's CI/CD platform.

### Running your first build

[Running your first build on Shippable](/tutorials/ci/runSampleCIBuild/)

### Reporting code coverage

[Code coverage with JaCoCo](/tutorials/ci/code-coverage-jacoco/)

### JFrog Artifactory

[Pushing to JFrog Artifactory after CI (Blog)](http://blog.shippable.com/pushing-to-jfrog-artifactory-after-ci)

### Interacting with Docker registries

####Docker Hub

* [Build Docker image](/tutorials/ci/hub-docker-build-image/)
* [Pull Docker image from Docker Hub](/tutorials/ci/hub-docker-pull-image/)
* [Push Docker image to Docker Hub](/tutorials/ci/hub-docker-push-image/)
* [Use custom image from Docker Hub for your builds](/tutorials/ci/hub-docker-custom-ci-image/)

####Amazon ECR

* [Build Docker image with a base image from Amazon ECR](/tutorials/ci/hub-amazon-ecr-build-docker-image/)
* [Pull Docker image from Amazon ECR](/tutorials/ci/hub-amazon-ecr-pull-docker-image/)
* [Push Docker image to Amazon ECR](/tutorials/ci/hub-amazon-ecr-push-docker-image/)
* [Use custom image from Amazon ECR for your builds](/tutorials/ci/hub-amazon-ecr-custom-ci-image/)

####Google Container Registry (GCR)

* [Build Docker image](/tutorials/ci/hub-gcr-build-docker-image/)
* [Pull Docker image from GCR](/tutorials/ci/hub-gcr-pull-docker-image/)
* [Push Docker image to GCR](/tutorials/ci/hub-gcr-push-docker-image/)
* [Use custom image from GCR for your builds](/tutorials/ci/hub-gcr-custom-ci-image/)

###Integrating with source control providers

* [Using Gitlab with Shippable](/tutorials/ci/scm-gitlab-ci/)
* [Linking your accounts from different providers](/tutorials/ci/link-github-bitbucket/)

### Deploying to IaaS/PaaS providers

The following tutorials show you how you can deploy your package after CI. For end to end Continuous Delivery pipelines, check out our [pipelines overview](/pipelines/overview/)

* [Deploy to AWS using Amazon CodeDeploy](/tutorials/ci/deploy-amazon-codedeploy/)
* [Deploy to Digital Ocean](/tutorials/ci/deploy-digital-ocean/)
* [Deploy to Heroku](/tutorials/ci/deploy-heroku/)
* [Deploy to AWS OpsWorks](/tutorials/ci/integrations/deploy/usingOpsWorls/)
* [Deploy a Node.js app to AWS Elastic Beanstalk (Blog)](http://blog.shippable.com/how-to-deploy-to-elastic-beanstalk-part-1)
* [Deploy a Docker app AWS Elastic Beanstalk (Blog)](http://blog.shippable.com/how-to-deploy-to-elastic-beanstalk-part-2)

### Advanced CI topics

* [Customizing CI workflows based on branch](http://blog.shippable.com/customize-environments-for-different-branches-of-a-continuous-integration-build)
* [Specifying different deployment targets based on branch](http://blog.shippable.com/specifying-deployment-targets-for-different-git-branches)
* [Matrix builds with minor Java versions](http://blog.shippable.com/matrix-builds-for-minor-java-versions-with-maven-and-artifactory)
* [Customizing the build badge](http://blog.shippable.com/customizing-build-badges-for-a-nodejs-project-status)
* [Changing the default timeout for a job](http://blog.shippable.com/changing-the-default-timeout-for-a-continuous-integration-project)
* [Retrying scripts to work around network hiccups](http://blog.shippable.com/automatically-retry-scripts-to-avoid-network-hiccups-during-ci-process)
* [Configuring cascading builds](http://blog.shippable.com/triggering-a-parameterized-build-after-continuous-integration)
* [Triggering a custom webhook after CI](http://blog.shippable.com/triggering-a-custom-webhook-after-continuous-integration)
