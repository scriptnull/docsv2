page_title: Amazon EC2 Container Registry (ECR) integration
page_description: Setting up Shippable account integrations for Amazon ECR
page_keywords: Docker Hub, amazon, ecs, gcr, google, shippable, quay, coreos, docker, registry, EC2 Container Service, Google, Docker Trusted registry, private, elastic compute cloud

# Amazon EC2 Container Registry (ECR) integration

An ECR integration lets you configure the following scenarios:

- Pull a private image for your CI workflow
- Build a Docker image which has a `FROM` that pulls a private image
- Push an image as part of CI
- Use an [image resource](../../pipelines/resources/image/) as part of your CD [pipeline](../../pipelines/overview/)

##Adding the Account Integration

You will need to configure this integration to pull from or push images to Amazon ECR.

1. Click on the gear icon for Account Settings in your top navigation bar and then click on the 'Integrations' section.
2. Click on the `Add Integration` button.
3. Choose `Amazon ECR` from the list of integration types.
4. For 'Integration Name' use a distinctive name that's easy to associate to the integration and recall. Example: `ecr-integration`
5. Enter your aws_access_key_id and aws_secret_access_key. You can follow instructions in [Amazon's guide for Creating and Managing access keys](http://docs.aws.amazon.com/general/latest/gr/managing-aws-access-keys.html)
6. Assign this integration to the Subscription(s) you want to use it in. Since you're likely a member of many organizations, you need to specify which of them can use this integration.
7. Click on `Save`

The integration will now be available to all your Continuous Integration and Deployment workflows.

<img src="/ci/images/integrations/imageRegistries/ecr/addInt.png" alt="Amazon EC2 Container Registry integration" style="width:700px;"/>

To learn how to use the integration for your scenario, check out the tutorials below.

---

##Tutorials

We have published the following tutorials to help get started on working with images stored in ECR as part of your CI/Pipelines workflows.

* [Pulling a Docker image from ECR](../../../tutorials/ci/hub-amazon-ecr-pull-docker-image/)

* [Using a Docker image from ECR to spin up your CI container](../../../tutorials/ci/hub-amazon-ecr-custom-ci-image/)

* [Build a Docker image](../../../tutorials/ci/hub-amazon-ecr-build-docker-image/)

* [Push a Docker image to ECR](../../../tutorials/ci/hub-amazon-ecr-push-docker-image/)

---

##Deleting the Integration

To remove the Amazon ECR integration, you'll need to remove this integration from all dependencies configured to use it. To find all the dependencies:

1. Click on the gear icon for Account Settings in your top navigation bar and then click on the `Integrations` section.
2. Select the Amazon ECR integration from the list of integrations. If you have many entries, use the `Filters` dropdown and select `ECR`. Alternatively, you can use the `Integration Name` field to provide the name of your Amazon ECR integration.
3. Your Amazon ECR integration shows up in the list.
4. Click on the `Delete` button.
5. A window pops up confirming that you want to delete the integration. This window lists all dependencies of this this integration. The list will include any project, environment or subscription image dependent on this integration.
6. If there are dependencies, individually access the `Settings` tab for each project/environment/subscription image and delete the Amazon ECR integration.
7. Once all dependencies of the Amazon ECR integration have been removed, Step 5 will show the message: `No dependency`.
8. Click the `Yes` button to delete the Amazon ECR Integration.

---
