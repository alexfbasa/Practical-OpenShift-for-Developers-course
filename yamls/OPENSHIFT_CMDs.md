## Creating a Project

$ oc new-project myproject --display-name 'My Project'
Already on project "myproject" on server "https://localhost:8443".
You can add applications to this project with the 'new-app' command.
For example, try:
oc new-app centos/ruby-22-centos7~https://github.com/openshift/ruby-ex.git
to build a new example application in Ruby.
You can list all projects you have access to using the oc projects command:
$ oc projects
You have one project on this server: "My Project (myproject)".
Using project "myproject" on server "https://localhost:8443".
The name of the current project, against which your commands will be applied, can
be determined by running the command oc project:
$ oc project
Using project "myproject" on server "https://localhost:8443".
When you have access to multiple projects, you can set the current project by run‐
ning oc project and specifying the name of the project:
$ oc project myproject
Now using project "myproject" on server "https://localhost:8443".
When you create a new project using oc new-project, the new project will automati‐
cally be set as the current project.
If you need to run a single command against a different project, you can pass the
name of the project using the --namespace option to any command that operates on
a project:
$ oc get templates --namespace openshift

## Adding a Collaborator

`admin`
A project manager. The user will have rights to view any resource in the project
and modify any resource in the project except for quotas. A user with this role
for a project will be able to delete the project.
`edit`
A user that can modify most objects in a project, but does not have the power to
view or modify roles or bindings. A user with this role can create and delete
applications in the project.
`view`
A user who cannot make any modifications, but can see most objects in a project.
To add another user with edit role to the project, so they can create and delete appli‐
cations, you need to use the oc adm policy command. You must be in the project
when you run this command:
$ oc adm policy add-role-to-user edit <collaborator>
Replace <collaborator> with the name of the user as displayed by the oc whoami
command when run by that user.
To remove a user from a project, run:
$ oc adm policy remove-role-from-user edit <collaborator>

## Deploying Your First Image

To deploy the container image use the oc new-app command, providing it with the
location of the image. The --name option is to set the name for the deployed applica‐
tion. If a name is not supplied, it will default to the last part of the image name. We’ll
use the name blog:
$ oc new-app openshiftkatacoda/blog-django-py --name blog

When a container image is deployed from the command line using oc new-app, the
running container will not be visible outside the OpenShift cluster. In the case of a
web application, you can make it visible by exposing the service. This is done using
the oc expose command:
$ oc expose service/blog

This will create a resource object called a route.
With the application deployed and visible, you can check on the status of the overall
project using the oc status command:
$ oc status

To get a list of the instances of the application that were deployed you can use the
command oc get pods:
$ oc get pods

To get a list of the resource objects created for the application you can use the oc get
all command:
$ oc get all -o name --selector app=blog

## Scaling Up the Application

When oc new-app is used to deploy an application from a container image, only one
instance of the application will be started. If you need to run more than once instance
in order to handle the expected traffic, you can scale up the number of instances by
running the oc scale command against the deployment configuration:
$ oc scale --replicas=3 dc/blog

## Runtime Configuration

Configuration for an application can be supplied by setting environment variables in
the container or by mounting configuration files into the container.
You set required environment variables by using the --env option when running the
oc new-app command:
$ oc new-app openshiftkatacoda/blog-django-py --name blog \
--env BLOG_BANNER_COLOR=green

Optional environment variables can be set later by running the oc set env com‐
mand against the deployment configuration:
$ oc set env dc/blog BLOG_BANNER_COLOR=green

When the environment variables are updated using oc set env, the application will
be redeployed automatically with the new configuration. If you want to see what environment variables will be set in the
container, you can use oc set env with the --
list option:
$ oc set env dc/blog --list


Deleting the Application
When you no longer need the application, you can delete it using the oc delete
command.
When doing this, you need to be selective about which resource objects you delete to
ensure you delete only those for that particular application. This can be achieved by
using the label that was applied by the oc new-app and oc expose commands to the
resource objects created:
$ oc delete all --selector app=blog