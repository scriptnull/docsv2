
#Pull a Docker image from GCR

You can pull any Docker image as long as you have access to it on Google Container Registry and use it during your CI workflow.

To do this, follow the steps below:

* Add an [account integration for GCR](/integrations/imageRegistries/gcr/) if you don't already have it.

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

* Add the `docker pull` command to any section of your yml to pull your image from GCR:

```
build:
  pre_ci:
    -  docker pull gcr.io/myOrg/myImageRepo:myTag

```
You can replace your myOrg, myImageRepo, myTag as required in the snippet above. The `docker pull` command will work in the `pre_ci`, `ci`, `post_ci`, `on_success` and `on_failure` sections of your yml.

For more information on docker pull syntax and options, please [refer to the Docker docs](https://docs.docker.com/engine/reference/commandline/pull/).
