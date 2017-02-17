page_title: Unified Pipeline Resources - cliConfig
page_description: List of supported resources
page_keywords: Deploy multi containers, microservices, Continuous Integration, Continuous Deployment, CI/CD, testing, automation, pipelines, docker, lxc

#cliConfig

A cliConfig resource contains the information required to configure a command line utility and is used as an input to [runCLI jobs](../jobs/runCLI/). Multiple cliConfigs may be used in a single runCLI job, but in most cases only one resource should be specified for each third party service.

You can create an cliConfig resource by adding it to `shippable.resources.yml`

```
resources:
  - name: <string>
    type: cliConfig
    integration: <string>
    pointer:
      region: <string>               #required for some integration types
      clusterName: <string>          #optional
```

* `name` should be an easy to remember text string. This will appear in the visualization of this resource in the SPOG view. It is also used to refer to this resource in the `shippable.jobs.yml`. If you have spaces in your name, you'll need to surround the value with quotes, however, as a best practice we recommend not including spaces in your names.

* `type` is always set to 'cliConfig'.

* `integration` should be the name of the integration that connects to the third party platform or service you want to be configured in your [runCLI jobs](../jobs/runCLI/).

* `region` is the AWS region or Google Cloud zone, for example 'us-east-1' or 'europe-west1-c.' It is required when `integration` is an [AWS](../../integrations/deploy/eb/) or [Amazon EC2 Container Registry (ECR)](../../integrations/imageRegistries/ecr/) integration and is optional for [Google Container Engine (GKE)](../../integrations/containerServices/gke/) integrations.

* `clusterName` is optional when using a [Google Container Engine (GKE)](../../integrations/containerServices/gke/) integration. This should be the name of a cluster on GKE. If a `clusterName` is given, `gcloud` and `kubectl` will be configured to use that cluster. Specifying a `clusterName` without a `region` is not supported.


## Supported Integrations

The following integrations are supported:

### Amazon EC2 Container Registry (ECR)

An [Amazon EC2 Container Registry (ECR)](../../integrations/imageRegistries/ecr/) integration may be used to authenticate with ECR in a [runCLI job](../jobs/runCLI/), where it will configure the AWS keys, set the region, and authenticate with `docker` before the `TASK` section. In this case, `aws` and `eb` will be configured as well as `docker`. It is specified as follows:

```
resources:
  - name: <string>                              #required
    type: cliConfig                             #required
    integration: <string>                       #required
    pointer:
      region: <string>                          #required
```

