#Pushing a Docker image to Amazon ECR

You can push your image to Amazon ECR in any section of your yml.  

To do this:

* Add an [account integration for ECR](/integrations/imageRegistries/ecr/)

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

* Add the following to your `shippable.yml` file:

```
build:
  post_ci:
    - docker push aws-account-id.dkr.ecr.us-east-1.amazonaws.com/image-name:image-tag
```

You can replace your myOrg, myImageRepo, and myTag as required in the snippet above.

If you also want to build the image as part of your CI workflow, check out the tutorial for [building a Docker image](/tutorials/ci/hub-amazon-ecr-build-docker-image/).

###Pushing the CI container with all artifacts intact

If you are pushing your CI container to ECR and you want all build artifacts preserved, you should commit the container before pushing it as shown below:

```
build:
  post_ci:
    #Commit the container only if you want all the artifacts from the CI step
    - docker commit $SHIPPABLE_CONTAINER_NAME aws-account-id.dkr.ecr.us-east-1.amazonaws.com/image-name:image-tag
    - docker push aws-account-id.dkr.ecr.us-east-1.amazonaws.com/image-name:image-tag

```
The environment variable `$SHIPPABLE_CONTAINER_NAME` contains the name of your CI container.

###Pushing Docker images with multiple tags

If you want to push the container image with multiple tags, you can just push twice as shown below:


```
build:
  post_ci:
    #Commit the container only if you want all the artifacts from the CI step
    - docker push aws-account-id.dkr.ecr.us-east-1.amazonaws.com/image-name:myTag1
    - docker push aws-account-id.dkr.ecr.us-east-1.amazonaws.com/image-name:myTag2

```
