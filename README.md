# Deploy a sample Node.js application on Kubernetes 

For the exercise you will have access to a Red Hat OpenShift cluster. You can use the [Kubernetes documentation](https://kubernetes.io/docs/home/) if you want, and use the kubectl commands. Or if you prefer, you can use the [Red Hat OpenShift documentation](https://docs.redhat.com/en/documentation/openshift_container_platform/4.18/html/about/welcome-index) and use the oc commands.

If you already know the commands but don’t remember the options, you can use the --help after the command to get help, or -h for short. For example:
```
kubectl create deployment --help 
kubectl create deployment -h
```
1. The application is available on [https://github.com/ericbos111/nodejs-sample](https://github.com/ericbos111/nodejs-sample) 
It uses port 3001.

2. Install the OpenShift cli from the OpenShift web interface > Help (the question mark)
![image](https://github.com/user-attachments/assets/6066b21e-f34f-4f1d-ab6c-f28d21933d3b)

or from [mirror.openshift.com](mirror.openshift.com), choose the same version as the cluster version. What I usually do is copy the link, and then from my terminal window, fetch this link with curl:
```
curl -lO https://mirror.openshift.com/pub/openshift-v4/clients/ocp/4.18.9/openshift-client-linux-amd64-rhel9-4.18.9.tar.gz
```
 Decompress the file with tar xvf
```
tar xvf https://mirror.openshift.com/pub/openshift-v4/clients/ocp/4.18.9/openshift-client-linux-amd64-rhel9-4.18.9.tar.gz
```
Copy the files to your path
```
cp oc kubectl /usr/local/bin/
```
Login to the cluster, you can copy the login command from the web interface, or
```
oc login api.<cluster>.techzone.ibm.com:6443 -u kubeadmin -p <password>
```

Create your own namespace or project, using the official documentation.
```
oc new-project eric
```
Deploy this application to your namespace in any way you like.
```
oc new-app https://github.com/ericbos111/nodejs-sample
```
Create a Service and an Ingress or Route, using the official documentation.
```
oc create route edge --service=nodejs-sample
```
	
Get the Ingress or Route, using the official documentation.
```
oc get route
```

Verify if you can access the route, either from your browser or with curl
```
curl -k https://<route>
```
Now, if you want to challenge yourself, you can fork the repo to your own Github, edit the application (in your own repository) and restart the build, for example, change the text “Hello from Node.js Starter Application!” to something else.  
Observe what happens. 
After some time, the result of your curl command will have changed.







# Relationship between 'devfile' and 'Dockerfile'

Before you begin creating an application with this `devfile` code sample, it's helpful to understand the relationship between the `devfile` and `Dockerfile` and how they contribute to your build. You can find these files at the following URLs:

* [Node.js `devfile.yaml`](https://github.com/nodeshift-starters/devfile-sample/blob/main/devfile.yaml)
* [Node.js `Dockerfile`](https://github.com/nodeshift-starters/devfile-sample/blob/main/Dockerfile)

1. The `devfile.yaml` file has an [`image-build` component](https://github.com/nodeshift-starters/devfile-sample/blob/main/devfile.yaml#L18-L24) that points to your `Dockerfile`.
2. The [`Dockerfile`](https://github.com/nodeshift-starters/devfile-sample/blob/main/Dockerfile) contains the instructions you need to build the code sample as a container image.
3. The `devfile.yaml` [`kubernetes-deploy` component](https://github.com/nodeshift-starters/devfile-sample/blob/main/devfile.yaml#L25-L37) points to a `deploy.yaml` file that contains instructions for deploying the built container image.
4. The `devfile.yaml` [`deploy` command](https://github.com/nodeshift-starters/devfile-sample/blob/main/devfile.yaml#L45-L52) completes the [outerloop](https://devfile.io/docs/2.2.0/innerloop-vs-outerloop) deployment phase by pointing to the `image-build` and `kubernetes-deploy` components to create your application.

## License

This stack is licensed under the [EPL 2.0](./LICENSE) license.

### Additional resources
* For more information about Node.js, see [How do I start with Node.js after I installed it?](https://nodejs.org/en/docs/guides/getting-started-guide).
* For more information about devfiles, see [Devfile.io](https://devfile.io/).
* For more information about Dockerfiles, see [Dockerfile reference](https://docs.docker.com/engine/reference/builder/).
