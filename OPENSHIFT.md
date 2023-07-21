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

# Deployment

**Understanding Deployments**

Deployments in Openshift share similarities with Kubernetes. Before delving into Openshift's specific features, let's
recap what Deployments are in general.

In Kubernetes, the smallest deployable unit is a **POD**, which consists of one or more Docker containers interdependent
on each other. Typically, a POD contains a single Docker container, and it serves as an instance of the application,
enabling scaling and high availability.

To achieve multiple instances of the application running, Replication Controllers come into play. These controllers
create replicas of pods, ensuring the desired number of application replicas run continuously. In the hierarchy,
Deployments come next.

**Enhancing Application Lifecycle Management**

Deployments in Openshift build upon Replication Controllers by introducing additional support for Application Lifecycle
Management. This includes features like seamless upgrades, rollbacks, and application revisioning, making it easier to
manage the application's lifecycle.

When you add an application to your project in Openshift, a Build configuration and a Deployment are automatically
created. The Deployment, visible in the applications deployments section, is configured to use the image built by the
Build configuration, which is pointed by the image stream.

Currently, the Deployment is set to deploy only one replica of the application, and the deployment strategy is set to
rolling. With a rolling deployment strategy, when you update the application, the replicas are updated one at a time,
ensuring a smooth transition.

**Triggers for Deployment Updates**

The triggers section answers questions about how often a Deployment gets updated and whether it's manual or automatic.
Openshift offers both manual and automatic ways to trigger a Deployment.

You can manually trigger a Deployment from the user interface (UI) by clicking on the deploy button or via the Command
Line Interface (CLI) using the provided command.

Additionally, the deployment configuration is set to automatically trigger a deployment whenever the dependent
application image is updated. This automatic update occurs whenever a build job runs, streamlining the deployment
process.

**YAML Configuration and User Interface**

The deployment configuration can be viewed in YAML format by selecting the edit YAML option in the actions dropdown.
Although the YAML file looks similar to a deployment configuration in Kubernetes, note that there are some differences.

The kind specified in Openshift is "deployment config," unlike Kubernetes where it is "deployment." Similarly, the app
version in Openshift is specific to the platform. However, other sections such as specification, replicas, strategy, and
template remain similar.

Thankfully, in Openshift, you won't need to work with the YAML file frequently, as a user interface is provided to
configure these values more conveniently.

**Deployment Strategies in Openshift**

In this chapter, we will delve into the various deployment strategies available in Openshift, along with some advanced
deployment techniques.

**Basic Deployment Strategies**

Openshift offers two basic deployment strategies: "recreate" and "rolling update."

The "recreate" strategy involves destroying all instances of the older version of the application and then deploying
newer instances. This means the application will be inaccessible to users during the update process.

The "rolling update" strategy, which is the default in Openshift, updates replicas one at a time. This ensures the
application remains accessible to users throughout the update process, providing a seamless upgrade experience.

**Advanced Deployment Strategies**

Two advanced deployment strategies in Openshift are "Blue/Green" and "A/B deployment."

The "Blue/Green" strategy involves deploying a newer version of the application alongside the current version. After
testing the new version successfully, user access is switched from the old (blue) version to the new (green) version by
modifying the load balancer or routing configuration.

"A/B deployment" allows testing new versions of the application in the production environment, but with limited user
traffic redirected to the new instances. Gradually, based on the A/B test results, traffic distribution to the new
version is increased until a full upgrade is achieved.

- `oc rollout latest <deployment_name>`: Triggers a build to deploy the latest version of the application.
- `oc rollout history`: Provides a history of previous builds.
- `oc rollout describe`: Gives detailed information about a deployment.
- `oc rollout undo`: Rolls back a deployment to a previous version.

## Resuming DeploymentConfig concept

In Openshift, a `DeploymentConfig` is a resource that manages the deployment and scaling of applications. It is an
extension of the Kubernetes `Deployment` resource and provides additional features specific to Openshift, such as
support for lifecycle hooks and automatic triggers for application deployments.

You should create a `DeploymentConfig` when you want to deploy and manage your application in Openshift, especially if
you want to take advantage of the extra features it offers. A `DeploymentConfig` is beneficial in scenarios where you
need to ensure high availability, have seamless upgrades and rollbacks, and want to automate the deployment process
based on certain triggers.

Here's an example of what a basic `DeploymentConfig` YAML file might look like:

**deployment-config.yaml:**

```yaml
apiVersion: apps.openshift.io/v1
kind: DeploymentConfig
metadata:
  name: my-app-deployment
spec:
  replicas: 3
  selector:
    app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
        - name: my-app-container
          image: your-docker-image:latest
          ports:
            - containerPort: 8080
```

Explanation of the YAML content:

