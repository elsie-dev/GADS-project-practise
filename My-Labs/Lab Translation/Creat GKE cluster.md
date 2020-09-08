#LAB: CREATING A GKE VIA GCP CONSOLE

OVERVIEW;In this lab, you use the GCP Console to build GKE clusters and deploy a sample Pod.

Objectives;

Use the GCP Console to build and manipulate GKE clusters

Use the GCP Console to deploy a Pod

Use the GCP Console to examine the cluster and Pods


#STEPS
0. SETTING UP THE LAB.
(Run this command,replacing project ID with the one allocated)
gcloud config set project project-ID

(Run this commnad to set the zone to us-central1-a)
gcloud config set compute/zone us-central1-a 


1.DEPLOY A GKE CLUSTER.
(Run this command to create a gke cluster with 3 nodes)
gcloud container clusters create standard-cluster-1 --num-nodes=3

(After creating a cluster,we need to authenticate credentials)
gcloud container clusters get-credentials standard-cluster-1

2.MODIFY GKE CLUSTER.
(To resize cluster's nodepools run)
gcloud container clusters resize standard-clusters-1 --node-pool default-node\
--num-nodes 3


3.DEPLOY A SAMPLE WORKLOAD.
Kubernetes provides a deployment object for deploying stateless application like the Nginx webserver

(Running nginx into our cluster)
kubectl create deployment Nginx

After deploying the application,expose it to the internet to make it reachable,by Running
{kubectl expose deployment Nginx --type LoadBalancer\
--port 80 --target-port 8080}

this creates a compute engine loadbalancer traffic on port 80


4.VIEWING DETAILS ABOUT WORKLOAD ON GCP CONSOLE.
(To inspect pods run,kubectl get pods)
kubectl get pods

(To inspect Nginx workload,we run the kubectl get services)
kubectl get services Nginx

From the output populated is an external ip,that can be used to view the application withtheexposed port
http://external-ip/

