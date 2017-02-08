#Using a custom Docker image from Amazon ECR for your builds

By default, all CI builds on Shippable run on our default Docker images which are preconfigured depending on programming language.

However, you have the option to override the default image and use your own if needed. In most cases, customers choose to do this if they are using Docker for their applications and have a very specific requirements for the build image.

To override the default CI build image and use your own, do the following:

* Add an [account integration for Amazon ECR](/integrations/imageRegistries/ecr/). This is required because your Dockerfile likely pulls an image from ECR and we need credentials to do the pull on your behalf.

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
- [optional] `region` specifies the region you want to pull from. Default is us-east-1.
- [optional] `branches` section: specify the branches this integration is applicable to. You can skip this if you want your integration to be applicable for all branches.. The `only` tag should be used when you want the integration on specific branches. You can also use the `except` tag to exclude specific branches.

* If you want to build a Docker image and then use it for your CI, you should follow instructions in our [Build a Docker image](/tutorials/ci/hub-amazon-ecr-build-docker-image/) tutorial. Please note that you should build the image in the `pre-ci` section of the yml.

* Add the following to your yml:

```
build:
  pre_ci_boot:
    image_name: aws-account-id.dkr.ecr.us-east-1.amazonaws.com/image-name
    image_tag: tip
    pull: true
    options: "-e HOME=/root"

```
- `image_name` value is the fully qualified name of the image you want to pull
- `image_tag` is the tag for the image you want to pull
- set `pull` to `true` if you want to pull the image and `false` if you pulled or built the Docker image in the `pre_ci` step.
- In the `options` tag, enter any docker options you want to use in the docker run command. You also need to include the HOME environment variable as shown if it is not already set in your 'Dockerfile'.

Your build should now pull the custom image and use it to run the CI commands.

###Important note for customers [overriding default image for CI](/ci/shippableyml/#pre-ci-boot)

If you are using a custom image for your CI workflow, we will try to login to ECR on your behalf from inside your CI build container. This means that you will need the AWS Command Line Interface (CLI) installed inside your custom image if you want this to succeed, else you will get a `aws: command not found` error.

You can solve this in 2 ways:

* Set `agent_only: true` for ECR integration in your `shippable.yml`:

```
integrations:
  hub:
    - integrationName: ecr-integration
      type: ecr
      agent_only: true

```

If this tag is set to true, we will not attempt to login to the registry from inside your CI build container. However, this also means that you will only be able to pull from or push to ECR in the `pre_ci` and `push` sections of the yml.

* If you want to use docker commands to interact with ECR in your `ci`, `post_ci`, `on_success` or `on_failure` sections within your `shippable.yml`, then include the following in your 'Dockerfile' to install the AWS CLI:

```
sudo pip install awscli

```
