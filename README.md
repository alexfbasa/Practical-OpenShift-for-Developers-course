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

```text
[ User, DeploymentConfig, Build, ImageStream, Route, Project, ReplicationController, BuildConfig, ImageStreamTag]
[ Template, Pod, ConfigMap, Secret, Service, Autoscaler (HPA)]
```

## Getting Started with OpenShift

## Jenkins Demo

One of the most powerful tools in OpenShift is the OpenShift command-line interface, also known as oc. This command-line
tool simplifies application management and deployment on OpenShift.
To start a Jenkins instance in OpenShift using the command-line interface, you can run the following command:

```shell
$oc new-app jenkins-ephemeral
```

## Setting up Credentials

To ensure the security of your credentials, we provide a template file named credentials_template.env within the Labs
project. You can copy or rename this file to credentials.env and populate it with your own values. This file will be
used in various labs and walk throughout the course to set up your credentials and configuration.

It is essential to keep this information confidential and avoid committing it to version control. To help with this, the
credentials.env file is listed in the .gitignore file for the Labs directory.

## Docker Build Strategy

The Docker build strategy is a commonly used approach in OpenShift for building container images. It involves creating a
Dockerfile that contains a list of instructions for building the image. When the build job runs, OpenShift automatically
creates a Docker image based on the specified Dockerfile. The resulting image is then pushed to the internal Docker
registry.

## Prerequisites

To follow along with the labs and exercises in this course, you will need a few prerequisites:
Container Development Software: Make sure you have the necessary software installed, such as code editors, OpenShift
Online, and Docker.
Online Accounts: You will need accounts on platforms like GitLab and Quay.io to complete some of the more complex labs.
Please sign up for these accounts if you haven't already.

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

* Go to the labs directory.
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

# PODs

In OpenShift, containers are run within pods, which are a grouping mechanism for containers. The term "pod" is a play on
the word used to describe a group of whales, indicating that multiple containers can be thought of as a pod or a group.
Pods in OpenShift serve the purpose of running container-based applications in a reliable and cohesive manner. While we
have been using a single container for our Hello World application so far, real-world applications often include
additional containers within the same pod. These additional containers are commonly referred to as sidecar containers
and can handle tasks such as logging, credential management, or proxies.

OpenShift requires containers to run inside a pod, meaning that you cannot directly run a single container without a
pod. However, it is possible to have a pod with just a single container, which is what we will use in this course,
utilizing the Hello World container image specific to this course.

To check if OpenShift is running, you can use the following commands:

If you're using Minishift and the cluster has stopped, you can start it by running:
```shell
minishift start
```
To check the status of your OpenShift cluster, use the command:
```shell
oc status
```
To create a Pod on OpenShift based on a YAML file, use the following command:
```shell
oc create -f pods/pod.yaml
```
Replace pods/pod.yaml with the path to your specific YAML file.

To show all currently running Pods in OpenShift, you can run:
```shell
oc get pods

```
### Get Pod Documentation

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

## Creating Pods from files

Create a Pod on OpenShift based on a file

```text
oc create -f pods/pod.yaml
```

Show all currently running Pods

```text
oc get pods
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

# DeploymentConfig

You can think of deployment configs as going one step further and being a collection of pods.
Using a deployment config to manage your pods is much better than trying to do it manually.
Deployment configs bring a lot of automation and extra configuration options to your pods.

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

```shell
oc delete svc/hello-world
oc delete dc/hello-world
oc delete istag hello-world:version
```

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

Use the environment variables with wget
wget -qO- $HELLO_WORLD_POD_PORT_8080_TCP_ADDR:$HELLO_WORLD_POD_PORT_8080_TCP_PORT

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
