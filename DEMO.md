# Deploy app using S2I 
We are going to build a image from our GIT repository as example:
* 1 - Go to build catalog and select Python:
* 2 - Name of the app: simple-webapp
* 3 - Git repository: https://github.com/alexfbasa/simple-webapp.git
* 4 - Create and check logs:

Checking logs:
Pod
```shell
oc logs POD_NAME
```
Check external access:
Check the running web-app accessing http://simple-webapp-""project-name"".""your-cluster-ip"".nip.io/
Check `how are you` accessing http://simple-webapp-""project-name"".""your-cluster-ip"".nip.io/how%20are%20you