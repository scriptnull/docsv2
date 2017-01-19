page_title: Deploy a sample application using Shippable's continuous delivery pipelines
page_description: Quick start for getting up to speed with pipelines
page_keywords: getting started, formations, quick start, documentation, shippable

#Connecting CI to Pipelines

Combining CI and Pipelines gives you a powerful end to end Continuous Delivery solution that can be up and running in minutes. The typical workflow is demonstrated in our [tutorial showing how to deploy a sample application](samplePipeline/).

This tutorial is focused on a specific scenario - how do you trigger your pipeline after your CI build succeeds?

Let us assume the following scenario: Your CI build runs unit tests and generates a Docker image, which you push to a Docker registry. Your pipeline takes this image, creates a service manifest, and then deploys it to a test environment.

<img src="../../images/pipelines/connectingCiPipelinesHow.png" alt="Shippable Continuous Integration and Delivery" style="width:800px;"/>

We're going to see how we can replace the question mark in the picture above with a trigger that will trigger a new version of myImage to be created, which will in turn trigger the manifest job and so on.

* Create an API token for your account. To do this, go to your **Account Settings** by clicking on the gear icon in the top navbar. Then click on **API tokens** in the left sidebar menu and create a token. Copy the token since you won't be able to see it again.

* Next, create an account integration of type 'Event Trigger'
    * While still on **Account Settings**, go to  **Integrations** in the left sidebar menu and then click on **Add Integration**
    * Click the **Create Integration** button next to **Event Trigger**. In the **Create Event Trigger Integration** tab, complete the settings as shown below. Select the subscription containing your CI project; this will add the account integration to that subscription as a subscription integration. Please make sure you update the `Authorization` textbox in the format `apiToken <token-value>`. The resource name should be the resource that refers to the image you're pushing as part of your CI.

    <br>

<img src="../../images/pipelines/samplePipelineEventTriggerIntegration.png" alt="Shippable Continuous Integration and Delivery" style="width:800px;"/>

* Next, add the following to the shippable.yml for your CI project:

```
integrations:
  notifications:
    - integrationName: triggerDemoPipeline #Replace with name of the integration from subscription settings
    type: webhook
    payload:
      - versionName=$BRANCH.$BUILD_NUMBER  #Replace with the tag of the image you pushed during CI
    on_success: always
    on_failure: never

```

And that's it. The next time you run your CI build and push your image, your pipeline will be triggered by your event trigger integration.

<img src="../../images/pipelines/connectingCiPipelines.png" alt="Shippable Continuous Integration and Delivery" style="width:800px;"/>
