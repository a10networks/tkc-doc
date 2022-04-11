# Thunder Kubernetes Connector i.e TKC (Documents)

## Table of Contents
1. [Overview](#Overview)
2. [Directories](#Directories)


To Access *TKC latest image*, please refer to the Docker Hub Repository: 
https://hub.docker.com/r/a10networks/a10-kubernetes-connector/tags 


For additional details of the working on *TKC with A10's Application Delivery Controller (ADC) please refer to the documentation* at https://documentation.a10networks.com/Install/Software/A10_ACOS_Install/index.html#tkc 


For accessing *A10's Community Discussion Forum*, and other tips and tricks for TKC with Thunder ADC head over to: 
https://community.a10networks.com/discussion/16920/thunder-kubernetes-connector-with-thunder-adc#latest 



## Overview

Thunder Kubernetes Connector (TKC) runs inside the Kubernetes cluster as a container and automatically configures the Thunder ADC as the Pods are created and scaled up/down.

The TKC takes as input an Ingress Resource file. This file contains SLB parameters that are to be configured on the Thunder ADC, such as virtual IP (VIP), the protocol (e.g., http) and port number (e.g., 80), which the Thunder ADC will listen for user requests, and the name of the service-group that will contain the list of nodes to which the Thunder ADC will forward the traffic.

The Ingress Resource file also specifies the service within the Kubernetes cluster for which SLB configuration is to be done on the Thunder ADC. The TKC will monitor the K8s API server (kube-apiserver) for changes to this service as the corresponding Pods are created or scaled up/down and keep track of the nodes on which these Pods are running. It will then make an aXAPI (REST API) call to the Thunder ADC and automatically configure these nodes as members of the service-group on the Thunder ADC.

This greatly simplifies and automates the process of configuring the Thunder ADC as new services are deployed within the K8s cluster. It also reduces the learning curve for DevOps engineers as they can specify SLB configuration parameters using Ingress Resource file, instead of spending time learning the configuration commands for the Thunder ADC.

For sending traffic to the Pods, the A10 solution support two major modes of operation:

### Load balancing at node level: 
In this mode, the Thunder ADC sends traffic to the NodePort allocated on each node for the application. Upon reaching the node, the traffic will then be internally load balanced by the K8s cluster and sent to the Pods. In this method, the load balancing works at node level and not at the Pod level. This means that if one node has 10 Pods, and another has 20 Pods, traffic will still be distributed equally between the two nodes (assuming round-robin load balancing), leading to unequal distribution of traffic at the Pod level.

<img width="628" alt="Screen Shot 2022-04-04 at 3 48 28 PM" src="https://user-images.githubusercontent.com/63386100/161645230-6f14042f-f148-422c-9538-db13818457a3.png">


### Load balancing at Pod level: 
In an upcoming release, the TKC will be able to program the Thunder ADC to send traffic directly to the Pods, bypassing the internal load balancing mechanism of the Kubernetes cluster. This is achieved using IP in IP tunnels to the Pod network on each node. In this mode, load balancing is done at the Pod level, thereby ensuring a balanced distribution of traffic among the Pods (assuming round-robin load balancing). This option, however, requires that the Container Network Interface (CNI) plugin used with the K8s cluster supports advertising of the Pod subnet (e.g., Calico CNI plugin).

<img width="618" alt="Screen Shot 2022-04-04 at 3 48 18 PM" src="https://user-images.githubusercontent.com/63386100/161645242-a81c8046-c887-41a3-a33e-b8fe0e170b60.png">


## Key Functions of TKC:  

### Support for L4 and L7 traffic of Web and Database application type 

Standard Kubernetes Ingress solutions provide load balancing only at layer 7 (HTTP or HTTPS traffic). Sometimes, you need to expose many legacy applications which rely on TCP or UDP applications and need a way to load balance those applications. The TKC integration provides TCP and UDP traffic support apart from the standard HTTP or HTTPS Ingress. Also, it works seamlessly across multiple clouds or on-premises data centers.  

### Full application configuration support for TCP, UDP, HTTP and HTTPs  

TKC with the assist of CRDs allows you to configure all application specific features. As an example the DevOps can change the TCP port number to expose the service at the ingress, for HTTPs type traffic, there are options to offload the TLS connection on the external Load Balancer or choose a specific cipher and key size for the reverse proxy connections – all of these different details can be configured using CRDs on the TKC.  

### Advanced & High Performant traffic management  

Thunder ADC provides enterprise-grade traffic management policies like header rewrite and cipher selections for security policies to efficiently load balancing traffic at layer 7. These policies applied to Kubernetes environment is achieved with CRDs provided by TKC.  

### Tunnelling traffic across external Load Balancer and Pods for “True” Load Balancing  

IPinIP tunneling support across external Thunder ADC and Kubernetes Nodes, and routing supported with Calico CNI. TKC in this scenario configures the Pods IPs and not the service IPs on the external Thunder, this will guarantee true load balancing and Pod health monitoring to continuously identify best route for incoming traffic.  
 
### Support for HELM and Terraform to automate TKC and Thunder  

Using HELM charts or Terraform Scripts,  DevOps or NetOps team can completely automate the deployment and updates of TKC using their CI/CD process. And Terraform scripts will also help automate and scale the orchestration and configuration of Thunder ADC.    

### Supportability for Kubernetes and OpenShift clusters  

TKC with Thunder ADC is supported for Kubernetes on-prem or Kubernetes on Cloud for Azure AKS, AWS EKS and Oracle CE. TKC with Thunder ADC is also supported for Openshift on-prem with advanced routing capabilities. 

### Service of type LoadBalancer with IPAM for on-prem Kubernetes Cluster (Future versions of TKC) 

For Kubernetes hosted in on-premise bare metal Kubernetes clusters, the service of type LoadBalancer could be highly efficient for DevOps and NetOps teams to host their apps and direct the external load balancer Thunder to recieve traffic to the VIP that’s auto allocated via the IPAM. This function can be used for TCP, UDP and HTTP(s) traffic. With this you are guaranteed consistent cloud load balancing architecture with Thunder ADC and TKC for your on-premise Kubernetes clusters.  




## Directories

```
| Directory              | Summary                                          |
| crd                    | TKC Supported CRD definitions                    |
| charts                 | Helm Charts to deploy specific use-cases via TKC |
```
