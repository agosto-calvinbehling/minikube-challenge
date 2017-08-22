Minneapolis Minikube Agosto Challenge
---
The goal of this challenge is to install minikube and run it. Giving you a way to play with Kubernetes without qualifying for a bill at the end of the month.
However, you can run the completion job on any kubernetes cluster.

Dependencies
---
Let's get started! There are a few things that you will need to download.

1. The [latest stable version of minikube](https://github.com/kubernetes/minikube/releases) which should be found with related documentation for your OS.
2. [Kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/) for sending commands to your cluster.
3. One of the following VM drivers. Virtualbox is the most portable one and it works pretty well for me.
	- virtualbox
	- vmwarefusion
	- KVM
	- xhyve
	- Hyper-V
4. A Google account to register completion


Steps
---
1. Starting your cluster via `minikube start`
	- The `--disable-driver-mounts` flag should prevent your vm driver from mounting any directories (e.g. the HOME directory) automatically.
	- The `--vm-driver` flag tells minikube which VM driver to use. The default is "virtualbox" so virtualbox users can skip this one.
```
minikube start --disable-driver-mounts --vm-driver="virtualbox"
```
2. Checking the cluster status.
```
kubectl get all
```
3. Start the kubectl proxy service. It should print a message including the address of your kubernetes master (e.g. [127.0.0.1:8001](http://127.0.0.1:8001))
```
kubectl proxy --port=8001
```
4. Open the kubernetes console. Add `/ui` to the end of your proxy path to jump to the console. (e.g. [127.0.0.1:8001/ui](http://127.0.0.1:8001/ui)). This ui is actually serving from inside the kubernetes cluster!
5. Congratulations! You have a kubernetes cluster. Play around a bit and when you're ready, complete the rest of the steps to register completion!
6. Deploy our challenge service. The yaml file is included in this repository.
```
kubectl apply -f complete-the-challenge.yaml
```
7. Access the service.This is the link that works on my machine: [http://127.0.0.1:8001/api/v1/namespaces/default/services/mk-challenge-service/proxy](http://127.0.0.1:8001/api/v1/namespaces/default/services/mk-challenge-service/proxy). The format is roughly:
```
http://[kubernetes_proxy_address[:port]]/api/v1/namespaces/[namespace_name]/services/[service_name[:port_name]]/proxy
```
8. Complete the challenge. Add the path `/complete` to the service path. It should look something like [http://127.0.0.1:8001/api/v1/namespaces/default/services/mk-challenge-service/proxy/complete](http://127.0.0.1:8001/api/v1/namespaces/default/services/mk-challenge-service/proxy/complete).
	- You need to sign in to a Google account for this to work. If you hit the login screen you may see an invalid token error. Try it again after logging in and you should be passed along to registration.
9. Fill out the form! You're done!

Tips & Tricks
---
1. Eventually you'll need kubectl to work on multiple clusters. It will store a config file with information on how to connect to the various clusters you've configured. The following commands help you reconfigure your cluster target.
```
kubectl config current-context  # prints name of current-context (e.g. "minikube")
kubectl config get-contexts  # List info about contexts.
kubectl config
```
If your context changes, you can switch again by using `kubectl config use-context <context>`
```
kubectl config use-context "minikube"
```
Minikube also comes with a helper command
```
minikube update-context
```

2. Cool addons. This enables some additional graphing and cluster information.
```
minikube addons enable heapster
minikube addons enable ingress
```

3. The `--profile` flag adds a name to the minikube instance you want to start. This is useful if you want multiple instances. The default profile is "minikube". Remember that you'll need to specify the profile on all minikube commands.

4. You can access your minikube nodes directly. This allows you to use `NodePort` to access a service in minikube. List the services to show what's available!
```
minikube service list
```