- `apiVersion`: Specifies the API version for the `DeploymentConfig`.
- `kind`: Indicates the type of resource, in this case, a `DeploymentConfig`.
- `metadata`: Includes metadata for the resource, such as the name of the deployment.
- `spec`: Contains the specification for the `DeploymentConfig`.
    - `replicas`: Specifies the number of replicas you want to run for your application. In this example, we have set it
      to 3.
    - `selector`: Defines the label selector that matches the pods controlled by this deployment.
    - `template`: Contains the pod template specification for the deployment.
        - `metadata`: Includes labels for the pod template.
        - `spec`: Specifies the actual configuration for the pod.
            - `containers`: Describes the containers running in the pod.
                - `name`: Name of the container.
                - `image`: Specifies the Docker image to use for the container. Replace `your-docker-image` with the
                  actual image name and version you want to deploy.
                - `ports`: Exposes ports for the container. In this example, we have exposed port 8080.

Note that this is a basic example, and you can customize the `DeploymentConfig` according to your specific application
requirements. For instance, you can add environment variables, resource limits, readiness and liveness probes, and other
advanced features to optimize the application deployment in Openshift.

Once you have created the `deployment-config.yaml` file, you can apply it to your Openshift cluster using the `oc apply`
command:

```bash
oc apply -f deployment-config.yaml
```

This will create the `DeploymentConfig` resource and start the deployment of your application based on the specified
configuration.

## Openshift SDN

In Openshift, networking plays a crucial role in ensuring that Pods and services can communicate with each other
effectively within the cluster. Here's a summary of the key points discussed in the lecture:

1. **Kubernetes Networking Basics:**
   In a Kubernetes cluster, each node (master and worker nodes) has its own IP address. When applications are deployed
   as Docker containers in Pods, each Pod is assigned a unique IP address. For pods to communicate with each other, they
   must be on a network that enables communication and provides unique IP addresses.

2. **Openshift Software Defined Networking (SDN):**
   Openshift addresses the networking challenge by using the Openshift Software Defined Networking. It creates an
   Overlay Network, based on the Open vSwitch standard, that spans across multiple nodes in the cluster. This virtual
   network facilitates communication between Pods on different nodes.

3. **Overlay Network:**
   The default network ID for the Overlay Network is 10.128.0.0/14. Each node is assigned a unique subnet within this
   range. For example, node 1 might have 10.128.2.0, node 2 with 10.128.4.0, and so on. All Pods on these nodes get
   unique IP addresses within their respective subnets.

4. **DNS in Openshift:**
   To address the issue of dynamic IP addresses for Pods, Openshift has a built-in DNS server. This DNS server helps map
   IP addresses to Pods and services, allowing you to use pod or service names to connect to other components instead of
   relying on static IP addresses.

5. **Using Services:**
   While direct connectivity between Pods using IP addresses is possible, it is not recommended as IP addresses might
   change. The preferred method for communication between Pods is to use Services. Services act as stable endpoints and
   provide load balancing and service discovery. They allow you to access Pods using the Service's ClusterIP.

6. **Plugins for Software Defined Networking:**
   Openshift provides different plugins for Software Defined Networking, with the default plugin being `ovs-subnet`.
   This plugin offers network connectivity between all Pods across the cluster. Additionally, Openshift supports other
   plugins such as `ovs-multitenant`, `Nuage`, `Contiv`, and `Flannel`, each offering different networking capabilities
   and catering to specific requirements.

The combination of Overlay Networks, Services, DNS, and networking plugins in Openshift ensures seamless communication
and efficient networking within the cluster, enabling your applications to run smoothly and interact with each other
effectively.

## Services and Routes

In Openshift, external connectivity for applications deployed in the cluster is provided through the concept of "
Services" and "Routes."

**1. Services:**
A Service is an abstract way to expose an application running in a Pod to other Pods within the cluster or external
clients. It acts as a stable endpoint, allowing external clients to access the application without worrying about the
specific IP addresses of individual Pods. Services provide load balancing and service discovery for the Pods behind
them.

When a Service is created, it is assigned a virtual IP address (ClusterIP), which serves as the entry point to access
the Pods associated with the Service. Services use selectors to determine which Pods to forward traffic to based on
labels. External clients can access the Service using its ClusterIP and the specified port.

**2. Routes:**
Routes are used to expose applications running in the cluster to external clients outside the cluster. Unlike Services,
which provide access only within the cluster, Routes allow external traffic to reach the Pods running the application.

Routes use the concept of Hostnames to define the external access point. When you create a Route, you specify a hostname
and an optional path that will be mapped to the Service serving the application. For example, a Route with the
hostname "example.com" could be mapped to a Service that serves the web application, allowing external clients to access
the application using "example.com" as the URL.

Routes are implemented through the Openshift router, which is responsible for managing incoming external traffic and
forwarding it to the appropriate Service and Pod inside the cluster.

**External Connectivity Example:**
Let's say you have a web application deployed in your Openshift cluster, and you want to make it accessible externally
through a custom domain name (example.com).

1. First, you create a Service to expose your web application internally within the cluster. This Service has a
   ClusterIP and selects the Pods running the web application based on their labels.

2. Next, you create a Route and map it to the Service associated with the web application. You set the hostname of the
   Route to "example.com."

3. When external clients access "example.com" in their browsers, the Openshift router intercepts the request and
   forwards it to the Service, which in turn load-balances the traffic to the Pods running the web application.

By using Services and Routes, you can effectively manage both internal communication between Pods and external access to
your applications in Openshift, providing a seamless experience for your users.