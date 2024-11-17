Install kubectl on macOS

1 - Download the latest release:
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/darwin/arm64/kubectl"

2 - Validate the binary
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/darwin/arm64/kubectl.sha256"
echo "$(cat kubectl.sha256)  kubectl" | shasum -a 256 --check

If valid, the output is:
    kubectl: OK

If the check fails, shasum exits with nonzero status and prints output similar to:
    kubectl: FAILED
    shasum: WARNING: 1 computed checksum did NOT match

3 - Make the kubectl binary executable.
    chmod +x ./kubectl

4 - Move the kubectl binary to a file location on your system PATH.
    sudo mv ./kubectl /usr/local/bin/kubectl
    sudo chown root: /usr/local/bin/kubectl

5 - Test to ensure the version you installed is up-to-date:
    kubectl version --client
    kubectl version --client --output=yaml

6 - After installing and validating kubectl, delete the checksum file:
    rm kubectl.sha256

Verify kubectl configuration
kubectl cluster-info
If you see a URL response, kubectl is correctly configured to access your cluster.

If you see a message CONNECTION REFUSED

You need install MINIKUBE, it's local Kubernetes.

For instaal this one, following the steps:
*curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-darwin-arm64
*sudo install minikube-darwin-arm64 /usr/local/bin/minikube
*minikube start

Manage your cluster
Pause Kubernetes without impacting deployed applications:

minikube pause
Unpause a paused instance:

minikube unpause
Halt the cluster:

minikube stop
Change the default memory limit (requires a restart):

minikube config set memory 9001
Browse the catalog of easily installed Kubernetes services:

minikube addons list
Create a second cluster running an older Kubernetes release:

minikube start -p aged --kubernetes-version=v1.16.1
Delete all of the minikube clusters:

minikube delete --all

How to create a pod?

For sample, run:
kubectl run nginx-pod --image=nginx:latest

Check if execute :
kubectl get pod

kubectl get pods //for detached mode
kubectl get pods --watch //for attached mode
kubectl get pod -o wide //Show all Pod information at a wide way
kubectl describe pod nginx-pod //Show all information of the created POD

kubectl edit pod <nameOfThePod>

Declarative Way by File

kubectl apply -f .\<nameOfTheFile>

Errors:
    ErrImagePull - Error to get image
    ImagePullBackOff - Err

How to delete a pods?

kubectl delete pod <nameOfThePod>
kubectl delete -f <nameOfTheFile>

How to interact within a created Pod?
kubectl exec -it <nameOfThePod> -- <command>

How to expose public IP in POD?

When you need expose public IP in the POD use KUBERNETES SERVICES.
The Kubernetes Services expose an application running in your cluster behind a single outward-facing endpoint,
even when the workload is split across multiple backends.
The Service is a method for exposing a network application that is running as one or more Pods in your cluster.
Provide owner IP's for communication
DNS provisioning for one or more pods.
They are capable of load balancing.

For this, Kubernetes using :
    ClusterIP,
    NodePort, and
    LoadBalancer



