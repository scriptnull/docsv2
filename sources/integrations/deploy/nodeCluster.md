page_title: Node Cluster integration
page_description: Setting up Node Cluster integrations on Shippable
page_keywords: shippable, cluster, node, aws, digital ocean, azure

# Node Cluster

A Node cluster is one or more nodes, to which Shippable could deploy your artifacts via its [Unified Pipelines](../../pipelines/overview/).

The nodes (VMs) could be from any cloud provider or self hosted. Shippable interacts with the nodes using IP Address and SSH keys.

##Adding the Account Integration.

You will need to configure this integration to deploy artifacts to a cluster of nodes.

* Click on the gear icon for Account Settings in your top navigation bar and then click on the 'Integrations' section.
* Click on the `Add Integration` button.
* For 'Integration type', choose `Node Cluster` from the list of choices.
* For 'Integration Name' use a distinctive name that's easy to associate to the integration and recall. Example: `app-cluster`
* Enter the IP address of the nodes.
* Choose the subscriptions, in which the integration can be accessible.
* Click on `Save`

<img src="/ci/images/integrations/deploy/nodeCluster/addNodeClusterInt.png" alt="Node Cluster" style="width:700px;"/>

* After saving, a script will be generated, which is used for creating a user called `shippable` and adding a public SSH key on all the nodes specified in the list.
* Copy the script and run it on all of the nodes.
* Click on `Done`, once the script is executed on all of the nodes.

<img src="/ci/images/integrations/deploy/nodeCluster/addNodeClusterIntSSH.png" alt="Node Cluster" style="width:700px;"/>

##Deployments to Node Cluster

Node cluster is used as an integration in [Unified Pipelines](../../pipelines/overview/) of Shippable. So, please make sure to get basic idea about it ( if you haven't ) and continue reading this guide.

* After creating the Node Cluster integration, it could be mentioned as an integration for [cluster](../../pipelines/resources/cluster/) resources.

```
# shippable.resources.yml
resources:
  - name: app-cluster
    type: cluster
    integration: app-cluster
    pointer:
      sourceName: 'application-cluster'
```

* [file](../../pipelines/resources/file/) type resources could be deployed using this integration. For a list of available artifact registries, from which the file could be pulled, please check the [file resource documentation](../../pipelines/resources/file/)

```
  - name: app-package
    type: file
    pointer:
      sourceName: http://example.com/src/package.html
    seed:
      versionName: initial
```
* Input the file resource to a [manifest](../../pipelines/jobs/manifest/) job and create a deploy job having input as the manifest and [cluster](../../pipelines/resources/cluster/) resource.
```
jobs:
  - name: app-manifest
    type: manifest
    steps:
      - IN: app-package

  - name: app-deploy
    type: deploy
    steps:
      - IN: app-manifest
        force: true # force deploys, when there are no changes in the yml configurations
      - IN: app-cluster
```
* When the `manifest` job is run, it generates a manifest and gives it as input to the deploy job. The deploy job, downloads the artifact and transfers it to the nodes (/tmp/shippable directory).

<img src="/ci/images/integrations/deploy/nodeCluster/useNodeClusterInt.png" alt="Node Cluster" style="width:700px;"/>

##Deleting the Integration

To remove the Node Cluster integration, you'll need to remove this integration from all dependencies configured to use it. To find all the dependencies:

1. Click on the gear icon for Account Settings in your top navigation bar and then click on the `Integrations` section.
2. Select the Node Cluster integration from the list of integrations. If you have many entries, use the Filters dropdown and select Node Cluster. Alternatively, you can use the Integration Name field to provide the name of your Node Cluster integration.
3. Your Node Cluster integration shows up in the list.
4. Click on the `Delete` button.
5. A window pops up confirming that you want to delete the integration. This window lists all dependencies of this this integration. The list will include any Subscription dependent on this integration.
6. If there are dependencies, individually access the Settings tab for each Subscription and delete the Node Cluster integration.
7. Once all dependencies of the Node Cluster integration have been removed, Step 5 will show the message: `No dependency`.
8. Click the Yes button to delete the Node Cluster Integration.
