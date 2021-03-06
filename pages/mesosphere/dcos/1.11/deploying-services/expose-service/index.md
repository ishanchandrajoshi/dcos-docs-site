---
layout: layout.pug
navigationTitle:  Exposing a Service
title: Exposing a Service
menuWeight: 5
excerpt: Launching a service with a Marathon app definition

enterprise: false
---


DC/OS agent nodes can be designated as [public](/mesosphere/dcos/1.11/overview/concepts/#public-agent-node) or [private](/mesosphere/dcos/1.11/overview/concepts/#private-agent-node) during [installation](/mesosphere/dcos/1.11/installing/). Public agent nodes provide access from outside of the cluster via infrastructure networking to your DC/OS services. By default, services are launched on private agent nodes and are not accessible from outside the cluster.

To launch a service on a public node, you must create a Marathon app definition with the `"acceptedResourceRoles":["slave_public"]` parameter specified and configure an edge load balancer and service discovery mechanism.

**Prerequisites:**

* DC/OS is [installed](/mesosphere/dcos/1.11/installing/)
* DC/OS CLI is [installed](/mesosphere/dcos/1.11/cli/install/)

1.  Create a Marathon app definition with the required `"acceptedResourceRoles":["slave_public"]` parameter specified. For example:

    ```json
    {
       "id":"/product/service/myApp",
       "acceptedResourceRoles":[
          "slave_public"
       ],
       "instances":1,
       "cpus":0.1,
       "mem":64,
       "networks":[
          {
             "mode":"container/bridge"
          }
       ],
       "container":{
          "type":"DOCKER",
          "docker":{
             "image":"group/image"
          },
          "portMappings":[
             {
                "containerPort":8080,
                "hostPort":0
             }
          ]
       }
    }
    ```

    For more information about the `acceptedResourceRoles` parameter, see the Marathon API [documentation](/mesosphere/dcos/1.11/deploying-services/marathon-api/).

1.  Add your app to Marathon by using this command, where `myApp.json` is the file containing your Marathon app definition.

    ```bash
    dcos marathon app add myApp.json
    ```

    If this is added successfully, there is no output. You can also add your app by using the **Services** tab of the DC/OS [web interface](/mesosphere/dcos/1.11/gui/services/).

1.  Verify that the app is added with this command:

    ```bash
    dcos marathon app list
    ```

    The output should look like this:

    ```bash
     ID     MEM  CPUS  TASKS  HEALTH  DEPLOYMENT  CONTAINER  CMD
    /myApp   64  0.1    0/1    ---      scale       DOCKER   None
    ```

    You can also view deployed apps by using the **Services** tab of the DC/OS [web interface](/mesosphere/dcos/1.11/gui/services/).

1.  Configure an edge load balancer and service discovery mechanism.

    - AWS users: If you installed DC/OS by using the [AWS CloudFormation templates](/mesosphere/dcos/1.11/installing/evaluation/community-supported-methods/aws/), an ELB is included. However, you must reconfigure the health check on the public ELB to expose the app to the port specified in your app definition (e.g. port 80).
    - All other users: You can use [Marathon-LB](/mesosphere/dcos/services/marathon-lb/1.12/), a rapid proxy and load balancer that is based on HAProxy.

1.  Go to your public agent to see the site running and to find your public agent IP. Type it into your browser.

    You should see the following message in your browser:

    ![Hello Brave World](/mesosphere/dcos/1.11/img/helloworld.png)

    Figure 1. Confirmation page

## Next steps

Learn how to load balance your app on a public node using [Marathon-LB](/mesosphere/dcos/services/marathon-lb/1.12/mlb-basic-tutorial/).
