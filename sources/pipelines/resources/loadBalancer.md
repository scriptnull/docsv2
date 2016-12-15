# loadBalancer

A `loadBalancer` resource is used to deploy your service manifest with a load balancer. Please note that this is only supported for Amazon's EC2 Container Service (ECS) at this time. It is used as an input for [deploy jobs](../jobs/deploy/).

You can create a `loadBalancer` resource by adding it to `shippable.resources.yml`:

```
resources:
  - name: <string>                              #required
    type: loadBalancer                          #required
    pointer:
      sourceName: "<string>"
      method: application | classic
      role: <string>                            #optional

```

* `name` should be an easy to remember text string. This will appear in the visualization of this resource in the SPOG view and the list of resources in the Pipelines `Resources` tab. It is also used to refer to this resource in the jobs yml. If you have spaces in your name, you'll need to surround the value with quotes, however, as a best practice we recommend not including spaces in your names.

* `type` is always set to 'loadBalancer'.

* `pointer` section provides information about the load balancer:
    * `sourceName` should be set depending on the type of load balancer. It is good practice to surround this field with quotes to avoid any parsing issues due to special characters.
        * For <a href="https://aws.amazon.com/elasticloadbalancing/classicloadbalancer/" target="_blank">Classic Load Balancers</a>, set this to the load balancer name
        * For <a href="https://aws.amazon.com/elasticloadbalancing/applicationloadbalancer/" target="_blank">Application Load Balancers</a>, set it to the target group arn  
    * `method` is set to `application` for Application Load Balancers and `classic` for Classic Load Balancers
    * `role` is set to the name of the AWS IAM role used to update the load balancer. This role should have a trust relationship allowing "ecs.amazonaws.com". If no role is specified, a role with the correct trust relationship will be used if one exists.
