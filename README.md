# Practical OpenShift for Developers Labs Repository

Welcome to the Labs repository for the Practical OpenShift for Developers course. In this repository, you will find
resources, tutorials, and hands-on exercises to help you learn and master OpenShift, a powerful container platform.


## Hand-on-Openshift

To log in to OpenShift and retrieve your access token, you can follow these steps:

Access the OpenShift web console or command line interface.

Obtain your access token from the web console. The process may vary depending on your OpenShift environment. Look for an
option to retrieve or view your access token.

Open a command line terminal.

Run the following command, replacing YOUR_OCP_IP with the IP address or hostname of your OpenShift cluster and
YOUR_TOKEN with the access token you obtained in the previous step:

```shell
oc login https://YOUR_OCP_IP:8443 --token=YOUR_TOKEN
```

This command will establish a connection to your OpenShift cluster using the specified IP address and access token.

After successfully logging in, you can use the oc whoami or os whoami command to retrieve the username associated with
your current session in the OpenShift cluster

## Creating Pods from files

To create a Pod on OpenShift based on a YAML file, use the following command:
Go to the folder labs\pods

```text
oc create -f pods/pod.yaml
```

Show all currently running Pods

```text
oc get pods
```

```shell
oc status
```

```shell
oc create -f pods/pod.yaml
```

Replace pods/pod.yaml with the path to your specific YAML file.
To show all currently running Pods in OpenShift, you can run:

```shell
oc get pods
```

## Get Pod Documentation

Get built-in documentation for Pods

```text
oc explain pod
```

Get details on the pod's spec

```text
oc explain pod.spec
```

Get details on the pod's containers

```text
oc explain pod.spec.containers
```

The oc command-line tool is used to interact with OpenShift clusters from the command line. It provides various
functionalities to manage resources, including querying and manipulating YAML files.
To explain the path to a specific property in a YAML file using oc, you need to identify the keys or fields that lead to
the desired property. In your case, the keys are spec, containers, and env, and the desired property is located at
spec.containers.env in the YAML file.

You can use the following command to extract information about the env property within the YAML file:

```shell
oc explain pod.spec.containers.env
```

This command will provide you with details about the env property, including its type (an array of objects), and the
fields for the objects within the array (such as name and value).

By following this approach, you can explore and understand the structure of complex resources like deployment configs or
any other OpenShift YAML definition. It allows you to retrieve information about specific properties and understand the
overall structure of the YAML file.

## DeploymentConfig

A DeploymentConfig in OpenShift is a resource that provides advanced automation and configuration options for managing
pods effectively. It extends the functionality of individual pods and allows for more controlled and automated
deployments. Here are some key points about DeploymentConfig:

* Unlike Kubernetes, DeploymentConfig is specific to OpenShift and provides additional features and capabilities
  tailored
  for OpenShift environments.
* To access comprehensive documentation about DeploymentConfig and its properties, you can use the:

```shell
oc explain deploymentconfig
```

command in an OpenShift cluster.

* A DeploymentConfig defines the template for a pod and manages the deployment process, including updating images or
  making configuration changes.
* The template for a pod within a DeploymentConfig follows the same format as regular pods in OpenShift and is specified
  under the template property in the spec section of the DeploymentConfig.
* The `replicas` parameter in the `spec` section of a DeploymentConfig is crucial as it determines the desired number of
  pod
  instances to be running.
*
    * If there are not enough instances running, the DeploymentConfig automatically starts new pods based on the
      template
      until the desired number of replicas is reached.
*
    * If there are excess pods (e.g., when reducing the replicas value), the DeploymentConfig automatically scales down
      by
      deleting pods until reaching the desired number.
* DeploymentConfig also provides other capabilities and behaviors, such as:
*
    * Automatic triggering of new deployments based on changes in the pod template or image.
*
    * Control over deployment strategies, such as rolling updates or blue-green deployments.
*
    * Integration of custom behavior using lifecycle hooks to perform actions during the deployment process.
* DeploymentConfig is typically written in YAML format, allowing you to define and configure its properties and
  behaviors
  in a structured manner.

By leveraging DeploymentConfig in OpenShift, you can effectively manage and automate the deployment of pods and ensure
scalability, reliability, and consistency in your application deployments.

````yaml
apiVersion: apps.openshift.io/v1 # Specifies the API version for the DeploymentConfig.
kind: DeploymentConfig           # Indicates the type of resource, which is a DeploymentConfig.
metadata:
  name: my-app                   # Sets the name of the DeploymentConfig as "my-app".
