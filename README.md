# Kubernetes
Install kubectl on macOS

1 - Download the latest release:

`curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/darwin/arm64/kubectl"
`

2 - Validate the binary

`curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/darwin/arm64/kubectl.sha256"
`

`echo "$(cat kubectl.sha256)  kubectl" | shasum -a 256 --check
`

If valid, the output is:
`kubectl: OK`

If the check fails, shasum exits with nonzero status and prints output similar to:

`kubectl: FAILED
shasum: WARNING: 1 computed checksum did NOT match
`

3 - Make the kubectl binary executable.

`chmod +x ./kubectl`

4 - Move the kubectl binary to a file location on your system PATH.
`sudo mv ./kubectl /usr/local/bin/kubectl`

`sudo chown root: /usr/local/bin/kubectl`

5 - Test to ensure the version you installed is up-to-date:

`kubectl version --client`

`kubectl version --client --output=yaml`

6 - After installing and validating kubectl, delete the checksum file:
rm kubectl.sha256

## Verify kubectl configuration
kubectl cluster-info
If you see a URL response, kubectl is correctly configured to access your cluster.

If you see a message CONNECTION REFUSED

## You need install MINIKUBE, it's local Kubernetes.

For install this one, following the steps:

`curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-darwin-arm64`

`sudo install minikube-darwin-arm64 /usr/local/bin/minikube`

`minikube start`

Manage your cluster
Pause Kubernetes without impacting deployed applications:
* `minikube pause`

Unpause a paused instance:
* `minikube unpause`

Halt the cluster:
* `minikube stop`

Change the default memory limit (requires a restart):

* `minikube config set memory 9001`

Browse the catalog of easily installed Kubernetes services:

* `minikube addons list`

Create a second cluster running an older Kubernetes release:

* `minikube start -p aged --kubernetes-version=v1.16.1`

Delete all the minikube clusters:

* `minikube delete --all`

Getting URL (Internal-IP)

* `minikube service svc-news-portal --url`

MiniKube Dashboard (Kubernetes Console)

* `minikube dashboard`

## How to create a pod?

For sample, run:
* `kubectl run nginx-pod --image=nginx:latest`

Check if execute :
* `kubectl get pod`

## Manage your PODs:

Getting all PODs (detached mode):
  * `kubectl get pods`

Getting all PODs (attached mode):
  * `kubectl get pods --watch`

Show all Pod information at a wide way:
  * `kubectl get pod -o wide`

Show all information of the created POD:
  * `kubectl describe pod nginx-pod`

Edit POD configuration:
  * `kubectl edit pod <nameOfThePod>`

## Kubernetes Declarative Way by File

`kubectl apply -f .\<nameOfTheFile>
`

Errors:

* ErrImagePull - Error to get image
* ImagePullBackOff - Err

## How to delete a pods?

* `kubectl delete pod <nameOfThePod>`
* `kubectl delete -f <nameOfTheFile>`

## How to interact within a created Pod?
* `kubectl exec -it <nameOfThePod> -- <command>`

## How to expose public IP in POD?

When you need expose public IP in the POD use KUBERNETES SERVICES.
### The Kubernetes Services 
Expose an application running in your cluster behind a single outward-facing endpoint, even when the workload is split across multiple backends.
The Service is a method for exposing a network application that is running as one or more Pods in your cluster:
* Provide owner IP's for communication
* DNS provisioning for one or more pods.
* They are capable of load balancing.

### Kubernetes Services Types :
* ClusterIP
* NodePort
* LoadBalancer
* ExternalName

#### ClusterIP
This default Service type assigns an IP address from a pool of IP addresses that your cluster has reserved for that purpose.  
```
apiVersion: v1
kind: Service
metadata:
  name: svc-pod-2
spec:
  type: ClusterIP
  selector:
    app: app-pod-2
  ports:
    - port: 9000
      targetPort: 80
```

#### NodePort

Using a NodePort gives you the freedom to setup your own load balancing solution

```
apiVersion: v1
kind: Service
metadata:
  name: svc-pos-1
spec:
  type: NodePort
  ports:
    - port: 80
      #targetPort: 80
  selector:
    app: app-pod-1
```
#### LoadBalancer

On cloud providers which support external load balancers, setting the type field to LoadBalancer provisions a load balancer for your Service. The actual creation of the load balancer happens asynchronously, and information about the provisioned balancer is published in the Service's .status.loadBalancer field. For example:

```
apiVersion: v1
kind: Service
metadata:
  name: svc-pod-1-lb
spec:
  type: LoadBalancer
  ports:
      - port: 80
        nodePort: 30000
  selector:
    name: app-pod-1
```