* `name` should be an easy to remember text string. This will appear in the visualization of this job in the SPOG view.
* `type` is 'cliConfig'
* `integration` should be the name of an [Amazon EC2 Container Registry (ECR)](../../integrations/imageRegistries/ecr/) integration that has been [enabled for the subscription](../../navigatingUI/subscriptions/settings/#adding-integrations). Make sure that this name matches the name used in the subscription settings.
* `region` is required for this type of integration. It should be the AWS region that you intend to interact with, for example 'us-east-1.'

*NOTE:* Using multiple Amazon EC2 Container Registry (ECR) integrations or both ECR and AWS integrations with different credentials or regions in the same runCLI job is not supported.

### AWS

An [AWS](../../integrations/deploy/eb/) integration may be specified as follows in order to use the `aws` and `eb` command line utilities. This will set the AWS access keys and the region in the environment before the `TASK` section is run in a [runCLI job](../jobs/runCLI/).

```
resources:
  - name: <string>                              #required
    type: cliConfig                             #required
    integration: <string>                       #required
    pointer:
      region: <string>                          #required
```

* `name` should be an easy to remember text string. This will appear in the visualization of this job in the SPOG view.
* `type` is 'cliConfig'
* `integration` should be the name of an [AWS](../../integrations/deploy/eb/) integration that has been [enabled for the subscription](../../navigatingUI/subscriptions/settings/#adding-integrations). Make sure that this name matches the name used in the subscription settings.
* `region` is required for this type of integration. It should be the AWS region that you intend to interact with, for example 'us-east-1.'

*NOTE:* Using multiple AWS integrations or both AWS and Amazon EC2 Container Registry (ECR) integrations in the same runCLI job is not supported.

### Docker

A [Docker](../../integrations/imageRegistries/dockerHub/) integration is used to authenticate with Docker Hub and will be used in a [runCLI job](../jobs/runCLI/) to `docker login` before the scripts in the `TASK` section. It may be created in `shippable.resources.yml` as follows:

```
resources:
  - name: <string>                              #required
    type: cliConfig                             #required
    integration: <string>                       #required
```

* `name` should be an easy to remember text string. This will appear in the visualization of this job in the SPOG view.
* `type` is 'cliConfig'
* `integration` should be the name of a [Docker](../../integrations/imageRegistries/dockerHub/) integration that has been [enabled for the subscription](../../navigatingUI/subscriptions/settings/#adding-integrations). Make sure that this name matches the name used in the subscription settings.

### Docker Trusted Registry

A [Docker Trusted Registry](../../integrations/imageRegistries/dockerTrustedRegistry/) integration is used to authenticate with that registry and if this resource is added to a [runCLI job](../jobs/runCLI/), it will be used to `docker login` before the scripts in the `TASK` section. It may be created in `shippable.resources.yml` as follows:

```
resources:
  - name: <string>                              #required
    type: cliConfig                             #required
    integration: <string>                       #required
```

* `name` should be an easy to remember text string. This will appear in the visualization of this job in the SPOG view.
* `type` is 'cliConfig'
* `integration` should be the name of a [Docker Trusted Registry](../../integrations/imageRegistries/dockerTrustedRegistry/) integration that has been [enabled for the subscription](../../navigatingUI/subscriptions/settings/#adding-integrations). Make sure that this name matches the name used in the subscription settings.

### Google Container Registry (GCR)

A [Google Container Registry (GCR)](../../integrations/imageRegistries/gcr/) integration may be used to authenticate with GCR in a [runCLI job](../jobs/runCLI/) before the scripts in the `TASK` section. It is specified as follows:

```
resources:
  - name: <string>                              #required
    type: cliConfig                             #required
    integration: <string>                       #required
```

* `name` should be an easy to remember text string. This will appear in the visualization of this job in the SPOG view.
* `type` is 'cliConfig'
* `integration` should be the name of a [Google Container Registry (GCR)](../../integrations/imageRegistries/gcr/) integration that has been [enabled for the subscription](../../navigatingUI/subscriptions/settings/#adding-integrations). Make sure that this name matches the name used in the subscription settings.

*NOTE:* Using multiple Google Container Registry (GCR) integrations or both GCR and Google Container Engine (GKE) integrations in the same runCLI job is not supported.

### Google Container Engine (GKE)

A [Google Container Engine (GKE)](../../integrations/containerServices/gke/) integration may be used to authenticate with GKE in a [runCLI job](../jobs/runCLI/) before the scripts in the `TASK` section. The integration will be used to authenticate with `gcloud`. It is specified as follows:

```
resources:
  - name: <string>                              #required
    type: cliConfig                             #required
    integration: <string>                       #required
    pointer:
      region: <string>                          #optional
      clusterName: <string>                     #optional
```

* `name` should be an easy to remember text string. This will appear in the visualization of this job in the SPOG view.
* `type` is 'cliConfig'
* `integration` should be the name of a [Google Container Engine (GKE)](../../integrations/containerServices/gke/) integration that has been [enabled for the subscription](../../navigatingUI/subscriptions/settings/#adding-integrations). Make sure that this name matches the name used in the subscription settings.
* `region` is the zone that should be configured for 'compute/zone' in `gcloud`. It is required if `clusterName` is specified and optional otherwise.
* `clusterName` is the name of a GKE cluster. If one is specified, `region` is also required and 'container/cluster' will be set in `gcloud` and credentials configured for `kubectl`.

*NOTE:* Using multiple Google Container Engine (GKE) integrations, GKE and Kubernetes integrations, or GKE and Google Container Registry (GCR) integrations in the same runCLI job is not supported.

### JFrog Artifactory

A [JFrog Artifactory](../../integrations/artifactRegistries/jfrogArtifactory/) integration may be used to configure credentials with the JFrog Artifactory CLI in a [runCLI job](../jobs/runCLI/) before the scripts in the `TASK` section. It is specified as follows:

```
resources:
  - name: <string>                              #required
    type: cliConfig                             #required
    integration: <string>                       #required
```

* `name` should be an easy to remember text string. This will appear in the visualization of this job in the SPOG view.
* `type` is 'cliConfig'
* `integration` should be the name of a [JFrog Artifactory](../../integrations/artifactRegistries/jfrogArtifactory/) integration that has been [enabled for the subscription](../../navigatingUI/subscriptions/settings/#adding-integrations). Make sure that this name matches the name used in the subscription settings.

### Kubernetes

A [Kubernetes](../../integrations/containerServices/kubernetes/) integration may be used to authenticate with the Kubernetes master in a [runCLI job](../jobs/runCLI/) before the scripts in the `TASK` section. It is specified as follows:

```
resources:
  - name: <string>                              #required
    type: cliConfig                             #required
    integration: <string>                       #required
```

* `name` should be an easy to remember text string. This will appear in the visualization of this job in the SPOG view.
* `type` is 'cliConfig'
* `integration` should be the name of a [Kubernetes](../../integrations/containerServices/kubernetes/) integration that has been [enabled for the subscription](../../navigatingUI/subscriptions/settings/#adding-integrations). Make sure that this name matches the name used in the subscription settings. Kubernetes integrations using a bastion host are not supported with [runCLI jobs](../jobs/runCLI/) at this time.

*NOTE:* Using multiple Kubernetes integrations or both Kubernetes and Google Container Engine (GKE) integrations in the same runCLI job is not supported.

### Private Docker Registry

A [Private Docker Registry](../../integrations/imageRegistries/privateRegistry/) integration is used to authenticate with that registry and will be used to `docker login` before the scripts in the `TASK` section in a [runCLI job](../jobs/runCLI/). It may be created in `shippable.resources.yml` as follows:

```
resources:
  - name: <string>                              #required
    type: cliConfig                             #required
    integration: <string>                       #required
```

* `name` should be an easy to remember text string. This will appear in the visualization of this job in the SPOG view.
* `type` is 'cliConfig'
* `integration` should be the name of a [Private Docker Registry](../../integrations/imageRegistries/privateRegistry/) integration that has been [enabled for the subscription](../../navigatingUI/subscriptions/settings/#adding-integrations). Make sure that this name matches the name used in the subscription settings.

### Quay.io

A [Quay](../../integrations/imageRegistries/quay/) integration may be used to authenticate with `docker` in a [runCLI job](../jobs/runCLI/) before the scripts in the `TASK` section. It is specified as follows:

```
resources:
  - name: <string>                              #required
    type: cliConfig                             #required
    integration: <string>                       #required
```

* `name` should be an easy to remember text string. This will appear in the visualization of this job in the SPOG view.
* `type` is 'cliConfig'
* `integration` should be the name of a [Quay](../../integrations/imageRegistries/quay/) integration that has been [enabled for the subscription](../../navigatingUI/subscriptions/settings/#adding-integrations). Make sure that this name matches the name used in the subscription settings.