spec:
  replicas: 3                    # Number of pod replicas to maintain
  selector:
    app: my-app                  # Selector for identifying pods managed by this DeploymentConfig
  template:
    metadata:
      labels:
        app: my-app              # Labels for identifying pods created from this template
    spec:
      containers:
        - name: my-app-container   # Name of the container within the pod
          image: myregistry/my-app-image:latest   # Container image to use
          ports:
            - containerPort: 8080    # Port to expose within the container
          resources:
            limits:
              cpu: "1"             # Maximum CPU limit for the container
              memory: "512Mi"      # Maximum memory limit for the container
            requests:
              cpu: "0.5"           # CPU resource request for the container
              memory: "256Mi"      # Memory resource request for the container
````

## Hands on DeploymentConfig

OpenShift and the oc command line tool offer simple methods for creating deployment configs based on various inputs.

To create a deployment config based on an image tag, the primary command is oc new-app. In this lesson, we'll deploy a
pre-built image using the hello world image from this course. The command is:

````shell
oc new-app quay.io/practicalopenshift/hello-world --as-deployment-config 
````

If you are running it and getting security certification issues:

````shell
oc new-app quay.io/practicalopenshift/hello-world --as-deployment-config --insecure-registry
````

The output of the command shows the plan for creating the deployment config and related resources. It includes the
creation of an image stream tag to track the image, a deployment config, and a service for load balancing. The status of
resource creation is indicated in the plan output, with "created" indicating successful creation.

The message about the application not being exposed is related to OpenShift's networking tiers. To access the
application from outside the OpenShift cluster, you need to create a service and expose it.

Running oc status provides an overview of the resources currently running in your project. It shows the service,
deployment config, image stream tag, and information about the running pod.

To check the running pods created by the deployment config, you can use the oc get pods command.

By running oc new-app and oc status, the deployment config is successfully created and managed, resulting in the running
of a pod for the application.

That concludes this lesson, and we'll continue with the next one.

#### Deleting DeploymentConfig

To delete resources like deployment configs in OpenShift, you can use the oc delete command, just like you did with
pods. When you run oc delete with the deployment config, it will send an HTTP request to the OpenShift REST API using
the DELETE verb.

Before deleting the deployment config, it's recommended to run oc status to ensure that your deployment is still running
and to check the status of your OpenShift cluster. The output of oc status should include the service, deployment
config, image stream tag, and one pod. If these resources are not present, you may need to refer to the previous lesson
and deploy the application using oc new-app.
You can check if the Pod is up and running:

```shell
oc get svc
```

And even though you can check the deployment config.

```shell
oc get dc
```

And you can also check if there is an image stream tag.

```shell
oc get istage
```

Once you have confirmed that everything is running, you can proceed to delete the resources. To delete a service, you
can use the oc delete svc command. Here, "svc" is an abbreviation for service. Run the following command:

```shell
oc delete svc <service-name>
```

Deleting resources one-by-one

```shell
oc delete svc/hello-world
oc delete dc/hello-world
oc delete istag hello-world:version
```

Replace <service-name> with the actual name of your service. This command will delete the specified service.

Remember to replace <service-name> with the actual name of the service you want to delete. You can find the name of the
service by running oc get svc.

You can also use similar commands to delete other types of resources. For example, to delete the deployment config, you
can use oc delete dc <deployment-config-name>, and to delete the image stream tag, you can use oc delete istag <
image-stream-tag-name>.

Make sure to exercise caution when deleting resources, as it cannot be undone.

### Deleting multiples types of resources at once.

Check the project label.

```shell
oc describe dc/hello-world
```

output

```text
Name:           hello-world
Namespace:      myproject
Created:        2 minutes ago
Labels:         app=hello-world
                app.kubernetes.io/component=hello-world
```

```shell
oc delete all -l app=hello-world
```

You can confirm if all the resources was deleted.

```shell
oc status
```

### Naming your DeploymentConfig

Create a DeploymentConfig with a Custom Name

```shell
oc new-app quay.io/practicalopenshift/hello-world --name demo-app --as-deployment-config --insecure-registry
```
```shell
oc new-app quay.io/practicalopenshift/hello-world --name demo-app-2 --as-deployment-config --insecure-registry
```

`--name demo-app`: This flag specifies the custom name you want to assign to the DeploymentConfig. Replace demo-app with
your desired name
Verify the DeploymentConfig Name
```shell
oc describe dc/demo-app
```
```shell
oc delete all -l app=demo-app
```


## Port forwarding for Pods

#### Open a local port that forwards traffic to a pod

```text
oc port-forward <pod name> <local port>:<pod port>
```

#### Example of 8080 to 8080 for hello world

oc port-forward hello-world-pod 8080:8080

## Shell into Pods

