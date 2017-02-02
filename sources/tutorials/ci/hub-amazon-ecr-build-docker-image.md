
#Building a Docker image

In some scenarios, you might want to build your Docker image as part of your CI workflow. There are usually a couple of primary reasons for this:

* You want to [package your application in a docker container and push it Amazon ECR](/tutorials/ci/hub-amazon-ecr-push-docker-image/)
* You want to build your own Docker image and use it to [override the default image for your CI build](/tutorials/ci/hub-amazon-ecr-custom-ci-image/)

The basic building block for building a Docker image as part of your CI workflow is shown in the steps below. To learn how to implement the scenarios above, please click on the either scenario.

* Add an [account integration for Amazon ECR](/integrations/imageRegistries/ecr/). This is required because your Dockerfile likely pulls an image from ECR and we need credentials to do the pull on your behalf.

* Customize the following snippet and include it in the `shippable.yml` for your project:

```
integrations:
  hub:
    - integrationName: ecr-integration
      type: ecr
      branches:
        only:
          - master
          - dev
```
- `integrationName` value is the name of the ECR integration you added in the previous step. It is important the name matches exactly. If not, the build will fail with an error as [described here](/ci/troubleshoot/#integration-name-specified-in-yml-does-not-match).
- `type` is `docker`.
- [optional] `branches` section: specify the branches this integration is applicable to. You can skip this if you want your integration to be applicable for all branches.. The `only` tag should be used when you want the integration on specific branches. You can also use the `except` tag to exclude specific branches.

* You can configure `shippable.yml` to build a container as part of your workflow by including the following:

```
build:
  ci:
    - docker build -t aws-account-id.dkr.ecr.us-east-1.amazonaws.com/image-name:image-tag .
```
You can replace your aws account id, region, image-name, and image-tag as required in the snippet above. The `docker build` command will work in the `pre_ci`, `ci`, `post_ci`, `on_success` and `on_failure` sections of your yml.
