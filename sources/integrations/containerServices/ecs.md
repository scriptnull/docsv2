page_title: Amazon EC2 Container Service integrations
page_description: Setting up Shippable account integrations for Amazon EC2 Container Service
page_keywords: amazon, ecs, gke, kubernetes, engine, google, shippable, quay, coreos, docker, registry, EC2 Container Service, Google, Docker Cloud, Datacenter, private

# Container Services integration

Shippable lets you easily deploy your Dockerized applications to popular Container Services like Amazon EC2 Container Service (ECS), Google Container Engine (GKE), or Red Hat OpenShift 3.

You will first need to configure an account integration with your credentials and/or keys in order to interact with these services using Shippable Pipelines.

Integrations on Shippable work as described in the [Integrations overview](../overview/). This section describes how you can add your ECS integration to your account. To actually use it for CI or Pipelines, you will also need to add this integration to the Subscription that contains your CI or Pipelines.

Follow the steps below to create an account integration with AWS ECS.

##Step 1: Create a user with access to ECS resources

On the [IAM console](https://console.aws.amazon.com/iam/) do following:

- Click on `Users`.
- Click `Create New User` and enter the following:  
     - User Name: `shippable-user-to-allow-ecs-access` (You can specify an alternate name as well.)
     - Ensure the 'Generate an access key for each user' option is checked.
     - Click the `Create` button.
     - Download the User Security Credentials (or copy them)
     - Click `Close`.

The credentials (Access Key/Secret Key pair) for this user will be used for creating an Account Integration on Shippable.

**If the user you just created has admin access to your ECS resources, [you can skip to Step 5](#createAccountInt).**

##Step 2: Attach a policy providing access to ECS resources

If the user you created in step 1 does not have admin access to your ECS resources, you will need to attach a policy that provides this access.

To do this, follow the steps below on the [IAM console](https://console.aws.amazon.com/iam/):

  - Click on `Users`.
  - Click on the user `shippable-user-to-allow-ecs-access`
  - In the 'Permissions' tab, click 'Inline Policies' dropdown and create a new one by clicking the `Click here` button.
  - Select 'Custom Policy' and click the `Select` button.
  - Name the policy as `shippable-user-policy-for-ecs-access` and add the following in the 'policy document'  

```
{
  "Statement": [
        {
            "Version": "2012-10-17",
            "Effect": "Allow",
            "Action": [
                "elasticloadbalancing:ConfigureHealthCheck",
                "elasticloadbalancing:DescribeLoadBalancers",
                "elasticloadbalancing:DeleteLoadBalancerListeners",
                "elasticloadbalancing:CreateLoadBalancerListeners",
                "iam:ListServerCertificates",
                "iam:ListRoles",
                "iam:PassRole",
                "ec2:DescribeRegions",
                "ec2:DescribeInstances",
                "ecs:DescribeClusters",
                "ecs:DescribeTasks",
                "ecs:ListClusters",
                "ecs:ListTaskDefinitions",
                "ecs:RegisterTaskDefinition",
                "ecs:DeregisterTaskDefinition",
                "ecs:DescribeServices",
                "ecs:UpdateService",
                "ecs:DeleteService",
                "ecs:CreateService",
                "ecs:ListTasks",
                "ecs:ListContainerInstances",
                "ecs:DescribeContainerInstances"

            ],
            "Resource": "*"
        }
    ]
}
```

- Click `Validate Policy` button to ensure the syntax and the indentation are correct.
- Click `Apply Policy` button.

The user `shippable-user-to-allow-ecs-access` now has permissions to access Amazon ECS and Load balancer resources.

**If you are not planning to use a Load Balancer for your deployments, [you can now skip to Step 5](#createAccountInt).**

##Step 3. Create an AWS IAM Role with access to ECS resources (Load Balancer scenarios only)  

This Role will be used by Shippable when creating a service on ECS. On the [IAM console](https://console.aws.amazon.com/iam/) do following:

- Click on `Roles`.
- Click `Create New Role` and enter the following:  
     - Role Name: `shippable-role-to-allow-ecs-access`
     - Role type: Select `AWS Service Roles`
     - Scroll down and select `Amazon EC2 Container Service Role`.
     - Select the checkbox for `AmazonEC2ContainerServiceRole`
     - Click `Next Step`.
     - Review and click `Create Role`

Amazon has a pre-existing policy called `AmazonEC2ContainerServiceRole` is being attached to the Role here. It contains everything that the role needs to allow the ECS agent to control the load balancing.

##Step 4 Update the `Trust Relationships` of the Shippable Role (Load Balancer scenarios only)  

- From the IAM console, navigate to and click the Role `shippable-role-to-allow-ecs-access`.
- Go to the 'Trust Relationships' tab.
- Click `Edit Trust Relationship` button.
- Ensure the Policy Document looks like the following:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "",
      "Effect": "Allow",
      "Principal": {
        "Service": "ecs.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

This gives the `ecs` agent running on each ECS instance(s) permissions to talk to the Load balancers

NOTE: Refer [the official Amazon documentation](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/service_IAM_role.html) on how to set up this special role and policy

<a name="createAccountInt"></a>
##Step 5. Create Shippable Account Integration

1. Click on the gear icon for Account Settings in your top navigation bar and then click on the 'Integrations' section.
2. Click on the `Add Integration` button.
3. For 'Integration type', choose `AWS` from the list of dropdown choices.
4. For 'Integration Name' use a distinctive name that's easy to associate to the integration and recall. Example: `AWS-Integration`.
5. Enter your access and secret keys provided by AWS. [See here](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_root-user.html#id_root-user_manage_add-key) for info on how to generate them.
6. Click on `Save`.

<img src="/ci/images/integrations/containerServices/ecs/addAwsInt.png" alt="Amazon AWS integration" style="width:700px;"/>

Please remember that before you can use this integration to set up your Pipelines, you will need to enable the integration for the Subscription that contains your Pipelines.

To do this:

* Go to your Subscription's Settings page and click on `Integrations` in the sidebar menu
* Click on `Add integration`
* Name your integration, choose the account integration you just created above, and click on `Save`  

You can now use this integration to set up your Environment and Deployment Pipelines on your ECS clusters.

For more information on this, please check out our [Deployment pipelines section](/pipelines/overview/).

---

##Deleting the Integration

To remove the ECS integration, you'll need to remove this integration from all dependencies configured to use it. To find all the dependencies:

1. Click on the gear icon for Account Settings in your top navigation bar and then click on the `Integrations` section.
2. Select the ECS integration from the list of integrations. If you have many entries, use the `Integration Name` field to provide the name of your ECS integration.
3. Your ECS integration shows up in the list.
4. Click on the `Delete` button.
5. A window pops up confirming that you want to delete the integration. This window lists all dependencies of this this integration. The list will include any project, environment or subscription image dependent on this integration.
6. If there are dependencies, individually access the `Settings` tab for each project/environment/subscription image and delete the ECS integration.
7. Once all dependencies of the ECS integration have been removed, Step 5 will show the message: `No dependency`.
8. Click the `Yes` button to delete the ECS Integration.

--------
