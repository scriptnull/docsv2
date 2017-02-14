page_title: Kubernetes integrations
page_description: Setting up Shippable account integrations for Google's Kubernetes
page_keywords: amazon, ecs, gke, kubernetes, engine, google, shippable, quay, coreos, docker, registry, EC2 Container Service, Google, Docker Cloud, Datacenter, private

# Kubernetes integration

Shippable lets you easily deploy your Dockerized applications to popular Container Services like Kubernetes, Amazon EC2 Container Service (ECS), Google Container Engine (GKE), Docker Cloud/Datacenter, and Joyent Triton.

You will first need to configure an account integration with your credentials and/or keys in order to interact with these services using Shippable Pipelines.

Integrations on Shippable work as described in the [Integrations overview](../overview/). This section describes how you can add your Kubernetes integration to your account.

Follow the steps below to create an account integration with Google's Kubernetes. The steps below assume you already have a Kubernetes cluster which you want to configure for deployment with Shippable.

<a name="createAccountInt"></a>
## Creating the Shippable Account Integration (Kubernetes master has a public IP address)

* Click on the gear icon for Account Settings in your top navigation bar and then click on the 'Integrations' section.
* Click on the `Add Integration` button and choose `Kubernetes` from the list of choices.
* For 'Integration Name' use a distinctive name that's easy to associate to the integration and recall. Example: `kube-int`.
* 'Cluster Access type' should be set to `Kubernetes master`
* Copy the contents of your [kubeconfig](https://kubernetes.io/docs/user-guide/kubeconfig-file/) file, usually found at ~/.kube/config, that contains credentials for accessing your Kubernetes cluster. You may copy the entire kubeconfig file or only the details related to a particular cluster
* Paste the contents into the 'KubeConfig File' textbox.
* Assign this integration to the Subscription(s) containing the repo with your pipelines config. Since you're likely a member of many organizations, you need to specify which of them can use this integration.
* Click on `Save`.

<img src="/ci/images/integrations/containerServices/kubernetes/kubernetes-integration.png" alt="Google Kubernetes integration" style="width:500px;"/>

You can now use this integration in your pipeline YML config to deploy to your Kubernetes pods.

For more information on this, please check out our docs on [Deployment pipelines](/pipelines/overview/), [deploy job](/pipelines/jobs/deploy/) and [cluster resource](/pipelines/resources/cluster/).

---

## Creating the Shippable Account Integration (Kubernetes master does not have a public IP address)

If your Kubernetes master node is not publicly accessible from the internet, Shippable's hosted service cannot communicate with it. To get around this problem, we need you to configure a Bastion host which is publicly accessible and can also communicate with the Kubernetes master node.

The requirement for Bastion host are:
* Bastion host should have IP address and port 22 open for SSH access
* You can SSH into the Bastion host
* Bastion host can access the Kubernetes master node, i.e. it contains the private SSH key
* Bastion host has kubectl installed. Install it with the following script:

```
curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl

```

Now, you can create the Shippable account integration:

* Click on the gear icon for Account Settings in your top navigation bar and then click on the 'Integrations' section.
* Click on the `Add Integration` button and choose `Kubernetes` from the list of choices.
* For 'Integration Name' use a distinctive name that's easy to associate to the integration and recall. Example: `kube-int`.
* 'Cluster Access type' should be set to `Bastion Host`.
* SSH into the Bastion host and from there, SSH into your Kubernetes master node and run the following commands. Copy the output of this file which gives you the [kubeconfig](https://kubernetes.io/docs/user-guide/kubeconfig-file/).
```
$ sudo su -
$ cat /etc/kubernetes/admin.conf
```
* Paste the contents into the 'KubeConfig File' textbox.
* Assign this integration to the Subscription(s) containing the repo with your pipelines config. Since you're likely a member of many organizations, you need to specify which of them can use this integration.
* Click on `Save`.
* You will see a script section which contains a script you need to run on your Bastion host. Run the script and then click on `Done`.

<img src="/ci/images/integrations/containerServices/kubernetes/kubernetes-bastion-integration.png" alt="Google Kubernetes integration" style="width:500px;"/>

You can now use this integration in your pipeline YML config to deploy to your Kubernetes pod.

For more information on this, please check out our docs on [Deployment pipelines](/pipelines/overview/), [deploy job](/pipelines/jobs/deploy/) and [cluster resource](/pipelines/resources/cluster/).

---

##Deleting the Integration

To remove the Kubernetes integration, you'll need to remove this integration from all dependencies configured to use it. To find all the dependencies:

* Click on the gear icon for Account Settings in your top navigation bar and then click on the `Integrations` section.
* Click on the `Delete` button against your Kubernetes integration.
* A window pops up confirming that you want to delete the integration. This window lists all dependencies of this this integration.
* If there are dependencies, individually access the `Settings` tab for each Subscription and delete the Kubernetes integration.
* Once all dependencies have been removed, Step 3 will show the message: `No dependency`.
* Click the `Yes` button to delete the Integration.

--------
