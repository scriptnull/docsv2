#Building a Docker image

In some scenarios, you might want to build your Docker image as part of your CI workflow. There are usually a couple of primary reasons for this:

* You want to [package your application in a docker container and push it to a registry](/tutorials/ci/hub-docker-push-image/)
* You want to build your own Docker image and use it to [override the default image for your CI build](/tutorials/ci/hub-docker-custom-ci-image/)

The basic building block for building a Docker image as part of your CI workflow is shown in the steps below. To learn how to implement the scenarios above, please click on the either scenario.

* If your Dockerfile pulls a private image from Docker Hub, you will first need to complete the instructions for adding an [account integration for Docker Hub](/integrations/imageRegistries/dockerHub/). Next, customize the following snippet and include it in the `shippable.yml` for your project:

```
integrations:                               #required only for private images
  hub:
    - integrationName: myIntegration    
      type: docker                        
      branches:                           
        only:
          - master
          - dev
```
- `integrationName` value is the name of the Docker Hub integration you added to the 'Subscription' settings. It is important the name matches exactly. If not, the build will fail with an error as  [described here](/ci/troubleshoot/#integration-name-specified-in-yml-does-not-match). Moreover, this account should have permissions to pull the the build image specified in the `image_name` setting.
- `type` is `docker`.
- [optional] `branches` section: specify the branches this integration is applicable to. You can skip this if you want your integration to be applicable for all branches.. The `only` tag should be used when you want the integration on specific branches. You can also use the `except` tag to exclude specific branches.

* You can configure `shippable.yml` to build a container as part of your workflow by including the following:

```
build:
  ci:
    -  docker build -t myOrg/myImageRepo:myTag .
```
The `docker build` command will work in the `pre_ci`, `ci`, `post_ci`, `on_success` and `on_failure` sections of your yml.
