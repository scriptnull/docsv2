page_title: Amazon EC2 Container Service integrations
page_description: Setting up Shippable account integrations for Amazon EC2 Container Service
page_keywords: amazon, ecs, gke, kubernetes, engine, google, shippable, quay, coreos, docker, registry, EC2 Container Service, Google, Docker Cloud, Datacenter, private

# Container Services integration

Shippable lets you easily deploy your Dockerized applications to popular Container Services like Amazon EC2 Container Service (ECS).

You will first need to configure an account integration with your credentials and/or keys in order to interact with these services using Shippable Pipelines.

Integrations on Shippable work as described in the [Integrations overview](../overview/). This section describes how you can add your ECS integration to your account. To actually use it for CI or Pipelines, you will also need to add this integration to the Subscription that contains your CI or Pipelines.

Follow the steps below to create an account integration with AWS ECS.

##Step 1: Create a user with access to ECS resources

On the [IAM console](https://console.aws.amazon.com/iam/) do following:

- Click on `Roles`.
- Click `Create New Role` and do the following:
      - Select **New Role** and add **Role Name** as: `shippable-role-to-allow-ecs-access`
      -   Role for Cross Account Access ->
         - **Allows IAM users from a 3rd party AWS account to access this account.** ->
         - **Account Id**: < shippable aws account id >
         - **External Id**: < users shippable account id >
      - Next Step -> **Create Role**

The credentials (Access Key/Secret Key pair) for this user will be used for creating an Account Integration on Shippable.

**If the user you just created has admin access to your ECS resources, [you can skip to Step 5](#createAccountInt).**

##Step 2: Attach a policy providing access to ECS resources

If the user you created in step 1 does not have admin access to your ECS resources, you will need to attach a policy that provides this access.

To do this, follow the steps below on the [IAM console](https://console.aws.amazon.com/iam/):

- Select the role `shippable-role-to-allow-ecs-access`
-  Then go to **Permissions** -> **Inline Policies** -> **Custom Policy**.
  -  Name the policy as `shippable-policy-to-access-ecs` and add following policy document

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
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
                "ecs:ListClusters",
                "ecs:RegisterTaskDefinition",
                "ecs:DeregisterTaskDefinition",
                "ecs:ListTaskDefinitions",
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

Now configure Shippable to use this role to access the account

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

Now configure Shippable to use this role to access the account

<a name="createAccountInt"></a>
##Step 5. Create Shippable Account Integration

1. From your [Shippable dashboard](https://app.shipable.com), Click on the gear icon for Account Settings in your top navigation bar and then click on the `Integrations` tab. Click on 'Add Integration'
2. **Integration type:** In the dropdown, select `AWS (IAM)`
3. **Integration Name:** Use a distinctive name that's easy to associate to the integration and recall. Example: `manishas-aws-iam`
4. Enter the ARN for the role `shippable-role-to-allow-ecs-access`. This will be a string with format like this `arn:aws:iam::12345678912:role/shippable-role-to-allow-ecs-access`
5. Click `Save`

<img src="/ci/images/integrations/containerServices/ecs/addAwsIamInt.png" alt="Amazon AWS-IAM integration" style="width:700px;"/>

## Amazon EC2 Container Service Using Account Keys

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
