# This is the Labs repository for the Practical OpenShift for Developers course.

How to stop a running Container:

```text
$docker run -it quay.io/practicalopenshift/hello-world
```

Get running images

```text
docker ps
```

Stop one

```text
docker kill <Container ID>
```

# PODs

Pod is a collection of containers running
The minimum container running inside a pod is 1

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

Follow build progress
oc logs -f bc/hello-world

Check status and pods
oc status
oc get pods
