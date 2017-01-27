page_title: Unified Pipeline Jobs
page_description: List of supported jobs
page_keywords: Deploy multi containers, microservices, Continuous Integration, Continuous Deployment, CI/CD, testing, automation, pipelines, docker, lxc


# runSh
This job type lets you run any custom scripts as part of your deployment pipeline. Depending on what you want to achieve in your script(s), you can specify input and output resources. You can also send notifications for specific events, like job start, success, or failure.

This is the most powerful job type since it is not a *managed* job; i.e. you write the scripts yourself so you have unlimited flexibility. You should use this job type if there is no managed job that provides the functionality you need. For example, pushing to Heroku is not yet natively supported through a managed job type, so you can write the scripts needed to do this and add it to your pipeline as a job of type `runSh`.

We are actively adding managed job types to reduce the need to use `runSh`. If you have a scenario that is not handled by our managed jobs, [open a support request](https://github.com/Shippable/support/issues/) and we'll look into adding a managed job for it.

## Anatomy of a runSh job

The following sample shows the overall structure of a runSh job:

```
jobs:
  - name: <name>                                #required
    type: runSh                                 #required
    on_start:                                   #optional
      - script: echo 'This block executes when the TASK starts'
      - NOTIFY: slackNotification
    steps:                                      #required
      - IN: <resource>
      - IN: <resource>
      - TASK:
        - script: <command>
        - script: <command>
      - OUT: <resource>
      - OUT: <resource>
    on_success:                                 #optional
      - script: echo 'This block executes after the TASK section executes successfully'
      - NOTIFY: slackNotification
    on_failure:                                 #optional
      - script: echo 'This block executes if the TASK section fails'
      - NOTIFY: slackNotification

```

* `name` should be set to something that describes what the job does and is easy to remember. This will be the display name of the job in your pipeline visualization.
* `type` indicates type of job. In this case it is always `runSh`
* `on_start` can be used to send a notification indicating the job has started running.
* `steps` section is where the steps of your custom job should be entered. You can have any number of `IN` and `OUT` resources depending on what your job needs. You can also have one or more `TASK` sections where you can enter one or more of your custom scripts. Keep in mind that everything under the `steps` section executes sequentially. Also, environment variables do not persist across `TASK` sections. Read our [advanced runSh documentation](#advancedRunSh) below to find out how to persist environment variables across `TASK` sections.
* `on_success` can be used to run scripts that only execute if the `TASK` section executes successfully. You can also use this to send a notification as shown in the example above. The `NOTIFY` tag is set to a [Slack notification resource](../resources/notification/).
* `on_failure` can be used to run scripts that only execute if the `TASK` section fails. You can also use this to send a notification as shown in the example above. The `NOTIFY` tag is set to a [Slack notification resource](../resources/notification/).

## runSh environment variables
These variables are available for use in the TASK section(s) of every runSh job.

| Env variable        | Description           |
| ------------- |-------------|
| RESOURCE_ID    | The ID of the runSh job.|
| JOB_NAME    | The name of the runSh job.|
| JOB_TYPE    | The type of the runSh job; this will always be `runSh`.|
| BUILD_ID    | The ID of the currently running build.|
| BUILD_NUMBER    | An alternate ID for the build.|
| BUILD_JOB_ID    | The ID of the currently running buildJob.|
| BUILD_JOB_NUMBER    | The number of the currently running buildJob, within the build. This is always `1`.|
| SUBSCRIPTION_ID    | ID of the subscription.|
| *JOBNAME*_PATH    | The path of the directory containing files for the runSh job. For a runSh job named `myRunShJob`, the variable name would be `MYRUNSHJOB_PATH`. This directory contains two subdirectories: `state` (contains the files that will be saved when the job is complete), and `previousState` (contains the files saved in the previous build).|

### Resource variables
These variables are added to the environment for all `IN` and `OUT` resources defined for the runSh job. In this case, jobs used as inputs to the runSh job are also considered to be "resources". *RESOURCENAME* is the name of the `IN` or `OUT` resource, in uppercase, with any characters other than letters, numbers, and underscores removed.

| Environment variable        | Description           |
| ------------- |-------------|
| *RESOURCENAME*_NAME    | The name of the resource.|
| *RESOURCENAME*_ID    | The ID of the resource.|
| *RESOURCENAME*_TYPE    | The type of the resource. For example: `image` or `manifest`.|
| *RESOURCENAME*_PATH    | The directory containing files for the resource.|
| *RESOURCENAME*_OPERATION    | The operation of the resource; either `IN` or `OUT`.|
| *RESOURCENAME*_VERSION_VERSIONNAME    | For `image` resources, this is the tag. For `version` resources and `release` jobs, this is the semantic version. Not used for other resource types.|
| *RESOURCENAME*_VERSION_VERSIONNUMBER    | The number of the version of the resource being used.|
| *RESOURCENAME*_VERSION_VERSIONID    | The ID of the version of the resource being used.|
| *RESOURCENAME*\_VERSION\_*FIELDNAME*    | `dockerOptions`-, `params`-, and `replicas`-type resources have a `version` section in the yml. Fields defined in these resources' `version` sections are added as environment variables. The field name is sanitized in the same way as the resource name. Objects and arrays are stringified JSON.|
| *RESOURCENAME*\_POINTER\_*FIELDNAME*    | Each field defined in that resource's `pointer` section in the yml is added as an environment variable. The field name is sanitized in the same way as the resource name. Objects and arrays are stringified JSON.|
| *RESOURCENAME*\_SEED\_*FIELDNAME*    | Each field defined in that resource's `seed` section in the yml is added as an environment variable. The field name is sanitized in the same way as the resource name. Objects and arrays are stringified JSON.|
| *RESOURCENAME*\_PARAMS\_*FIELDNAME*    | Only added for `param`-type resources. Each field defined in that resource's `params` section in the yml is added as an environment variable. The field name is sanitized in the same way as the resource name. Objects and arrays are stringified JSON. Secure variables are decrypted.|
| *RESOURCENAME*\_INTEGRATION\_*FIELDNAME*    | Explained in the next section.|


### Resource integration variables
When an `IN` resource uses a subscription integration, the fields associated with that integration are added as environment variables. In the following table, *RESOURCENAME* is the name of the `IN` resource, in uppercase, with any characters other than letters, numbers, and underscores removed. The available environment variables for each integration are as follows:

| Account Integration type                | Environment variables                          |
|-----------------------------------------|------------------------------------------------|
| Amazon ECR                              | *RESOURCENAME*_INTEGRATION_AWS_ACCESS_KEY_ID <br/> *RESOURCENAME*_INTEGRATION_AWS_SECRET_ACCESS_KEY |
| AWS                                     | *RESOURCENAME*_INTEGRATION_AWS_ACCESS_KEY_ID <br/> *RESOURCENAME*_INTEGRATION_AWS_SECRET_ACCESS_KEY <br/> *RESOURCENAME*_INTEGRATION_URL |
| Amazon Web Services (IAM)               | *RESOURCENAME*_INTEGRATION_ASSUMEROLEARN <br/> *RESOURCENAME*_INTEGRATION_OUTPUT <br/> *RESOURCENAME*_INTEGRATION_URL |
| Azure Container Service                 | *RESOURCENAME*_INTEGRATION_USERNAME <br/> *RESOURCENAME*_INTEGRATION_URL |
| Bitbucket                               | *RESOURCENAME*_INTEGRATION_URL <br/> *RESOURCENAME*_INTEGRATION_TOKEN |
| Bitbucket Server                        | *RESOURCENAME*_INTEGRATION_USERNAME <br/> *RESOURCENAME*_INTEGRATION_URL <br/> *RESOURCENAME*_INTEGRATION_TOKEN |
| Docker Hub                              | *RESOURCENAME*_INTEGRATION_USERNAME <br/> *RESOURCENAME*_INTEGRATION_PASSWORD <br/> *RESOURCENAME*_INTEGRATION_EMAIL |
| Docker Cloud                            | *RESOURCENAME*_INTEGRATION_USERNAME <br/> *RESOURCENAME*_INTEGRATION_URL <br/> *RESOURCENAME*_INTEGRATION_TOKEN |
| Docker Datacenter                       | *RESOURCENAME*_INTEGRATION_USERNAME <br/> *RESOURCENAME*_INTEGRATION_URL <br/> *RESOURCENAME*_INTEGRATION_PASSWORD |
| Docker Trusted Registry                 | *RESOURCENAME*_INTEGRATION_USERNAME <br/> *RESOURCENAME*_INTEGRATION_URL <br/> *RESOURCENAME*_INTEGRATION_PASSWORD <br/> *RESOURCENAME*_INTEGRATION_EMAIL|
| Email                                   | *RESOURCENAME*_INTEGRATION_EMAILADDRESS|
| Event Trigger                           | *RESOURCENAME*_INTEGRATION_PROJECT <br/> *RESOURCENAME*_INTEGRATION_WEBHOOKURL <br/> *RESOURCENAME*_INTEGRATION_AUTHORIZATION |
| GCR                                     | *RESOURCENAME*_INTEGRATION_JSON_KEY|
| GitHub                                  | *RESOURCENAME*_INTEGRATION_URL <br/> *RESOURCENAME*_INTEGRATION_TOKEN |
| GitHub Enterprise                       | *RESOURCENAME*_INTEGRATION_URL <br/> *RESOURCENAME*_INTEGRATION_TOKEN |
| Gitlab                                  | *RESOURCENAME*_INTEGRATION_URL <br/> *RESOURCENAME*_INTEGRATION_TOKEN |
| Google Container Engine                 | *RESOURCENAME*_INTEGRATION_JSON_KEY <br/> *RESOURCENAME*_INTEGRATION_URL |
| Hipchat                                 | *RESOURCENAME*_INTEGRATION_TOKEN |
| Jenkins                                 | *RESOURCENAME*_INTEGRATION_USERNAME <br/> *RESOURCENAME*_INTEGRATION_PASSWORD <br/> *RESOURCENAME*_INTEGRATION_URL |
| JFrog Artifactory                       | *RESOURCENAME*_INTEGRATION_USERNAME <br/> *RESOURCENAME*_INTEGRATION_PASSWORD <br/> *RESOURCENAME*_INTEGRATION_URL |
| Joyent Triton Public Cloud              | *RESOURCENAME*_INTEGRATION_USERNAME <br/> *RESOURCENAME*_INTEGRATION_URL <br/> *RESOURCENAME*_INTEGRATION_VALIDITYPERIOD <br/> *RESOURCENAME*_INTEGRATION_PUBLICKEY <br/> *RESOURCENAME*_INTEGRATION_PRIVATEKEY| <br/> *RESOURCENAME*_INTEGRATION_CERTIFICATES |
| Kubernetes                              | *RESOURCENAME*_INTEGRATION_CLUSTERACCESSTYPE <br/> *RESOURCENAME*_INTEGRATION_MASTERKUBECONFIGCONTENT <br/> *RESOURCENAME*_INTEGRATION_BASTIONKUBECONFIGTYPE <br/> *RESOURCENAME*_INTEGRATION_BASTIONKUBECONFIGCONTENT <br/> *RESOURCENAME*_INTEGRATION_BASTIONHOSTIP <br/> *RESOURCENAME*_INTEGRATION_BASTIONPRIVATEKEY <br/> *RESOURCENAME*_INTEGRATION_BASTIONPUBLICKEY |
| Node Cluster                            | *RESOURCENAME*_INTEGRATION_NODES <br/> *RESOURCENAME*_INTEGRATION_PUBLICKEY <br/> *RESOURCENAME*_INTEGRATION_PRIVATEKEY|
| PEM key                                 | *RESOURCENAME*_INTEGRATION_KEY |
| Private Docker registry                 | *RESOURCENAME*_INTEGRATION_USERNAME <br/> *RESOURCENAME*_INTEGRATION_URL <br/> *RESOURCENAME*_INTEGRATION_PASSWORD <br/> *RESOURCENAME*_INTEGRATION_EMAIL |               |
| Quay.io                                 | *RESOURCENAME*_INTEGRATION_USERNAME <br/> *RESOURCENAME*_INTEGRATION_URL <br/> *RESOURCENAME*_INTEGRATION_PASSWORD <br/> *RESOURCENAME*_INTEGRATION_EMAIL <br/> *RESOURCENAME*_INTEGRATION_ACCESSTOKEN |
| Slack                                   | *RESOURCENAME*_INTEGRATION_WEBHOOKURL |
| SSH key                                 | *RESOURCENAME*_INTEGRATION_PUBLICKEY <br/> *RESOURCENAME*_INTEGRATION_PRIVATEKEY |


<a name="advancedRunSh"></a>
##runSh scenarios

* [Using integrations with a runSh job](../../tutorials/pipelines/runShUseIntegrations.md)
* [Updating resource versions](../../tutorials/pipelines/updateResourceVersion.md)
* [Extracting resource version information](../../tutorials/pipelines/extractVersionInformation.md)
* [Persisting state between runs](../../tutorials/pipelines/persistStateBetweenRuns.md)
