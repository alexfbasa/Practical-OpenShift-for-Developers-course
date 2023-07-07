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

## Network Services

In OpenShift, network services play a crucial role in enabling communication and connectivity between various components
within a cluster. They facilitate the interaction between applications, services, and external clients. Let's explore
some of the key network services in OpenShift:

1. **Service**: A Service in OpenShift provides stable, internal network access to a set of pods that perform the same
   function. It acts as an abstraction layer that decouples the pods from the consuming clients. Services are assigned a
   virtual IP address and are responsible for load balancing traffic among the pods associated with them. Services can
   be exposed internally within the cluster or externally to the outside world.

2. **Route**: A Route is an OpenShift resource that exposes services to the external network. It provides external
   access to applications running within the cluster. Routes define an HTTP or HTTPS ingress point that allows traffic
   to be directed to a specific service. With Routes, you can access applications using a fully qualified domain name (
   FQDN) rather than relying on the cluster's internal IP addresses.

3. **Ingress**: Ingress is an API object that manages external access to services within an OpenShift cluster. It
   provides a way to control and configure how inbound traffic is routed to services. Ingress resources define rules and
   configurations for routing HTTP and HTTPS traffic to different services based on hostnames, paths, or other criteria.
   Ingress can be used to expose multiple services under a single domain or subdomain.

4. **Load Balancer**: A Load Balancer is a network service that distributes incoming network traffic across a set of
   backend pods or services. In OpenShift, a Load Balancer is typically used when deploying on cloud providers that
   offer load balancer services. When you expose a Service using a Load Balancer, the cloud provider provisions a load
   balancer that forwards traffic to the Service.

5. **ClusterIP**: ClusterIP is a type of Service that provides internal network access to the pods within an OpenShift
   cluster. It assigns a stable IP address to the Service within the cluster's virtual network. ClusterIP Services are
   accessible only from within the cluster and are not exposed to external clients.

These network services in OpenShift facilitate communication and connectivity between various components, allowing for
scalable and reliable deployment of applications. They provide mechanisms for load balancing, external access, routing,
and service discovery, making it easier to manage and expose services within the cluster.

## Routes

When working with OpenShift, you often need to expose your applications to external users or integrate them with
external services. For this purpose, OpenShift provides an external network interface called a Route.

A Route in OpenShift allows you to give an external DNS name to a service and expose your application for access from
outside the cluster. Let's walk through the process step by step:

To create a Route, we'll use the `oc expose` command. This command determines whether to create a Route or a Service
based on the argument you provide.
If you pass a Service or Deployment Config as an argument to oc expose, OpenShift will create a Route for you. For
example, let's say we have a Service called "hello-world" that we want to expose externally. We can use the command:

## ConfigMap

ConfigMaps are a way to store and manage configuration data separately from the application code. They allow you to
externalize configuration settings and make them available to your application pods as environment variables or as
mounted files.

Let's use the example of an NGINX web server to explain ConfigMaps:

Creating a ConfigMap: You can create a ConfigMap by providing a set of key-value pairs or by directly referencing
configuration files. In this case, let's create a ConfigMap named "nginx-config" with two files: nginx.conf and
index.html

```shell
oc create configmap nginx-config --from-file=nginx.conf --from-file=index.html
```

This command creates a ConfigMap named "nginx-config" and populates it with the contents of nginx.conf and index.html
files.

Using ConfigMaps in Pod: To use the ConfigMap in a pod, you can reference it in the pod's configuration file (e.g., a
YAML file). Here's an example of a pod configuration file that mounts the ConfigMap files as volumes:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
spec:
  containers:
    - name: nginx
      image: nginx
      volumeMounts:
        - name: config-volume
          mountPath: /etc/nginx
  volumes:
    - name: config-volume
      configMap:
        name: nginx-config
```

In this example, the pod specification includes a volumeMount that mounts the ConfigMap files under the /etc/nginx
directory inside the container. The volumes section specifies the config-volume that references the ConfigMap named "
nginx-config".

Accessing ConfigMap Files: Once the pod is created, the ConfigMap files (nginx.conf and index.html) will be available
within the pod at the specified mount path (/etc/nginx). The NGINX container can then read the configuration files from
that path.
For example, within the NGINX container, you can reference the nginx.conf file in the configuration:

```text
...
http {
  include /etc/nginx/nginx.conf;
  ...
}
...
```

## ConfigMap

ConfigMaps in OpenShift are a resource type that holds configuration data for pods to consume. They are separate
entities from running pods and serve the purpose of centralizing and managing configuration information.

One common use case for ConfigMaps is when deploying applications to different environments. For example, during local
development, you may have non-application dependencies such as a local REST service or database running on your machine.
To simplify the development environment and connect your application to these local dependencies, you can use values
that point to the local versions. However, in a full OpenShift deployment, you would want to use different values that
point to the actual REST service and database. ConfigMaps provide the flexibility to define and manage these values,
allowing you to easily switch between different environments.
Ex Local Env:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config-local
data:
  REST_SERVICE_URL: http://localhost:8000
  DATABASE_URL: jdbc:mysql://localhost:3306/mydb

```

