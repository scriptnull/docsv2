
#Building a Docker image

In some scenarios, you might want to build your Docker image as part of your CI workflow. There are usually a couple of primary reasons for this:

* You want to [package your application in a docker container and push it GCR](/tutorials/ci/hub-gcr-push-docker-image/)
* You want to build your own Docker image and use it to [override the default image for your CI build](/tutorials/ci/hub-gcr-custom-ci-image/)

The basic building block for building a Docker image as part of your CI workflow is shown in the steps below. To learn how to implement the scenarios above, please click on the either scenario.

* Add an [account integration for GCR](/integrations/imageRegistries/gcr/). This is required because your Dockerfile likely pulls an image from GCR and we need credentials to do the pull on your behalf.

* Customize the following snippet and include it in the `shippable.yml` for your project:

```
integrations:                               
  hub:
    - integrationName: myIntegration
      type: gcr
      branches:
        only:
          - master
          - dev
```
- `integrationName` value is the name of the GCR integration you added in the previous step. It is important the name matches exactly. If not, the build will fail with an error as  [described here](/ci/troubleshoot/#integration-name-specified-in-yml-does-not-match).
- `type` is `gcr`.
- [optional] `branches` section: specify the branches this integration is applicable to. You can skip this if you want your integration to be applicable for all branches.. The `only` tag should be used when you want the integration on specific branches. You can also use the `except` tag to exclude specific branches.

* You can configure `shippable.yml` to build a container as part of your workflow by including the following:

```
build:
  ci:
    - docker build -t gcr.io/myOrg/myImageRepo:myTag .
```
The `docker build` command will work in the `pre_ci`, `ci`, `post_ci`, `on_success` and `on_failure` sections of your yml.

###Building Docker images with multiple tags

If you want to build the image with multiple tags, here is how you can do it:

```
build:
  ci:
    -  docker build -t gcr.io/myOrg/myImageRepo:myTag1 -t gcr.io/myOrg/myImageRepo:myTag2
```

###Docker build reference

For a complete list of everything supported by the `docker build` command, refer to the [official Docker docs](https://docs.docker.com/engine/reference/commandline/build/).
