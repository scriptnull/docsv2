page_title: Using integrations with your runSh job
page_description: Continuous deployment tutorials
page_keywords: containers, lxc, docker, Continuous Integration, Continuous Deployment, CI/CD, testing, automation, Tokens, account settings

#Using integrations with your runSh job
The `runSh` job type is a custom job that lets you run any custom scripts as part of your deployment pipeline. For scenarios where you want to interact with a supported third party service, you will need to add an integration to your custom job.

For example, if you want to push to a Docker registry or pull a private Docker image from a registry, you must add an integration for the Docker registry. For more on Integrations, read our [Integrations overview page.](/integrations/overview/)

There are two ways to add an integration to your custom job. The first is to create an `integration`-type resource, and add it to the custom job. The second is by adding other resource types as inputs to your custom job; any integrations associated with those resources will be automatically added to your job.

### Specifying an integration resource as an input
To add an integration resource to your custom job, you first define it in your `shippable.resources.yml`:

```
resources:
  - name: dockerCredentials                #required
    type: integration                      #required
    integration: myDockerHubIntegration    #required
```

Next, specify the integration resource as an input to the custom job in your `shippable.jobs.yml`:

```
jobs:
  - name: myCustomJob
    type: runSh
    steps:
      - IN: dockerCredentials
      - TASK:
        - script: ./doStuffWithIntegration.sh
```

### Adding integrations associated with other resource types
When a resource is added as an input to a custom job, the integration associated with the resource is also added. This works for all types of resources. To add a resource as an input, you first define it in your `shippable.resources.yml`:
```
resources:
  - name: myImage
    type: image
    integration: myDockerHubIntegration
    pointer:
      sourceName: library/busybox
    seed:
      versionName: latest
```

Then, add your image resource in your `shippable.jobs.yml`:

```
jobs:
  - name: myCustomJob
    type: runSh
    steps:
      - IN: myImage
      - TASK:
        - script: ./doStuffWithIntegration.sh
```

### Using integration data
In both of the above examples, the integration `myDockerHubIntegration` was added to the runSh job. (In the first example, it was added to `myCustomJob` as part of the `dockerCredentials` resource; in the second, it was added because it is associated with the `myImage` resource.) The fields from `myDockerHubIntegration` are all made available as environment variables to the scripts in the runSh job. For a list of variables created for each integration, read our [runSh page](/pipelines/jobs/runSh/#resource-integration-variables).

In this case, there are three environment variables created for this Docker Hub integration. Their names will be:

- *RESOURCENAME*_INTEGRATION_USERNAME
- *RESOURCENAME*_INTEGRATION_PASSWORD
- *RESOURCENAME*_INTEGRATION_EMAIL

where *RESOURCENAME* is the name of the resource added to the custom job. In the first example, *RESOURCENAME* will be `DOCKERCREDENTIALS`; in the second example, *RESOURCENAME* will be `MYIMAGE`.

We'll assume we added `dockerCredentials` to the custom job, so the *RESOURCENAME* is `DOCKERCREDENTIALS`. Our `doStuffWithIntegration.sh` script can now use the `myDockerHubIntegration` environment variables to log into Docker Hub:
```
dockerLogin() {
  docker login -u $DOCKERCREDENTIALS_INTEGRATION_USERNAME -p $DOCKERCREDENTIALS_INTEGRATION_PASSWORD
}
```