Ex production Env:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config-production
data:
  REST_SERVICE_URL: http://production-service:8000
  DATABASE_URL: jdbc:mysql://production-database:3306/mydb
```

When working with ConfigMaps, it's important to understand the concept of consuming. Consuming a ConfigMap means using
the data inside it from a pod. Once a ConfigMap exists in OpenShift, you can refer to it in the pod definition to
consume its data. Multiple pods can consume the same ConfigMap, allowing for centralized configuration management. For
example, a common scenario is sharing a database name between an application and the associated database pod. By using a
ConfigMap, you can store the database name in one place and update it centrally. All pods that consume the ConfigMap
will automatically use the updated configuration.

OpenShift provides various tools to create ConfigMaps. You can create them using command line arguments, files, or
entire directories. It's important to note that ConfigMaps are not meant for storing sensitive data. OpenShift has a
dedicated resource type called Secrets for handling sensitive information securely.

ConfigMaps have a storage size limit of `one megabyte`. If your configuration data exceeds this limit, alternative
approaches should be considered.

In YAML ConfigMap definitions, the important part is the key-value pairs stored in the data field. Each key represents a
configuration property, and its corresponding value holds the configuration data.

In this section, you will learn different methods to create and use ConfigMaps in OpenShift, gaining a deeper
understanding of their benefits and practical applications.

Note: OpenShift ConfigMaps are based on the Kubernetes concept of ConfigMaps. You can find extensive documentation and
resources on ConfigMaps from both Kubernetes and OpenShift.

## ImageStream and ImageStreamTag

In OpenShift, an ImageStream and ImageStreamTag are specific resources used for managing container images within the
cluster. Let's delve into each concept:

ImageStream:

An ImageStream is an OpenShift-specific resource that acts as a named stream of container images. It provides a
higher-level abstraction for managing images and their versions.
ImageStreams track the complete history of a container image, including different versions, tags, and related metadata.
By using an ImageStream, you can decouple your application deployments from specific image versions and refer to them by
a consistent name.
ImageStreams facilitate easy updates to the latest version of an image, making it simpler to manage and deploy
applications in a dynamic environment.
ImageStreamTag:

An ImageStreamTag is a specific version or tag of a container image within an ImageStream.
It represents a unique identifier for a particular version of an image, allowing you to track and reference it easily.
ImageStreamTags provide stability for referencing a specific image version, even if newer versions of the image are
added to the ImageStream.
By associating an ImageStreamTag with a deployment or build configuration, you can ensure that the desired image version
is consistently used.
Together, ImageStreams and ImageStreamTags enable efficient management and versioning of container images in OpenShift.
With ImageStreams, you can abstract away the underlying image details and focus on deploying and maintaining
applications based on specific versions or tags.

By leveraging ImageStreamTags, you ensure that your deployments and builds consistently use a specific version of an
image, even as new versions are added to the ImageStream.

These OpenShift-specific resources help streamline image management, simplify application updates, and enhance
reproducibility within your OpenShift projects.

`oc imagestream`:

An ImageStream is an OpenShift-specific resource that represents a named stream of container images. It acts as a
higher-level abstraction over container images and provides various capabilities for image management.
The oc imagestream command is used to create, view, modify, and delete ImageStreams in OpenShift.
You can create an ImageStream using a YAML definition or by running the oc create imagestream command.
An ImageStream can have multiple tags associated with it, each representing a specific version or variant of the
container image.
`oc imagestreamtag`:

An ImageStreamTag is a specific version or tag of a container image within an ImageStream.
The oc imagestreamtag command allows you to interact with the tags of an ImageStream.
You can use this command to create, view, modify, and delete tags within an ImageStream.
Tags can be associated with specific container images, and they provide a way to refer to and track different versions
or variants of the image.
ImageStreamTags can be used in deployments, builds, and other resources to ensure consistency and reproducibility when
referring to container images.

## Registry

oc adm policy add-cluster-role-to-user cluster-admin developer

## Commands

```shell
oc delete pods --all
oc delete dc --all
oc delete configmap --all
oc delete route --all
oc delete service --all
```