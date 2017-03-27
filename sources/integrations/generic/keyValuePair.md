page_title: keyValuePair integration
page_description: Custom integrations on Shippable
page_keywords: shippable, custom, integrations, key-value, keyValue


# Key-Value pair

A key-value pair integration is a custom integration where you can give any key-value pairs.

##Adding the Account Integration.

* Click on the gear icon for Account Settings in your top navigation bar and then click on the 'Integrations' section.
* Click on the `Add Integration` button.
* For 'Integration type', choose `Key-Value pair` from the list of choices.
* For 'Integration Name' use a distinctive name that's easy to associate to the integration and recall. Example: `key-value-integration`
* Enter the key-value pairs. A key can contain only letters, digits and underscore. And can not start with a digit.
* Choose the subscriptions, in which the integration can be accessible.
* Click on `Save`

<img src="/ci/images/integrations/generic/keyValuePair/addAccountIntegration.png" alt="Key-Value" style="width:700px;"/>

##Using Key-Value pair integration in CI

To use key-value pair integration in CI builds, you need to add it in your `shippable.yml` under `generic` integrations section. You just need to provide the name of the integration.
```
# shippable.yml
integrations:
  generic:
    - integrationName: "key-value-integration"
```

When you run a build for this project, your key-value pairs will be available as environment variables.

<img src="/ci/images/integrations/generic/keyValuePair/ciJobConsoles.png" alt="Key-Value-jobConsoles" style="width:700px;"/>

##Using Key-Value pair integration in Pipelines

You can only use a key-value pair integration in your [runSh](../..//pipelines/jobs/runSh.md) and [runCLI](../..//pipelines/jobs/runCLI.md) jobs.

* Create an integration resource with key-value pair integration.
```
# shippable.resources.yml
resources:
  - name: key-values
    type: integration
    integration: "key-value-integration"
```

* Add this resource as an `IN` to your `runSh`/`runCLI` job.
```
# shippable.jobs.yml
jobs:
  - name: runSh-job
    type: runSh
    steps:
      - IN: key-values
      - TASK:
        - script: echo $API_KEY
        - script: echo $API_URL
```

When you run a build for this job, your key-value pairs will be available as environment variables.

<img src="/ci/images/integrations/generic/keyValuePair/pipelinesBuildJobConsoles.png" alt="Key-Value-jobConsoles" style="width:700px;"/>
