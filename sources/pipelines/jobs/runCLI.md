page_title: Unified Pipeline Jobs
page_description: List of supported jobs
page_keywords: Deploy multi containers, microservices, Continuous Integration, Continuous Deployment, CI/CD, testing, automation, pipelines, docker, lxc


# runCLI
This job combines the unlimited flexibility of a runSh job with the convenience of automatic environment preparation. A runCLI job lets you run any custom scripts, as described in [runSh](./runSh/). Additionally, when you add [cliConfig](../resources/cliConfig/) resources as inputs to this job, the relevant CLI tools will automatically be installed and configured. runCLI jobs also contain the `shippable_replace` tool, a command-line tool that replaces placeholders in files with values from the environment.


## Anatomy of a runCLI job

The following sample shows the overall structure of a runCLI job:

```
jobs:
  - name: <name>                                #required
    type: runCLI                                #required
    on_start:                                   #optional
      - script: echo 'This block executes when the TASK starts'
      - NOTIFY: slackNotification
    steps:                                      #required
      - IN: <resource>                          #specify at least one cliConfig resource to set up CLI tools
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
* `type` indicates type of job. In this case it is always `runCLI`.
* `on_start` can be used to send a notification indicating the job has started running.
* `steps` section is where the steps of your custom job should be entered. You can have any number of `IN` and `OUT` resources depending on what your job needs. If an `IN` resource is a [cliConfig](../resources/cliConfig/), we will use the integration specified in the `cliConfig` to configure the relevant CLI tools for you. You can also have one `TASK` section where you can enter one or more of your custom scripts. Keep in mind that everything under the `steps` section executes sequentially.
* `on_success` can be used to run scripts that only execute if the `TASK` section executes successfully. You can also use this to send a notification as shown in the example above. The `NOTIFY` tag is set to a [Slack notification resource](../resources/notification/).
* `on_failure` can be used to run scripts that only execute if the `TASK` section fails. You can also use this to send a notification as shown in the example above. The `NOTIFY` tag is set to a [Slack notification resource](../resources/notification/).
* `always` can be used to run scripts that only execute if the `TASK` section succeeds or fails. You can also use this to send a notification as shown in the example above. The `NOTIFY` tag is set to a [Slack notification resource](../resources/notification/).

## Configured tools
The runCLI job uses the subscription integration specified in the [cliConfig](../resources/cliConfig/) to determine which CLI tools to configure. These tools are configured with the credentials contained in the [subscription integration](../../navigatingUI/subscriptions/settings/#adding-integrations). Below is a list of the tools configured for each integration type.

| Integration Type                    | Configured Tools           |
| ------------------------------------|-------------|
| AWS                                 | [AWS CLI](https://aws.amazon.com/cli/); [EB (Elastic Beanstalk) CLI](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb-cli3.html) |
| Amazon EC2 Container Registry (ECR) | [Docker Engine](https://docs.docker.com/engine/reference/commandline/docker/) |
| Docker Hub                          | [Docker Engine](https://docs.docker.com/engine/reference/commandline/docker/) |
| Docker Trusted Registry             | [Docker Engine](https://docs.docker.com/engine/reference/commandline/docker/) |
| Google Container Engine             | [gcloud](https://cloud.google.com/sdk/gcloud/); [kubectl](https://kubernetes.io/docs/user-guide/kubectl/) |
| Google Container Registry (GCR)     | [Docker Engine](https://docs.docker.com/engine/reference/commandline/docker/) |
| JFrog Artifactory                   | [JFrog CLI](https://www.jfrog.com/confluence/display/CLI/CLI+for+JFrog+Artifactory) |
| Kubernetes                          | [kubectl](https://kubernetes.io/docs/user-guide/kubectl/) |
| Private Docker Registry             | [Docker Engine](https://docs.docker.com/engine/reference/commandline/docker/) |
| Quay.io                             | [Docker Engine](https://docs.docker.com/engine/reference/commandline/docker/) |

It's worth noting that if a runCLI job receives multiple `cliConfig` resources for the same provider, we will use the credentials contained in the last listed `cliConfig` when authenticating.

## shippable_replace
When you create a `runCLI` job, the `shippable_replace` command-line tool is automatically installed. `shippable_replace` looks for placeholders in one or more files, then replaces the placeholders with the values of the corresponding environment variables.

`shippable_replace` is used as follows:
```
shippable_replace [paths to files]
```

The file(s) passed to `shippable_replace` may contain one or more placeholders for environment variables. Placeholders are strings of the form $ENVIRONMENT_VARIABLE_NAME or ${ENVIRONMENT_VARIABLE_NAME}. We recommend ${ENVIRONMENT_VARIABLE_NAME}, as it is less likely to cause errors if you have an environment variable name that is a substring of another environment variable's name.

For example, say you wanted to insert the current job name into two different files: `file1.json` and `test/file2.txt`. The name of the environment variable containing the job name is `JOB_NAME`. (See the [runSh documentation](./runSh/) for the list of available environment variables.)

`file1.json` might look like this:
```
{
  "foo": "bar",
  "jobName": "${JOB_NAME}"
}
```
`test/file2.txt` contains this:
```
This job, ${JOB_NAME}, is the coolest job ever.
```
`shippable_replace` is then added to the task section of the `runCLI` job:
```
jobs:
  - name: myRunCLIJob
    type: runCLI
    steps:
      - IN: <resource>
      - TASK:
        - script: shippable_replace file1.json test/file2.txt
```

After `shippable_replace`, `file1.json` now looks like this:
```
{
  "foo": "bar",
  "jobName": "myRunCLIJob",
  "fizz": "buzz"
}
```
And `test/file2.txt` looks like this:
```
This job, myRunCLIJob, is the coolest job ever.
```

## Resource variables
Variables are added to the environment for all `IN` and `OUT` resources defined for the runCLI job. The environment variables are the same as those available for [runSh](./runSh/) jobs.
