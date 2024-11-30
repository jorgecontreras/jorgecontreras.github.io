# Kubernetes CKAD Certification study notes

## Who is it for?

This certification is for Kubernetes engineers, cloud engineers and other IT professionals responsible for building, deploying, and configuring cloud native applications with Kubernetes.

## Purpose

The Certified Kubernetes Application Developer (CKAD) can design, build and deploy cloud-native applications for Kubernetes.
A CKAD can define application resources and use Kubernetes core primitives to create/migrate, configure, expose and observe scalable applications.
The exam assumes working knowledge of container runtimes and microservice architecture.

## Curriculum

| Topic                                        | Weight  |
|----------------------------------------------|---------|
| Application Design and Build                 | 20%     |
| Application Deployment                       | 20%     |
| Application Observability and Maintenance    | 15%     |
| Application Environment, Configuration, and Security | 25%  |
| Services and Networking                      | 20%     |

> https://github.com/cncf/curriculum

## Resources I'm using

**Certified Kubernetes Application Developer (CKAD), 3rd Edition**

by Sander van Vugt | Released December 2022 | Publisher(s): Pearson | ISBN: 0138086555

A [complete guide](https://learning.oreilly.com/course/certified-kubernetes-application/9780138086558/) to learning how to run Kubernetes applications and pass the CKAD exam

**Certified Kubernetes Application Developer (CKAD) Exam Prep Labs**

By O'Reilly Media Editorial

This collection of [interactive labs](https://learning.oreilly.com/playlists/2e9fe6dc-2a05-47fe-ae0a-34d6485287cc) provides hands-on training that enhances the exam prep guidance in Benjamin Muschko's Certified Kubernetes Application Developer (CKAD) Study Guide, second edition, which walks you through the topics you need to understand for the CKAD exam.

**Kubernetes Book Club**

I'm also participating in the [Kubernetes Virtual Book Club](https://community.cncf.io/kubernetes-virtual-book-club/) hosted by CNCF. This book club is a great way to connect with other learners, discuss Kubernetes-related topics, and gain deeper insights into cloud-native practices. It's a valuable resource for preparing for the CKAD exam, as we explore Kubernetes concepts in a collaborative environment, share experiences, and solve problems together.

If you’re preparing for a Kubernetes certification or just want to learn more about cloud-native technologies, I highly recommend joining the book club.

## Kubernetes Command Cheatsheet

### Create a Pod
**Description**: Create a Pod with a specific image.  
**Command**:  
`kubectl run <pod-name> --image=<image>`  
**Example**:  
`kubectl run nginx-pod --image=nginx`  

---

### Apply a YAML File
**Description**: Apply a configuration file to the cluster.  
**Command**:  
`kubectl apply -f <file-name.yaml>`  
**Example**:  
`kubectl apply -f pod.yaml`  

---

### List Resources
**Description**: List resources like Pods, Deployments, or ConfigMaps.  
**Command**:  
`kubectl get <resource-type>`  
**Example**:  
`kubectl get pods`  

---

### Describe a Resource
**Description**: Show detailed information about a specific resource.  
**Command**:  
`kubectl describe <resource-type> <name>`  
**Example**:  
`kubectl describe pod nginx-pod`  

---

### View Logs
**Description**: View the logs of a specific Pod.  
**Command**:  
`kubectl logs <pod-name>`  
**Example**:  
`kubectl logs nginx-pod`  

---

### Check Resource Usage
**Description**: View CPU and memory usage of nodes or Pods.  
**Command**:  
`kubectl top <pods|nodes>`  
**Example**:  
`kubectl top pods`  

---

### Create a Deployment
**Description**: Create a Deployment with a specific image.  
**Command**:  
`kubectl create deployment <name> --image=<image>`  
**Example**:  
`kubectl create deployment nginx-deployment --image=nginx`  

---

### Scale a Deployment
**Description**: Scale a Deployment to a desired number of replicas.  
**Command**:  
`kubectl scale deployment <name> --replicas=<number>`  
**Example**:  
`kubectl scale deployment nginx-deployment --replicas=3`  

---

### Expose a Deployment
**Description**: Expose a Deployment as a Service.  
**Command**:  
`kubectl expose deployment <name> --type=<type> --port=<port>`  
**Example**:  
`kubectl expose deployment nginx-deployment --type=ClusterIP --port=80`  

---

### Delete a Resource
**Description**: Delete a resource such as a Pod or Deployment.  
**Command**:  
`kubectl delete <resource-type> <name>`  
**Example**:  
`kubectl delete pod nginx-pod`  

---

### Edit a Resource
**Description**: Edit a live resource configuration.  
**Command**:  
`kubectl edit <resource-type> <name>`  
**Example**:  
`kubectl edit deployment nginx-deployment`  

---

### Get Resource YAML
**Description**: Output the YAML configuration of a resource.  
**Command**:  
`kubectl get <resource-type> <name> -o yaml`  
**Example**:  
`kubectl get pod nginx-pod -o yaml`  

---

### Create a ConfigMap
**Description**: Create a ConfigMap from literal values.  
**Command**:  
`kubectl create configmap <name> --from-literal=<key>=<value>`  
**Example**:  
`kubectl create configmap app-config --from-literal=APP_ENV=production`  

---

### Create a Secret
**Description**: Create a Secret with sensitive data.  
**Command**:  
`kubectl create secret generic <name> --from-literal=<key>=<value>`  
**Example**:  
`kubectl create secret generic db-secret --from-literal=username=admin --from-literal=password=pass`  

---

### Port Forwarding
**Description**: Forward a port from a Pod to your local machine.  
**Command**:  
`kubectl port-forward <pod-name> <local-port>:<pod-port>`  
**Example**:  
`kubectl port-forward nginx-pod 8080:80`  


## To be added:

- Key Concepts
- Exercises
- Exam Tips
- Flash cards