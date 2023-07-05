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

```shell
oc delete all -l app=demo-app-2
```

## Deployment from Git

In addition to deploying applications from pre-built images, OpenShift allows you to deploy applications directly from
Git repositories. This tutorial will guide you through the process of deploying an ap

```shell
oc new-app https://gitlab.com/practical-openshift/hello-world.git --as-deployment-config --insecure-registry
```

Explanation:

* `oc new-app`: This command creates a new application.
* `https://gitlab.com/practical-openshift/hello-world.git`: Replace this URL with the URL of your Git repository
  containing the application's source code.
* `--as-deployment-config`: This flag specifies that you want to create a DeploymentConfig instead of a basic
  deployment.
  When you use the oc new-app command with a Git repository as the source code, OpenShift follows a series of steps to
  build and deploy the application. Let's break down the process and the output you mentioned:

OpenShift attempts to download the Golang Alpine image specified in the Dockerfile of your Git repository. It does this
by importing the image into its internal registry and creating an image stream tag for it.

OpenShift sets up a trigger for the Golang Alpine image stream tag. This trigger ensures that whenever the Golang Alpine
image stream tag changes, a new build will be initiated.

A Docker build is created using the source code from your Git repository. OpenShift goes through the Dockerfile
instructions step by step, creating intermediate containers for each instruction.

The resulting image from the Docker build is pushed to a new image stream tag.

OpenShift creates a new deployment config named "Hello World" to deploy the application. The deployment config specifies
that the application will be load balanced on port 8080 by a service.

The warning line you mentioned indicates that running the image directly on Golang Alpine may cause security constraints
in OpenShift. However, the "Hello World" image uses the user command, which follows OpenShift standards and complies
with its security controls.

In the creation section of the output, you can see that OpenShift successfully creates the Golang image stream, the "
Hello World" image stream, the "Hello World" build config, the "Hello World" deployment config, and the "Hello World"
service.

The success area of the output mentions that a build was scheduled. Builds in OpenShift are resources that produce
images from source, similar to Docker builds. The build process can be tracked using the oc logs -f command followed by
the build config name.
Check the status of the DeploymentConfig:

```shell
oc describe dc/hello-world
oc logs -f bc/hello-world

oc delete all -l app=hello-world
```

## Git and Openshift

Suppose you have a Git repository with the following file structure:

- nginx-app/
    - Dockerfile
    - index.html

In the Dockerfile a simple Nginx service.

```dockerfile
# Use the Nginx base image
FROM nginx:latest

# Copy the index.html file to the root directory of Nginx
COPY index.html /usr/share/nginx/html
```

You can run your nginx from your git by the command:

```shell
oc new-app https://github.com/your-user/nginx-app.git --as-deployment-config
```

This will create an application in OpenShift based on your Git repository. OpenShift will automatically detect the
Dockerfile and create a build config to build the container image. It will then deploy the application using a
deployment config.

## Networking

OpenShift provides a network abstraction called a "service" to enable network access to Pods. You can use
the `oc explain service` command to get detailed documentation about services in OpenShift.

The concept of a service in OpenShift is an abstraction of a software service. Regardless of the number of Pods running
your application, you only need one service to expose all of them to the network. External applications interacting with
your application are unaware of the underlying Pods and communicate with the service to access it. The service handles
the distribution of traffic among the Pods, providing the abstraction layer.

Services in OpenShift depend on a proxy and a selector. The proxy refers to the internally exposed IP and port that the
service listens on. This IP is only accessible within the OpenShift cluster and is commonly referred to as a virtual IP.
Other applications within the cluster can access your service using this virtual IP. Services also expose a port, such
as port 3306 for MySQL or port 8080 for the Hello World application in the example.

To access a service from outside your cluster, you need to use another OpenShift resource called a "Route." Routes allow
external access to services by exposing them to external IP addresses and ports. Routes will be covered in more detail
later in the documentation section.

The most important configuration options for services are found in the `spec` field. You can use
the `oc explain service spec` command to explore the details of the `spec` field and its available configuration
options.

One of the fields in the `spec` is the cluster IP, which represents the virtual IP assigned to the service by OpenShift.
This IP is only exposed within the OpenShift cluster and enables communication between other Pods within the cluster and
the service.

Additionally, the `spec` field contains a list of ports exposed by the service, allowing you to define the ports used by
your application. The `selector` field specifies how selectors work and provides more information about how services
route traffic to the appropriate Pods.

Feel free to explore further options and details using the `oc explain` command with the appropriate parameters.

If you have any more questions or need further assistance, please let me know!

## Creating an OpenShift Network Service

To establish an OpenShift service that allows requests from another pod within our cluster, follow these steps:

Start by ensuring that you have a running OpenShift pod. Execute the following command to create the pod:

```shell
oc create -f labs/pod.yaml
```

Upon completion of the command, your first pod will be created successfully.

Now, let's proceed with creating a service for the pod. Utilize the following command:

```shell
oc expose --port (port_number) pod/pod_name
```

You will receive a concise status message, indicating successful exposure instead of the usual "created" message
obtained from other commands.

Verify the success of the oc expose command by using the `oc status` command. This will confirm whether the service and
pod are correctly configured.

Upon receiving the results from oc status, you should observe the service listed, with the associated pod beneath it.
This indicates that the service has been set up correctly.

It's important to note that services are internally exposed within the OpenShift cluster through a virtual IP, as
explained in the previous lesson.

To validate the service fully, let's create another pod and make a network request from the second pod to the first one.
Use the following command to set up the second pod:

```shell
oc create -f pods/pod2.yaml
```

After the command completes, your second pod will be created.
To open a shell in the second pod, use the following command:

```shell
oc rsh pod-name 
```

Upon execution, you should see a simple prompt displaying the current directory and a dollar sign as the prompt.

Now, we will make a request to the newly created service. The question that arises is: which IP does OpenShift use for
the service? There are a couple of options to find the virtual IP for a service.

The first option is to check the output of oc status. The IP will be displayed next to the service line.
For our initial request, we will use the IP and port obtained from oc status to make the network request between the two
pods.
In previous lessons, we primarily used the curl command, but the Hello World pods do not have curl installed. Instead,
we will use the wget command. Use the following command to make the request:

```shell
wget -qO- (service_IP:port)
```

Using variables:

```shell
curl $HELLO_WORLD_POD_SERVICE_HOST:$HELLO_WORLD_POD_SERVICE_PORT
```

Replace (service_IP:port) with the appropriate values obtained from oc status.
After running the command, you should receive the "Hello World" output:

```shell
Hi! I'm an environment variable.
```

This message corresponds to the one specified in the pod YAML definition.

Let's expose our service for external access:
If you do not have an application up and running, you can use the command `oc new-app app-name --as deployment-config`
```shell
oc expose svc/hello-world
```
After executing the command, you'll receive a confirmation message stating that OpenShift has successfully exposed the
Route for the "hello-world" Service.
To check the status and obtain the URL of the newly created Route, we can use the oc status command. The first line of
the output will display the Route URL.
Copy the Route URL provided by the oc status command. Now, you can use tools like curl to send HTTP requests to that
address and verify that the Route is working as expected. For example, you can run: curl <Route_URL> to check if the
Route is accessible.
By following these steps, you have successfully created a Route in OpenShift, allowing external access to your
application.
Checking the new created route
```shell
oc get -o yaml route/hello-world
```

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
