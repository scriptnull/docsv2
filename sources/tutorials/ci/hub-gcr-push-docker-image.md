#Pushing a Docker image to GCR

You can push your image to GCR in any section of your yml.  

To do this:

* Add an [account integration for GCR](/integrations/imageRegistries/gcr/)

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

* Add the following to your `shippable.yml` file:

```
build:
  post_ci:
    - docker push gcr.io/myOrg/myImageRepo:myTag
```

You can replace your myOrg, myImageRepo, and myTag as required in the snippet above.

###Pushing the CI container with all artifacts intact

If you are pushing your CI container to GCR and you want all build artifacts preserved, you should commit the container before pushing it as shown below:

```
build:
  post_ci:
    #Commit the container only if you want all the artifacts from the CI step
    - docker commit $SHIPPABLE_CONTAINER_NAME gcr.io/myOrg/myImageRepo:myTag
    - docker push gcr.io/myOrg/myImageRepo:myTag

```

The environment variable `$SHIPPABLE_CONTAINER_NAME` contains the name of your CI container.
