# YAML 
Syntax:
```yaml
Key: Value
key1: 1
key2: someString
```
Two properties and two simple values.
```yaml
otherKey1:
  innerKey1: 1
  innerKey2: 2
```
Yaml list
```yaml
- 1
- 2
- 3
```
```yaml
ingredients:
  - Tomato
  - Onion
  - Garlic
```
## Pod definition 
```yaml
apiVersion: v1    #--- Top level
kind: Pod         #--- Top level         
metadata:         #--- Top level
  name: hello-world-pod
  labels:
    app: hello-world-pod
spec:             #--- Top level
  containers:
  - env:
    - name: MESSAGE     #--- First object
      value: Hi! I'm an environment variable
    image: quay.io/practicalopenshift/hello-world
    imagePullPolicy: Always
    name: hello-world-override
    resources: {}
```
Adding new objets
```yaml
apiVersion: v1    #--- Top level
kind: Pod         #--- Top level         
metadata:         #--- Top level
  name: hello-world-pod
  labels:
    app: hello-world-pod
spec:             #--- Top level
  containers:
  - env:
    - name: MESSAGE     #--- First object
      value: Hi! I'm an environment variable
    - name: MESSAGE2    #--- Second object
      value: Hi! I'm an environment variable
    image: quay.io/practicalopenshift/hello-world
    imagePullPolicy: Always
    name: hello-world-override
    resources: {}
```
Adding new containers
```yaml
apiVersion: v1    #--- Top level
kind: Pod         #--- Top level         
metadata:         #--- Top level
  name: hello-world-pod
  labels:
    app: hello-world-pod
spec:             #--- Top level
  containers:
  - env:
    - name: MESSAGE 
      value: Hi! I'm an environment variable
    image: quay.io/practicalopenshift/hello-world
    imagePullPolicy: Always
    name: hello-world-override
    resources: {}
  - env:
    - name: MESSAGE
      value: Hi! I'm an environment variable
    image: quay.io/practicalopenshift/hello-world
    imagePullPolicy: Always
    name: hello-world-override
    resources: {}
```
## Getting explanation about the YAML
```text
oc explain pod.spec.containers.env
```