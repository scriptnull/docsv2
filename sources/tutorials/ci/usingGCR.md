# Using your Google Container Registry (GCR) integration

Shippable integrates with all docker registries so you can interact with your docker images as part of your [CI](../../../../ci/overview/) and [CD](../../../../pipelines/overview/) workflows.

This page has several mini-tutorials designed to help you get started with your actually using your Google Container Registry (GCR) integration to implement common scenarios.

## Continuous integration tutorials

You can interact with Docker images hosted on GCR in your CI workflow in the following ways:

- [Use an image from GCR as your build container](#customImage)
- [Pull an image as part of your CI workflow](#pullImage)
- [Build an image which pulls the base image from GCR](#buildImage)
- [Push an image](#pushImage)

If you're interacting with private images, follow the instructions in the Private Images section below before proceeding to your scenario.

<a name="dockerHubCreds"></a>
### GCR credentials setup

If you want to pull a private image from GCR or want to push an image, you will first need to complete the following steps:

* Add an [account integration for GCR](../../../../integrations/imageRegistries/gcr/)
* Go to your Subscription's Settings tab. This should be the Subscription containing your project.
* Click on **Integrations** in the sidebar menu.
* Click on **Add Integration**.
* Name your GCR integration with a friendly name. This can be the same name as the one in your account integration.
* From the dropdown, choose the account integration for GCR
* Click on **Save**. This should save your integration to your Subscription.

Next, customize the following snippet and include it in the `shippable.yml` for your project:

```
integrations:                               #required only for private images
    hub:
      - integrationName: myIntegration
        type: gcr
        branches:
          only:
            - master
            - dev
```
- `integrationName` value is the name of the GCR integration you added to the 'Subscription' settings. It is important the name matches exactly. If not, the build will fail with an error as  [described here](/ci/troubleshoot/#integration-name-specified-in-yml-does-not-match). Moreover, this account should have permissions to pull the the build image specified in the `image_name` setting.
- `type` is `docker`.
- [optional] `branches` section: specify the branches this integration is applicable to. You can skip this if you want your integration to be applicable for all branches.. The `only` tag should be used when you want the integration on specific branches. You can also use the `except` tag to exclude specific branches.

<a name="customImage"></a>
### Using an image from GCR as your build container

Before you start, please ensure that you have completed the [GCR credentials setup](#dockerHubCreds) **if you are pulling a private image**.

You can configure shippable.yml to pull an image and use it as your [build container](../../../../ci/overview/#ciWorkflow) by including the following:

```
build:
  pre_ci_boot:
    image_name: gcr.io/myOrg/myImageRepo    #required
    image_tag: imageTag                     #required
    pull: true                              #required, false if image already built or pulled in pre_ci
    options: "-e HOME=/root"                #optional

```
While the above is a sample code for your `shippable.yml`, use the descriptions of each field below to modify the `yml` and tailor it to your requirements.

- `image_name:` value is in the format gcr.io/docker-registry-username/docker-registry-image-repo.
- `image_tag:` value is the tag for the image that you want to pull.
- `pull:` set to `true` to pull the image from GCR. If the image is already on your build machine because it was built or pulled during `pre_ci`, you can set this to `false`
- In the `env` section, you can enter any environment variables you want to be set inside your CI container. Read more about [environment variables](/ci/advancedOptions/envVar/).
- In the `options` tag, enter any docker options you want to use in the docker run command. You also need to include the HOME environment variable as shown if it is not already set in your image.

<a name="pullImage"></a>
### Pulling an image as part of your CI

Before you start, please ensure that you have completed the [GCR credentials setup](#dockerHubCreds) **if you are pulling a private image**.

You can configure shippable.yml to pull an image as part of your workflow by including the following:

```
build:
  ci:
    -  docker pull gcr.io/myOrg/myImageRepo:myTag
```

The `docker pull` command will work in the `pre_ci`, `ci`, `post_ci`, `on_success` and `on_failure` sections of your yml.

<a name="buildImage"></a>
### Building an image

Before you start, please ensure that you have completed the [GCR credentials setup](#dockerHubCreds) **if your Dockerfile pulls a private image**.

You can configure shippable.yml to build a container as part of your workflow by including the following:

```
build:
  pre_ci:
    -  docker build -t gcr.io/myOrg/myImageRepo:myTag .
```
The `docker build` command will work in the `pre_ci`, `ci`, `post_ci`, `on_success` and `on_failure` sections of your yml.

<a name="pushImage"></a>
###Pushing an image

Before you start, please ensure that you have completed the [GCR credentials setup](#dockerHubCreds).

You can configure shippable.yml to push an image as part of your workflow by including the following:

```
build:
  post_ci:
    #Commit the container only if you want all the artifacts from the CI steps
    - docker commit $SHIPPABLE_CONTAINER_NAME gcr.io/myOrg/myImageRepo:myTag

    #push your image
    - docker push gcr.io/myOrg/myImageRepo:myTag
```

You can push your image to GCR in the `ci`, `post_ci`, `on_success` and `on_failure` sections of your yml.

##Pipeline tutorials

You can interact with images from GCR as part of your [Continuous Deployment pipelines](../../../../pipelines/overview/).

To learn more, check out the [image resource section](../../../../pipelines/resources/image/) and [service manifests](../../../../pipelines/jobs/manifest/).
