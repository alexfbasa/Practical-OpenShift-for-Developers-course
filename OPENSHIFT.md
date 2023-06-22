# Openshift

OpenShift is a container application platform developed by Red Hat, which is based on the Kubernetes container
orchestration system. It provides a comprehensive set of tools and functionalities for deploying, managing, and scaling
containerized applications.

OpenShift simplifies the process of building, deploying, and running applications by providing features such as
automated build and deployment pipelines, container image management, scaling capabilities, service discovery, and load
balancing. It allows developers to focus on writing code and delivering features, while the platform takes care of
infrastructure management and container orchestration.

OpenShift provides a scalable and flexible platform for running both traditional and cloud-native applications. It
supports various programming languages, frameworks, and databases, enabling developers to use their preferred tools and
technologies. It also offers built-in security features, access controls, and monitoring capabilities to ensure the
stability and reliability of applications.

With its robust ecosystem, OpenShift allows organizations to build and deploy applications across multiple environments,
including on-premises data centers, public clouds, and hybrid cloud setups. It promotes collaboration and DevOps
practices by enabling teams to work together efficiently and automate the application lifecycle management process.

In summary, OpenShift is a powerful container application platform that provides developers and organizations with the
tools and capabilities needed to build, deploy, and manage modern applications with ease.

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

* Pod (Openshift):
  In OpenShift, a pod is the smallest and simplest unit of deployment. It represents a group of one or more containers
  that are scheduled and run together on the same host.
  A pod provides a cohesive and isolated environment for containers to run within. It encapsulates multiple containers
  and their shared resources, such as networking and storage.
  Containers within a pod share the same network namespace, allowing them to communicate with each other using localhost
  or private IP addresses.
  Pods are designed to be ephemeral and disposable. They can be easily created, scaled, replicated, and destroyed as
  needed.
  Pods can be managed and orchestrated by OpenShift, which handles scheduling, load balancing, scaling, and other
  management aspects.

* Container (Docker):
  A Docker container is a lightweight and standalone executable package that includes everything needed to run an
  application, including the code, runtime, system tools, libraries, and dependencies.
  Containers created using Docker are typically single entities, representing a specific application or service.
  Docker containers are designed to be portable and consistent across different environments. They can be easily shared,
  distributed, and run on different hosts or platforms that support Docker.
  Containers created with Docker are isolated from the host system and other containers, providing process-level
  isolation and resource management.
  Docker containers can be built, deployed, and managed using Docker tools and technologies.
  In summary, a pod in OpenShift is a higher-level construct that manages one or more containers and provides an
  environment for them to run together. It offers additional features and abstractions for managing groups of
  containers.
  On the other hand, a Docker container is a standalone unit that encapsulates an application and its dependencies,
  providing portability and consistency across different environments. OpenShift can utilize Docker containers as part
  of its pod-based architecture.

## Registry

oc adm policy add-cluster-role-to-user cluster-admin developer