oc rsh will work with any Pod name from oc get pods
oc rsh <pod name>
oc rsh getting into a specific running service
oc rsh -c <ex:nginx-proxy> <pod name>

In the shell, check the API on port 8080
wget localhost:8080

Exit the rsh session
exit

Watch live updates to pods
oc get pods --watch

Delete (stop) Pods

Delete any OpenShift resource
oc delete <resource type> <resource name>

(# Delete the pod for this section)
oc delete pod hello-world-pod

```text
Deployment configs define the template for a pod and manage deploying new images or configuration changes.
```

```shell
# Version 3 to 4.4
$ oc new-app quay.io/practicalopenshift/hello-world
 
# Version 4.5+
$ oc new-app quay.io/practicalopenshift/hello-world --as-deployment-config 
```

#### Deploying an image

Deploy an existing image based on its tag
oc new-app <image tag>

For this lesson
oc new-app quay.io/practicalopenshift/hello-world

Check running resources
oc status

Check pods
oc get pods

oc get works with different types of resources

```shell
oc get svc
oc get dc
oc get istag
```

oc delete also works with different types of resources

Describe the DC to get its labels
oc describe dc/hello-world

Delete all application resources using labels
oc delete all -l app=hello-world

#### Deploying an image from git repository

Deploy from Git using oc new-app
oc new-app <git repo URL>

For this lesson
oc new-app https://gitlab.com/practical-openshift/hello-world.git
oc new-app https://github.com/alexfbasa/nginx-demo.git --name nginx-test --as-deployment-config

Follow build progress
oc logs -f bc/hello-world

Check status and pods
oc status
oc get pods

Create a pod based on a file (just like the Pods section)
oc create -f pods/pod.yaml

Create a service for the pod
oc expose --port 8080 pod/hello-world-pod

heck that the service and pod are connected properly
oc status

Create another pod
oc create -f pods/pod2.yaml

Shell into the second pod
oc rsh hello-world-pod-2

Get the service IP and Port
oc status

In the shell, you can make a request to the service (because you are inside the OpenShift cluster)
wget -qO- <service IP : Port>

Inside the pod, get all environment variables
env

Use the environment variables with wget:

```shell
wget -qO- $HELLO_WORLD_POD_PORT_8080_TCP_ADDR:$HELLO_WORLD_POD_PORT_8080_TCP_PORT
```

Create a Route based on a Service
oc expose svc/hello-world

# Get the Route URL

oc status

# Check the route

curl <route from oc status>

# Creating ConfigMaps

# Create a configmap using literal command line arguments

oc create configmap message-map --from-literal MESSAGE="Hello From ConfigMap"

oc get cm/message-map

# Create a ConfigMap using literal command line arguments

oc create configmap <configmap-name> --from-literal KEY="VALUE"

# Create from a file

oc create configmap <configmap-name> --from-file=MESSAGE.txt

# Create from a file with a key override

oc create configmap <configmap-name> --from-file=MESSAGE=MESSAGE.txt

# Same --from-file but with a directory

oc create configmap <configmap-name> --from-file pods

# Verify

oc get -o yaml configmap/<configmap-name>

# Consuming ConfigMaps as Environment Variables

# Set environment variables (same for all types of ConfigMap)

oc set env dc/hello-world --from cm/<configmap-name>
oc set env dc/hello-world --from cm/message-map

## Source-to-Image (S2I)

Source-to-Image (S2I) is a framework that simplifies the process of creating Docker images from application code. With
S2I, you don't need to manually provide instructions in a Dockerfile. Instead, it takes your application code and
generates a reusable Docker image by leveraging pre-built builder images.

For example, in the previous exercise where we deployed the application using the Python catalog item, it automatically
created a build configuration of type Source-to-Image build. S2I utilizes a pre-built Python builder image and injects
your application code into it to create the final application image.

If you need to modify the build strategy, you can edit the build configuration either through the web console interface
or using a YAML file. For example, you can modify the build strategy in the nginx_build-conf.yaml file.

S2I provides a convenient way to streamline the image building process, allowing developers to focus on their
application code while leveraging pre-configured builder images to create container images effortlessly.

```yaml
apiVersion: build.openshift.io/v1
kind: BuildConfig
metadata:
  name: python-app
spec:
  source:
    type: Git
    git:
      uri: https://github.com/your-username/repo-name.git
  strategy:
    type: Source
    sourceStrategy:
      from:
        kind: ImageStreamTag
        name: python:latest
  output:
    to:
      kind: ImageStreamTag
      name: python-app:latest
```

## Custom build

OpenShift provides Image Streams that enable you to use custom build strategies. Image Streams track and manage updates
to an image, allowing you to use different versions of the image. You can create and manage Image Streams through the
OpenShift web console or using YAML files.
