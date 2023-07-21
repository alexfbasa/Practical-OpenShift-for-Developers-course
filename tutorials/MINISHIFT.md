# Minishift

Minishift is a tool that enables developers to run a single-node, local instance of OpenShift on their personal
computer. It is designed to provide a lightweight and easy-to-use environment for developing, testing, and experimenting
with applications that are meant to run on the OpenShift platform.

Minishift allows developers to set up an OpenShift cluster locally, providing a local version of the same features and
functionalities available in a full-scale OpenShift deployment. It leverages virtualization technologies, such as
VirtualBox or KVM, to create a virtual machine that hosts the OpenShift cluster.

With Minishift, developers can simulate the entire OpenShift environment, including the master node, worker nodes,
networking, storage, and services. They can deploy, manage, and monitor applications just as they would in a production
OpenShift environment.

Minishift provides a command-line interface (CLI) and web console for interacting with the local OpenShift cluster.
Developers can use familiar OpenShift commands and tools to create projects, deploy applications, configure routes, and
manage the cluster's resources.

By using Minishift, developers can accelerate their application development and testing workflows. They can iteratively
develop and debug applications locally before deploying them to a production OpenShift environment, saving time and
reducing the risk of issues in the production environment.

In summary, Minishift is a tool that enables developers to run a local OpenShift cluster on their personal computer. It
provides a lightweight and convenient way to develop and test applications that are compatible with the OpenShift
platform, allowing developers to simulate an OpenShift environment locally and streamline their development process.

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
