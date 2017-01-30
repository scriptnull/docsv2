page_title: Deploying to Google Container Engine (GKE)
page_description: This tutorial shows you how to implement a CI/CD workflow with Docker build, Docker Hub, and deployments to GKE
page_keywords: getting started, formations, quick start, documentation, shippable

# Continuous Delivery for a Node.js app with GitHub, Docker Hub, and Kubernetes

This tutorial walks you through how to configure a continuous delivery pipeline using Shippable.

###Fork the demo project to your account

1. Fork the [node-build-push-docker-hub-deploy-kubernetes](https://github.com/shippableSamples/node-build-push-docker-hub-deploy-kubernetes) repository. This contains the source code for the demo, as well as CI config (shippable.yml) and pipeline config (shippable.resources.yml and shippable.jobs.yml).

###Create necessary  integrations

You will need the following integrations:

1. GitHub integration: Follow instructions on the [GitHub integration page](/integrations/scm/github/) for the sections:
    * **Adding an Account Integration**
    * **Use your integration in your Pipeline configuration**
2. Docker Hub integration: Follow instructions on the [Docker Hub integration page](/integrations/imageRegistries/dockerHub/) for the sections: **Adding an Account Integration**

3. A Kubernetes integration: Follow instructions on the [Kubernetes integration page](/integrations/containerServices/kubernetes/)

###Set up CI for the sample application

Before you get started with setting up your deployments, let's set up CI for your sample application.

1. Make the following changes to the shippable.yml at the root of your forked sample application:
    * replace `manishas-dh` with the **Docker Hub** integration you created. Please use the integration name from your Subscription Settings here.
    * in the `env` section, replace the value for `DOCKER_ACC` with your Docker Hub org name where you want to push the image.

1. [Enable your forked sample application for CI](http://docs.shippable.com/ci/runFirstBuild/#enable-a-project). From the Subscription page on Shippable, click on **Enable project** in the left sidebar menu, find the repo and click on **Enable**.

1. You can trigger a build for your sample application in one of two ways:
    * Go to the Shippable UI and click on `Build` for the project on the Subscription page
    * Make a small change to the project, even if it's just to the README file. This should trigger a build on Shippable. Wait for your green build!

1. After the build is complete, check your Docker Hub org to ensure that the image was pushed successfully. Write down the tag of the image pushed (should be master.build_number if everything went well).

###Create a Kubernetes pod
Creation of the pod is not covered extensively in this tutorial, since it assumes a pod is already available. Create a Kubernetes pod with at least one master and one slave. There are no other constraints. Refer to [Install Kubernetes on Linux with kubeadm docs](https://kubernetes.io/docs/getting-started-guides/kubeadm/)

###Edit pipeline configuration ymls

1. Open up **samplePipelinesTest/shippable.resources.yml** and make the following edits:
    * shipdemo-img resource
        * replace `integration: manishas-dh`  with `integration: <your docker hub integration name>`
        * replace `sourceName` value with `sourceName: <your Docker Hub org name>/node-build-push-docker-hub-deploy-kubernetes`
        * replace `versionName: master.1` with `version: <image tag you copied from Docker Hub>`
    * kube-cluster resource
        * replace 'integration: manisha-kube' with `integration: <your Kubernetes integration name>`

### Understand the configuration

Before you proceed with setting up this pipeline on Shippable, let's take a moment to understand what it does.

The resources configured in shippable.resources.yml are:

1. shipdemo-img is an [image](../../pipelines/resources/image/) resource for the image to be deployed.
1. shipdemo-img-opts is a [dockerOptions](../../pipelines/resources/image/) resource which specifies options for the container, like memory, port mappings, etc.
1. kube-cluster is a [cluster](../../pipelines/resources/cluster/) resource specifying where the demo application should be deployed

The jobs configured in shippable.jobs.yml are:

1. shipdemo-manifest is a [manifest](../../pipelines/jobs/manifest/) job that creates a new service manifest each time the image shipdemo-img is updated.
1. kube-deploy is a [deploy](../../pipelines/jobs/deploy/) job that deploys the manifest shipdemo-manifest to the Test cluster kube-cluster

### Seed your pipeline in Shippable

1. From the Shippable dashboard, go to the Subscription where you forked both repositories
1. Follow instructions on the Pipelines page to [seed your pipeline](../../pipelines/gettingStarted/#seedPipeline).
1. Go to the SPOG pill menu of your Pipelines tab and voila! You should see your pipeline there:

###Deploy to Test
Right click on the **shipdemo-manifest** job in the SPOG view and click on `Run`. This will run the manifest job which creates a new service manifest. The deploy job is set up to run after manifest finishes, so it will be automatically triggered.

<img src="/tutorials/images/pipelines/deployment-pipeline-to-kubernetes.png" alt="Shippable Continuous Integration and Delivery" style="width:1000px;"/>

###Check out your deployed container in your Kubernetes pod

You can now go to your Kubernetes pod and make sure your app is deployed. Running `docker ps` should show you the container.

###Connect CI and Pipelines

Now that you have your pipeline up and running, you should connect it to your CI. On completing this step, every code change to your sample application will trigger a deployment to the Test cluster we set up in the previous steps.

To do this:

1. Create an API token for your account. To do this, go to your **Account Settings** by clicking on the gear icon in the top navbar. Then click on **API tokens** in the left sidebar menu and create a token. Copy the token since you won't be able to see it again.  

1. Next, we will create an account integration of type 'Event Trigger'
    * Go to  **Integrations** in the left sidebar menu and then click on **Add Integration**
    * Select **Event Trigger** from the list and complete the settings. Choose `Resource` and then pick the shipdemo-img resource. Please make sure you update the `Authorization` textbox in the format `apiToken <token-value>`. Also, make sure you assign the integration to the Subscription where you've forked the sample repo.

    <img src="/tutorials/images/pipelines/trigger-deploy-kubernetes.png" alt="Trigger a deployment to Kubernetes" style="width:400px;"/>

1. Next, make the following changes to the shippable.yml at the root of your forked sample application. Add the following in the `notifications` section:

```
notifications:
  - integrationName: #your event trigger integration name
    type: webhook
    payload:
      - versionName=$BUILD_NUMBER
    on_success: always
```

Try pushing a simple change to the sample app, even as simple as changing the README file or [changing the image](https://github.com/shippableSamples/node-build-push-docker-hub-deploy-kubernetes/blob/master/public/resources/images/captain.png), and see your pipeline light up! The newer version of your app will be deployed to your Kubernetes pod.
