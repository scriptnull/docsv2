
#Pull a Docker image from Amazon ECR

You can pull any Docker image as long as you have access to it on Amazon's EC2 Container Registry (ECR) and use it during your CI workflow.

To do this, follow the steps below:

* Add an [account integration for Amazon ECR](/integrations/imageRegistries/ecr/) if you don't already have it.

* Customize the following snippet and include it in the `shippable.yml` for your project:

```
integrations:
  hub:
    - integrationName: ecr-integration
      type: ecr
      region: us-east-1
      branches:
        only:
          - master
          - dev
```
- `integrationName` value is the name of the ECR integration you added in the previous step. It is important the name matches exactly. If not, the build will fail with an error as  [described here](/ci/troubleshoot/#integration-name-specified-in-yml-does-not-match).
- `type` is `ecr`.
- [optional] `region` specifies the region you want to push to. Default is us-east-1.
- [optional] `branches` section: specify the branches this integration is applicable to. You can skip this if you want your integration to be applicable for all branches.. The `only` tag should be used when you want the integration on specific branches. You can also use the `except` tag to exclude specific branches.

* Add the `docker pull` command to any section of your yml to pull your Docker image from ECR:

```
build:
  pre_ci:
    - docker pull aws-account-id.dkr.ecr.us-east-1.amazonaws.com/image-name:image-tag

```
You can replace your aws account id, region, image-name, and image-tag as required in the snippet above. The `docker pull` command will work in the `pre_ci`, `ci`, `post_ci`, `on_success` and `on_failure` sections of your yml.


For more information on docker pull syntax and options, please [refer to the Docker docs](https://docs.docker.com/engine/reference/commandline/pull/).
