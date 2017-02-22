page_title: Unified Pipeline Jobs - runSh
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
    always:                                     #optional
      - script: echo 'This block executes if the TASK section succeeds or fails'
      - NOTIFY: slackNotification


```

* `name` should be set to something that describes what the job does and is easy to remember. This will be the display name of the job in your pipeline visualization.
* `type` indicates type of job. In this case it is always `runSh`
* `on_start` can be used to send a notification indicating the job has started running.
* `steps` section is where the steps of your custom job should be entered. You can have any number of `IN` and `OUT` resources depending on what your job needs. You can also have one `TASK` section where you can enter one or more of your custom scripts. Keep in mind that everything under the `steps` section executes sequentially.
* `on_success` can be used to run scripts that only execute if the `TASK` section executes successfully. You can also use this to send a notification as shown in the example above. The `NOTIFY` tag is set to a [Slack notification resource](../resources/notification/).
* `on_failure` can be used to run scripts that only execute if the `TASK` section fails. You can also use this to send a notification as shown in the example above. The `NOTIFY` tag is set to a [Slack notification resource](../resources/notification/).
* `always` can be used to run scripts that only execute if the `TASK` section succeeds or fails. You can also use this to send a notification as shown in the example above. The `NOTIFY` tag is set to a [Slack notification resource](../resources/notification/).

## runSh environment variables
These variables are available for use in the TASK section(s) of every runSh job.

| Env variable        | Description           |
| ------------- |-------------|
| RESOURCE_ID    | The ID of the runSh job. |
| JOB_NAME    | The name of the runSh job. |
| JOB_TYPE    | The type of the runSh job; this will always be `runSh`. |
| BUILD_ID    | The ID of the currently running build. |
| BUILD_NUMBER    | An alternate ID for the build. |
| BUILD_JOB_ID    | The ID of the currently running buildJob. |
| BUILD_JOB_NUMBER    | The number of the currently running buildJob, within the build. This is always `1`. |
| SUBSCRIPTION_ID    | ID of the subscription. |
| JOB_PATH    | The path of the directory containing files for the runSh job. This directory contains two subdirectories: `state` (contains the files that will be saved when the job is complete), and `previousState` (contains the files saved in the previous build). |
| JOB_STATE      | The location of the `state` directory for this job, which contains the files that will be saved when the job is complete. |
| JOB_PREVIOUS_STATE | The location of the directory containing the `state` information from when the job last ran. |
| JOB_MESSAGE    | The path of the file containing the message for this job. |
| *JOBNAME*_NAME    | The name of the job, as given in `shippable.jobs.yml`. For a runSh job named `myRunShJob`, the variable name would be `MYRUNSHJOB_NAME` and the value would be `myRunShJob`.|
| *JOBNAME*_TYPE    | The same as `JOB_TYPE`. For a runSh job named `myRunShJob`, the variable name would be `MYRUNSHJOB_TYPE` and the value would be `runSh`. |
| *JOBNAME*_PATH    | The same directory as `JOB_PATH`. For a runSh job named `myRunShJob`, the variable name would be `MYRUNSHJOB_PATH`. This directory contains two subdirectories: `state` (contains the files that will be saved when the job is complete), and `previousState` (contains the files saved in the previous build). |
| *JOBNAME*_STATE    | The same as `JOB_STATE` with a different variable name. *JOBNAME* is the name of the job, converted to uppercase, with any characters that are not letters, numbers, or underscores removed. |
| *JOBNAME*_PREVIOUS_STATE |The same as `JOB_PREVIOUS_STATE` with a different variable name. *JOBNAME* is the name of the job, converted to uppercase, with any characters that are not letters, numbers, or underscores removed. |
| *JOBNAME*_MESSAGE  | The same as `JOB_MESSAGE` with a different variable name. *JOBNAME* is the name of the job, converted to uppercase, with any characters that are not letters, numbers, or underscores removed.  |

### Resource variables
These variables are added to the environment for all `IN` and `OUT` resources defined for the runSh job. In this case, jobs used as inputs to the runSh job are also considered to be "resources". *RESOURCENAME* is the name of the `IN` or `OUT` resource, in uppercase, with any characters other than letters, numbers, and underscores removed.

| Environment variable        | Description           |
| ------------- |-------------|
| *RESOURCENAME*_NAME    | The name of the resource. |
| *RESOURCENAME*_ID    | The ID of the resource. |
| *RESOURCENAME*_TYPE    | The type of the resource. For example: `image` or `manifest`. |
| *RESOURCENAME*_PATH    | The directory containing files for the resource. |
| *RESOURCENAME*_STATE    | The directory containing saved files for the resource. If the resource is a `gitRepo`, this is the directory containing the cloned repository. If it's a `params` resource, this path will be the location of a file with the parameters. Jobs will have the files saved when the job ran in this directory. |
| *RESOURCENAME*_META    | The directory with files containing information about the resource, such as the version in use and the integration if there is one.  |
| *RESOURCENAME*_OPERATION    | The operation of the resource; either `IN` or `OUT`. |
| *RESOURCENAME*_VERSIONNAME    | For `image` resources, this is the tag. For `version` resources and `release` jobs, this is the semantic version. Not used for other resource types. |
| *RESOURCENAME*_VERSIONNUMBER    | The number of the version of the resource being used. |
| *RESOURCENAME*_VERSIONID    | The ID of the version of the resource being used. |
| *RESOURCENAME*_SOURCENAME    | For resources with pointer sections in the yml, this is the sourceName defined in the pointer. |
| *RESOURCENAME*\_SEED\_VERSIONNAME    | If a resource has a seed section in the yml, this is the versionName defined in the seed. |
| *RESOURCENAME*\_PARAMS\_*FIELDNAME*    | Only added for `param`-type resources. Each field defined in that resource's `params` section in the yml is added as an environment variable. The field name is sanitized in the same way as the resource name. Objects and arrays are stringified JSON. Secure variables are decrypted. |
| *RESOURCENAME*\_INTEGRATION\_*FIELDNAME*    | Explained in the next section. |


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


### gitRepo resource variables
If an `IN` resource is a [gitRepo](../resources/gitRepo.md), the following environment variables are added. The environment variables reflect the commit SHA information contained in the version of the `gitRepo` resource. In the following table, *RESOURCENAME* is the name of the `gitRepo` resource, in uppercase, with any characters other than letters, numbers, and underscores removed.

| Environment variable             | Description                                                |
|----------------------------------|------------------------------------------------------------|
| *RESOURCENAME*_BASE_BRANCH       | If the version was created for a pull request, this is the name of the base branch into which the pull request changes will be merged. |
| *RESOURCENAME*_BRANCH            | When the version was created for a commit, this is the name of branch on which the commit occurred. If it was created for a pull request, this is the base branch. |
| *RESOURCENAME*_COMMIT            | SHA of the most recent commit. |
| *RESOURCENAME*_COMMIT_MESSAGE    | Commit message for the most recent commit. |
| *RESOURCENAME*_COMMITTER         | Name of the last committer. |
| *RESOURCENAME*_GIT_TAG_NAME      | For git tags, the name of the tag. This env variable is currently supported for GitHub only. |
| *RESOURCENAME*_HEAD_BRANCH       | This is only set for pull requests and is the name of the branch the pull request was opened from. |
| *RESOURCENAME*_IS_GIT_TAG        | Set to true if the version was created for a git tag. If not, this will be set to false. This env variable is currently supported for GitHub only. |
| *RESOURCENAME*_IS_RELEASE        | Set to true if the version was created for a release. If not, this will be set to false. This env variable is currently supported for GitHub only. |
| *RESOURCENAME*_KEYPATH           | Path to the ssh keyfile associated with the gitRepo. This is the key that is used to clone the repo |
| *RESOURCENAME*_PULL_REQUEST      | Pull request number if the version was created for a pull request. If not, this will be set to false. |
| *RESOURCENAME*_RELEASE_NAME      | If the version was created for a release, this is the release title. This env variable is currently supported for GitHub only. |
| *RESOURCENAME*_RELEASED_AT       | This is only set for releases, and is the timestamp when the release was published. This env variable is currently supported for GitHub only. |


### integration resource variables
If an `IN` resource is an [integration](../resources/integration.md), the following environment variables could be added, depending on the account integration type associated with the `integration` resource.  In the following table, *RESOURCENAME* is the name of the `integration` resource, in uppercase, with any characters other than letters, numbers, and underscores removed.

| Environment variable        | Account integration type | Description                                |
|-----------------------------|--------------------------|--------------------------------------------|
| *RESOURCENAME*_KEYPATH      | ssh-key or pem-key       | points directly to the private key file. |

<a name="advancedRunSh"></a>
##runSh scenarios

* [Using integrations with a runSh job](../../tutorials/pipelines/runShUseIntegrations.md)
* [Updating resource versions](../../tutorials/pipelines/updateResourceVersion.md)
* [Extracting resource version information](../../tutorials/pipelines/extractVersionInformation.md)
* [Persisting state between runs](../../tutorials/pipelines/persistStateBetweenRuns.md)
