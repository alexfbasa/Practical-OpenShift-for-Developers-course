To create a project in Openshift with a web application running on Nginx and set up the deployment pipeline using the
Jenkinsfile, you can use YAML template files to define the necessary configurations for the project deployment.

Let's assume you have the following template files for your project:

1. `openshift-template.yaml`: Defines the general project configurations and parameters that will be used in other
   templates.
2. `pod-template.yaml`: Defines the configurations for the Pod that will run Nginx.
3. `service-template.yaml`: Defines the configurations for the Service that exposes the Nginx Pod for external access.
4. `route-template.yaml`: Defines the configurations for the Route that allows access to the Nginx web page through a
   URL.

Now, let's create a simple example of the content of each template file:

**1. openshift-template.yaml:**

```yaml
apiVersion: v1
kind: Template
metadata:
  name: cp-0001-template
parameters:
  - name: APP_NAME
    description: The name of the application.
    required: true
    value: my-nginx-app
objects:
  - from: pod-template.yaml
    path: pod.yaml
  - from: service-template.yaml
    path: service.yaml
  - from: route-template.yaml
    path: route.yaml
```

**2. pod-template.yaml:**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: ${APP_NAME}-pod
spec:
  containers:
    - name: ${APP_NAME}-container
      image: nginx:latest
      ports:
        - containerPort: 80
```

**3. service-template.yaml:**

```yaml
apiVersion: v1
kind: Service
metadata:
  name: ${APP_NAME}-service
spec:
  selector:
    app: ${APP_NAME}-pod
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: ClusterIP
```

**4. route-template.yaml:**

```yaml
apiVersion: route.openshift.io/v1
kind: Route
metadata:
  name: ${APP_NAME}-route
spec:
  to:
    kind: Service
    name: ${APP_NAME}-service
  port:
    targetPort: 80
  wildcardPolicy: None
```

Now, in the Jenkinsfile, you can use the command-line client `oc` (OpenShift CLI) to apply the template files and create
the project automatically. Here's an example of the Jenkinsfile:

**Jenkinsfile:**

```groovy
pipeline {
    agent any

    stages {
        stage('Deploy to Openshift') {
            steps {
                script {
                    def project = 'cp-0001'
                    def appName = 'my-nginx-app'

                    sh "oc new-project ${project}"

                    sh "oc process -f openshift-template.yaml -p APP_NAME=${appName} | oc create -f -"
                }
            }
        }
    }
}
```

The above Jenkinsfile defines a pipeline with a stage to deploy the project in Openshift. It uses the `oc process`
command to process the templates and then creates the objects in Openshift using the `oc create` command. The parameter
values are replaced with the values defined in the `project` and `appName` variables.

By executing this pipeline in Jenkins, it will automatically create the project in Openshift with the Nginx web
application running and set up the necessary services and routes for accessing it via a web browser.