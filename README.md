# Practical OpenShift for Developers Labs Repository

Welcome to the Labs repository for the Practical OpenShift for Developers course. In this repository, you will find
resources, tutorials, and hands-on exercises to help you learn and master OpenShift, a powerful container platform.

## Openshift resources:

OpenShift involves different teams and resource types to facilitate efficient application development and management.
The two main teams involved are:

<ol><li>OpenShift Administrator Team:These are the experts responsible for setting up and maintaining OpenShift clusters.
They play a crucial role in ensuring the smooth operation of the platform.</li>
<li>Application Development Team: This team focuses on delivering business value by writing code and deploying applications to OpenShift.
They utilize a variety of tools called OpenShift Resources to run their applications.</li></ol>

OpenShift offers various resource types, each with its own unique functionality. Some of the commonly used resource
types include:

* User
* DeploymentConfig
* Build
* ImageStream
* Route
* Project
* ReplicationController
* BuildConfig
* ImageStreamTag
* Template
* Pod
* ConfigMap
* Secret
* Service
* Autoscaler (HPA)

The first type of team is the OpenShift Administrator team.
These are the people who set up and maintain OpenShift clusters.
Administrator teams are very important because there would be no OpenShift without them, but they are
not the only group to use OpenShift.
The second kind of team is the application development team.
Application development teams are responsible for delivering business value by writing code and deploying
applications to OpenShift.
Application developers have a whole different set of concerns than administrators.
Developers don't need to learn how to set up an OpenShift cluster or how to maintain one.
Instead, developers need to learn how to use a variety of tools called OpenShift Resources in order
to run their applications.
OpenShift has many different resource types, each with its own unique functionality.

## Docker Build Strategy

The Docker build strategy is a commonly used approach in OpenShift for building container images. It involves creating a
Dockerfile that contains a list of instructions for building the image. When the build job runs, OpenShift automatically
creates a Docker image based on the specified Dockerfile. The resulting image is then pushed to the internal Docker
registry.

## Prerequisites

To follow along with the labs and exercises in this course, you will need a few prerequisites:
Container Development Software: Make sure you have the necessary software installed, such as code editors, OpenShift
Online, and Docker, all of these resources will set up with minishift.
Online Accounts: You will need accounts on platforms like GitLab and Quay.io to complete some of the more complex labs.
Please sign up for these accounts if you haven't already.

* GitHub
* Quay.io

## Hands-on-Docker

As a starting point, let's run a container process on your local machine using Docker. Make sure you have Docker
installed, and then run the following command:

```shell
docker run hello-world
```

This command will run a simple container that prints a "Hello, World!" message to verify that your Docker setup is
working correctly.

By mastering OpenShift and containerization techniques, you will be equipped with powerful tools to develop and deploy
applications efficiently. Let's dive into the hands-on labs and start exploring the world of OpenShift and containers!

## What are images?

In the context of containers, an image is a fundamental building block. Containers are created from images, which
contain all the necessary files, dependencies, and configurations required to run a specific application or service.

When running the docker run command, Docker downloads or pulls the specified image from a container registry, such as
Docker Hub, and uses it to create and run a container.

Building an image is the initial step in the container lifecycle. The build process typically occurs on a host operating
system, which could be your personal computer, a continuous integration pipeline step, or containers hosted on
OpenShift.

To build an image, you need two main components: source files and a Dockerfile. The source files include all the
necessary files required to run the container, such as compiled build artifacts (e.g., binary files) or source code
files. The Dockerfile serves as a blueprint for building the image based on the provided source files.

Once the Docker build command completes the image building process, you can use the resulting image with the docker run
command to create a running container. Technically, the image is initially stored in a local image storage system. You
can also push the image to a container registry, such as Docker Hub or a private registry.

Container registries act as large image storage systems hosted on the internet. They allow you to store and share images
for your projects or collaborate with others. For example, the Hello World image in the previous lesson was built by
someone else who then pushed it to Docker Hub, making it available for download.

The process of going from source code to a running container involves two steps: building the image and then running it
on a container runtime, such as Docker or OpenShift. A container runtime refers to the software or environment capable
of running containers.

## Docker build

To build the image for the Hello World source code, you first need to navigate to its directory. Follow these steps:

* Go to the labs' directory.
* Change into the Hello World Go directory.
  Once you are in the correct directory, you can proceed with the Docker Build command. Here's the command you need to
  run:

```shell
$ docker build .
```

The docker build command requires the directory containing the source files for your image as the primary argument. In
this case, the . specifies the current directory.

When you run the docker build command, the build process starts by downloading the base image specified in the
Dockerfile using the FROM instruction. The download may take some time, depending on the size of the image.

If you are following along with the course, you should pause the video until your docker build command completes. This
allows the build process to finish before moving on to the next step.

## Tagging a Container Image

After building the image in the previous lesson, you should have received an output similar to the following:

```text
Successfully built <image_id>
```

To run this image, you need to know its image ID. Let's run the image using its ID.

To run the image, use the docker run command with the -i and -t flags. These flags ensure that the container runs
properly in the terminal. Copy the first line from your output, and combine it with the docker run command. Your command
should look like this:

```shell
docker run -it <image_id>
```

Execute this command to run the image.

You should see the startup message, indicating that the container is running. You can use Ctrl + C to exit the process.

Random IDs like the one used in the command above are not ideal for communicating with others as they are not
human-friendly. To give the image a more human-friendly name, you can add a tag.

In Git, tags are named references to existing commits. Similarly, in containers, tags are named references to existing
container images. Tags are essential for working with OpenShift and container-based development.

You can specify a tag during the build process using the -t option with the docker build command. This will attach the
tag to the image.

To build the image with a different tag, you can use the same docker build command but specify a different tag.
Instead of using your-app:latest, use your-app:your-tag in the command:

```shell
docker build -t your-app:your-tag .
```

This command will build the image with the specified tag and the current directory as the build context.

After running the command, you will see the confirmation that the image has been built with the repository your-app and
the tag your-tag.

To verify the images, you can run docker images again. In the output, you will notice that both the latest and your-tag
tags share the same image ID. This is because the repository and tag are metadata about the image and do not affect the
runtime behavior of the container.

# Minishift

Minishift is a tool that allows you to run a single-node OpenShift cluster on your local machine for development and
testing purposes. It is designed to provide a lightweight and easy-to-use OpenShift environment.
You can easily run a minishift cluster just executing that line:

```shell
minishift start
```

However, additional parameters can be defined.
If you're using Minishift and the cluster has stopped, you can start it by running:

```shell
minishift config set vm-driver virtualbox
```

```shell
minishift start --cpus 2 --memory "16GB"
```

Passing Proxy definitions

```shell
minishift start --cpus 2 --memory "16GB"  --show-libmachine-logs --vm-driver virtualbox --http-proxy http://proxy:3128 --https-proxy http://proxy:3128 --no-proxy localhost,.proxy.fr,127.0.0.1,.proxy.net,192.168.99.100
```

Passing Proxy definitions to the Docker service

```shell
minishift start --cpus 2 --memory "16GB" --docker-env HTTP_PROXY=http://proxy:3128  --docker-env HTTPS_PROXY=http://http://proxy:3128  --docker-env NO_PROXY=.int.kn,*.int.kn
```

Passing Proxy definitions everywhere.

```shell
minishift start --cpus 2 --memory "16GB" --http-proxy http://proxy:3128  --https-proxy http://proxy:3128  --no-proxy .int.com,*.int.com --docker-env HTTP_PROXY=http://proxy:3128 --docker-env HTTPS_PROXY=http://proxy:3128  --docker-env NO_PROXY=.int.com,*.int.com,172.30.1.1

```

If your minishift got successfully running, you will get an output like that:

```text
The server is accessible via web console at:
    https://192.168.99.102:8443/console

You are logged in as:
    User:     developer
    Password: <any value>

To login as administrator:
    oc login -u system:admin
```

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

## PODs

In OpenShift, containers are run within pods, which are a grouping mechanism for containers. The term "pod" is a play on
the word used to describe a group of whales, indicating that multiple containers can be thought of as a pod or a group.
Pods in OpenShift serve the purpose of running container-based applications in a reliable and cohesive manner. While we
have been using a single container for our Hello World application so far, real-world applications often include
additional containers within the same pod. These additional containers are commonly referred to as sidecar containers
and can handle tasks such as logging, credential management, or proxies.

OpenShift requires containers to run inside a pod, meaning that you cannot directly run a single container without a
pod. However, it is possible to have a pod with just a single container, which is what we will use in this course,
utilizing the Hello World container image specific to this course.

## Pod Vs Container

* Pod:
  In OpenShift, a pod is the smallest and simplest unit of deployment. It represents a group of one or more containers
  that are scheduled and run together on the same host.
  A pod provides a cohesive and isolated environment for containers to run within. It encapsulates multiple containers
  and
  their shared resources, such as networking and storage.
  Containers within a pod share the same network namespace, allowing them to communicate with each other using localhost
  or private IP addresses.
  Pods are designed to be ephemeral and disposable. They can be easily created, scaled, replicated, and destroyed as
  needed.
  Pods can be managed and orchestrated by OpenShift, which handles scheduling, load balancing, scaling, and other
  management aspects.

* Docker Container:
  A Docker container is a lightweight and standalone executable package that includes everything needed to run an
  application, including the code, runtime, system tools, libraries, and dependencies.
  Containers created using Docker are typically single entities, representing a specific application or service.
  Docker containers are designed to be portable and consistent across different environments. They can be easily shared,
  distributed, and run on different hosts or platforms that support Docker.
  Containers created with Docker are isolated from the host system and other containers, providing process-level
  isolation
  and resource management.
  Docker containers can be built, deployed, and managed using Docker tools and technologies.
  In summary, a pod in OpenShift is a higher-level construct that manages one or more containers and provides an
  environment for them to run together. It offers additional features and abstractions for managing groups of
  containers.
  On the other hand, a Docker container is a standalone unit that encapsulates an application and its dependencies,
  providing portability and consistency across different environments. OpenShift can utilize Docker containers as part
  of
  its pod-based architecture.

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

``````
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
