#Using a custom Docker image from Docker Hub for your builds

By default, all CI builds on Shippable run on our default Docker images which are preconfigured depending on programming language.

However, you have the option to override the default image and use your own if needed. In most cases, customers choose to do this if they are using Docker for their applications and have a very specific requirements for the build image.

To override the default CI build image and use your own, do the following:

* If you're using a private image, add an [account integration for Docker Hub](/integrations/imageRegistries/dockerHub/). Next, customize the following snippet and include it in the `shippable.yml` for your project:

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

* If you want to build a Docker image and then use it for your CI, you should follow instructions in our [Build a Docker image](/tutorials/ci/hub-docker-build-image/) tutorial. Please note that you should build the image in the `pre-ci` section of the yml.

* Add the following to your yml:

```
build:
  pre_ci_boot:
    image_name: docker-hub-org/image-name
    image_tag: tip
    pull: true
    options: "-e HOME=/root"

```
- `image_name` value is the fully qualified name of the image you want to pull
- `image_tag` is the tag for the image you want to pull
- set `pull` to `true` if you want to pull the image from Docker Hub and `false` if you pulled or [built the Docker image](/tutorials/ci/hub-docker-build-image/) in the `pre_ci` step.
- In the `options` tag, enter any docker options you want to use in the docker run command. You also need to include the HOME environment variable as shown if it is not already set in your 'Dockerfile'.
