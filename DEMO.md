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
into a reusable Docker image without the need for a Dockerfile.
[Application Code](labs/demos/simple-webapp/app.py)
The S2I tool creates the image by using a pre-built
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

```
apiVersion: image.openshift.io/v1
kind: ImageStream
metadata:
  name: my-python-imagestream
spec:
  tags:
    - name: latest
      from:
        kind: DockerImage
        name: registry.example.com/my-organization/python@sha256:abcdef1234567890  <-- ImageStream ID
```

When an image is created by an Image Stream, it points to the image ID, which is unique across builds. Even if the
target image, such as the Python image, gets updated, the Image Stream still points to the previous known good image
using the image ID. Note that Image Streams can be updated as needed.

## Build Configuration

To create a new build configuration, you can follow these steps:

1. Select the build in the OpenShift web console and go to the configuration tab.
2. In the configuration details, you can see the build strategy, the URL of the GitHub repository, and the Python image
   used by the S2I tool.
   [View Build Config](simple-webapp-docker/images/img_3.png)
3. Once the build is successful, the final Docker image is pushed to the internal Docker registry at the
   address `cp-0001/simple-webapp:latest`.
4. In the top right corner of the user interface, there is an action button with options like edit and edit YAML.
   Clicking on edit YAML provides the build configuration in YAML format.
5. To create a new build configuration using the Docker build strategy, reuse the S2I YAML file and modify it
   accordingly. Check files:
   [S2I app file](labs/demos/S2i-build-config.yaml)
   [Docker app file](labs/demos/Docker-build-config.yaml)
6. Give the build configuration a new name, such as `simple-webapp-docker`, to differentiate it from the existing one
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

Let's proceed with a demo of Builds in OpenShift.

## New build Configuration Strategy

In this demo, we will look at builds and how to create a new Build configuration in OpenShift. A build configuration was
automatically created in the previous demo. Under the Build section, we see that we have the simple web app built.
Clicking on the Build gives us more details about each run and view related logs.

Under the configuration tab, you can see the configuration such as the Build strategy which happens to be source
in this case, for Source to Image strategy. The source repo points to the GitHub repository. The output refers to the
image stream where the fully built Docker image will be pushed. On the right, we see a list of web hooks that can be
used to integrate code changes to Build.

You can also provide additional environment variables. For any build configuration in the event tag gives you a
list of events associated with the Build. This is information on the existing Build configuration.

But how do you create a new build configuration? For example, in this case, we have a Source Build strategy. How
do you create a new build configuration for a Docker-based strategy?

It is easy to simply use the existing YAML configuration for the current Build strategy and modify it to our
needs.

1. Click on "Actions" and "Edit YAML," and we get the YAML-based configuration for the existing build. Copy the
   contents and move it to an editor. There are a lot of things in this file that we don't need if we're creating a new
   build configuration, such as annotations, create timestamps, and labels. We'll get rid of all of these.

2. We will modify the name to `simple-webapp-docker`. We will also output the new Build to a new image
   `simple-webapp-docker`. We will leave the source repository as is. However, if you look at the source code repository
   right now, we do not have a Dockerfile in it. As we discussed in the lecture, if we are using a Docker strategy, we
   need a Dockerfile in our source code. Since we don't have a Dockerfile, I'm going to go and create one. We will copy
   over instructions that I have used previously. So, I now have a Dockerfile, and I should be good to use this
   repository for my Docker build.
3. I can now create a new build configuration by clicking on the "Add to Project" button and selecting the "Import
   YAML/JSON" option in the project window. I will now paste the contents of the file and click on Create. This will
   create a new build configuration. However, as you can see, it does not start because it says there is an invalid
   output reference. I purposely wanted to show this error because you might come across it from time to time. This is
   because we used a new image stream tag, but that image stream tag was not created.
   ```text
   spec:
     output:
       to:
         kind: ImageStreamTag
         # New build to new image
         name: 'simple-webapp-docker:latest'  <-- It is not available.
     runPolicy: Serial
   ```
4. To fix that, we need to create a new image stream tag. I will use the existing image stream tag, so go to ImageStream
   tab and open the `simple-webapp` imageStream. We will
   copy its contents and what we really need is the name of the image stream. I'm just going to copy the first few lines
   and create a new file. I will then get rid of the unwanted lines and only leave the name.
   ```text
   apiVersion: image.openshift.io/v1
   kind: ImageStream
   metadata:
     annotations:
       openshift.io/generated-by: OpenShiftWebConsole
     creationTimestamp: '2023-07-18T20:51:12Z'
     generation: 1
     labels:
       app: simple-webapp
     name: simple-webapp
   ```
   ```text
   apiVersion: image.openshift.io/v1
   kind: ImageStream
   metadata:
     name: simple-webapp-docker
   ```

5. I will then follow the same procedure I followed to create a new build configuration by going to "Add to
   Project" and selecting the "Import YAML/JSON" option. I will provide the image stream configuration details. This
   creates a new image stream.
   When I go back to my build, I can see that my build is running. If I go to the logs, I can see the Docker build
   happening, and I now see that the build configuration is complete and pushed to the new image stream.
   Well, that's it for this demo.

