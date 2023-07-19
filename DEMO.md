# Deploy app using S2I

We are going to build a image from our GIT repository as example:

* 1 - Go to build catalog and select Python:
* 2 - Name of the app: simple-webapp
* 3 - Git repository: [Git alexfbasa](https://github.com/alexfbasa/simple-webapp.git).
* 4 - Create and check logs:

Checking logs:
Pod

```shell
oc logs POD_NAME
```

Check external access:
Check the running web-app accessing http://simple-webapp-""project-name"".""your-cluster-ip"".nip.io/
Check `how are you` accessing http://simple-webapp-""project-name"".""your-cluster-ip"".nip.io/how%20are%20you

Builds

## Docker Build Strategy

To build a Docker image for this application, we would create a Dockerfile with the necessary instructions. In this
case, we start with an Ubuntu OS, install Python and Pip packages, install Flask, copy the application code into the
image, and specify the entry point to run the application. Running the Docker build command using this Dockerfile
creates the Docker image.
[Dockerfile](simple-webapp-docker/Dockerfile)

This build strategy is supported by OpenShift and is known as the Docker build strategy. It requires providing the list
of instructions in a Dockerfile alongside the application code in the source code repository. When the build job runs,
OpenShift automatically creates a Docker image using the Dockerfile, and the image is pushed to the internal Docker
registry.

## Source-to-Image (S2I) Build Strategy

OpenShift also supports the Source-to-Image (S2I) build strategy, which is a framework that converts application code
into a reusable Docker image without the need for a Dockerfile. The S2I tool creates the image by using a pre-built
Python builder image and injecting the application code into it. In a previous exercise, when we deployed the
application using the Python catalog item, OpenShift automatically created a build configuration of type Source-to-Image
build.

To modify the build strategy, you can edit the build configuration from the web console interface or a YAML file.

## Custom Builders for Individual Artifacts

If you need to build individual artifacts like jar files, Python packages, or gem files, you can use custom builders.
However, this is an advanced topic that we won't cover in this course.

## Image Streams for Consistent Referencing

To provide consistent referencing for Docker images and prevent unexpected impacts from image updates, OpenShift
introduced Image Streams. Image Streams map actual Docker images hosted at different locations to image names within
projects in OpenShift. Image Streams serve as pointers to the actual Docker images and provide a layer of abstraction
for referencing Docker images from within OpenShift.

When an image is created by an Image Stream, it points to the image ID, which is unique across builds. Even if the
target image, such as the Python image, gets updated, the Image Stream still points to the previous known good image
using the image ID. Note that Image Streams can be updated as needed.

## Creating a New Build Configuration

To create a new build configuration, you can follow these steps:

1. Select the build in the OpenShift web console and go to the configuration tab.
2. In the configuration details, you can see the build strategy, the URL of the GitHub repository, and the Python image
   used by the S2I tool.
3. Once the build is successful, the final Docker image is pushed to the internal Docker registry at the
   address `test-project/simple-webapp-docker`.
4. On the top right corner of the user interface, there is an action button with options like edit and edit YAML.
   Clicking on edit YAML provides the build configuration in YAML format.
5. To create a new build configuration using the Docker build strategy, reuse the S2I YAML file and modify it
   accordingly.
6. Give the build configuration a new name, such as `simple-web-app-docker`, to differentiate it from the existing one
   in the UI.
7. Modify the source to use a new repository containing the Dockerfile.
8. Set the strategy type to Docker, specify the source image to build the application from (e.g., Ubuntu 16.04), and use
   the same image stream for the output.
9. Once the YAML configuration file is ready, click on the "Add to project" button and select the import YAML option.
10. Copy the contents of the Docker build config YAML file into the interface and click on Create. This adds a new build
    configuration.
11. Opening the new build configuration shows that the build strategy is set to Docker, and the source repository
    containing the Dockerfile is used.
12. You can manually start a build by clicking on the "Start Build" button. If a build is already in progress, the new
    build will be added to the queue and wait for the previous build to finish.

By following these steps, you can create a new build configuration using the Docker build strategy in OpenShift.

Let's proceed with a demo of Builds in OpenShift.