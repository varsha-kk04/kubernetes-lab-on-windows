\# Kubernetes Lab on Windows Using Kind



\## 1. Objective



To create a three-node Kubernetes cluster using Kind and demonstrate deployment, scaling, and self-healing using NGINX.



\## 2. Tools Used



\- Windows PowerShell

\- Docker Desktop

\- Kind (Kubernetes IN Docker)

\- kubectl

\- NGINX



\## 3. Cluster Configuration



\- 1 control-plane node

\- 2 worker nodes



File: `kind-config.yaml`



```yaml

kind: Cluster

apiVersion: kind.x-k8s.io/v1alpha4

nodes:

&#x20; - role: control-plane

&#x20; - role: worker

&#x20; - role: worker

```



\## 4. Experiment and Commands with Output



\### Step 1: Check Installed Tools



\*\*Command:\*\*



```powershell

docker --version

kubectl version --client

kind version

git --version

```



\*\*Output (version details):\*\*



\- Docker Desktop: 29.6.2

\- kubectl: v1.36.1

\- Kind: v0.33.0

\- Git: 2.55.0.windows.3



\### Step 2: Create the Kubernetes Cluster



\*\*Command:\*\*



```powershell

kind create cluster --config kind-config.yaml

```



\*\*Result:\*\*



The Kind Kubernetes cluster was created successfully.



\### Step 3: Verify Cluster Nodes



\*\*Command:\*\*



```powershell

kubectl get nodes

```



\*\*Output:\*\*



```text

NAME                 STATUS   ROLES           AGE   VERSION

kind-control-plane   Ready    control-plane   10m   v1.37.0

kind-worker          Ready    <none>          10m   v1.37.0

kind-worker2         Ready    <none>          10m   v1.37.0

```



\*\*Observation:\*\* All three nodes were in the `Ready` state.



\### Step 4: Deploy NGINX with Three Replicas



\*\*Command:\*\*



```powershell

kubectl create deployment web --image=nginx --replicas=3

```



\*\*Result:\*\*



```text

deployment.apps/web created

```



Verify the deployment:



```powershell

kubectl get deployments

kubectl get pods

```



\*\*Observation:\*\* Kubernetes created three NGINX Pods.



\### Step 5: Scale the Deployment to Six Replicas



\*\*Command:\*\*



```powershell

kubectl scale deployment web --replicas=6

```



\*\*Result:\*\*



```text

deployment.apps/web scaled

```



Verify the deployment:



```powershell

kubectl get deployments

```



\*\*Output:\*\*



```text

NAME   READY   UP-TO-DATE   AVAILABLE   AGE

web    6/6     6            6           8m33s

```



\*\*Observation:\*\* All six replicas became available.



\### Step 6: View Pod Details



\*\*Command:\*\*



```powershell

kubectl get pods -o wide

```



\*\*Purpose:\*\* Displays the Pod names, status, node assignment, IP addresses, and other details.



\*\*Observation:\*\* The NGINX Pods were running in the Kubernetes cluster.



\### Step 7: Test Kubernetes Self-Healing



First, list the Pods:



```powershell

kubectl get pods

```



Delete one Pod using its actual name:



```powershell

kubectl delete pod <pod-name>

```



Replace `<pod-name>` with the name of one of the NGINX Pods.



\*\*Result:\*\* The selected Pod was deleted, and Kubernetes automatically created a replacement Pod.



\*\*Example:\*\* A deleted Pod named `web-5fc9f4bf66-b58mz` was replaced by `web-5fc9f4bf66-vk6tj`.



Verify again:



```powershell

kubectl get pods

kubectl get deployments

```



\*\*Observation:\*\* The deployment continued to maintain six replicas.



\## 5. Results



1\. Successfully created a Kubernetes cluster with one control-plane node and two worker nodes.

2\. Deployed NGINX using a Kubernetes Deployment.

3\. Scaled the deployment from three replicas to six replicas.

4\. Verified that all six replicas were available.

5\. Tested self-healing by deleting a Pod and observing its replacement.



\## 6. Conclusion



This experiment demonstrates Kubernetes cluster creation, application deployment, scaling, and self-healing using Kind on Windows. It shows how Kubernetes maintains the desired number of application replicas when a Pod is deleted.



\## 7. Useful Commands



| Purpose | Command |

|---|---|

| View cluster nodes | `kubectl get nodes` |

| View Pods | `kubectl get pods` |

| View Pods with node details | `kubectl get pods -o wide` |

| View deployments | `kubectl get deployments` |

| Scale to six replicas | `kubectl scale deployment web --replicas=6` |

| Describe a deployment | `kubectl describe deployment web` |

| View cluster information | `kubectl cluster-info` |

| View Kind clusters | `kind get clusters` |



