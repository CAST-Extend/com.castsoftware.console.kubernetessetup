# CAST Imaging Console

CAST Console Enterprise Edition is a Web Application which provides the services required to configure and run CAST analyses remotely, on multiple analysis machines. It supports the full analysis process from registering applications to Imaging publication. This setup contains the components required to install CAST Console Enterprise Edition Services.

## Pre-requisites

- Kubernetes
- helm

## Important Note

Out of the 7 pods used by Console, 5 must be attached to the same node of the cluster, called the ConsoleHost:

  - postgres
  - keycloak
  - dashboards
  - aip-service-registry
  - aip-gateway

ConsoleHost must be set in variable ConsoleHost.name defined in values.yaml

The remaining pods can go on any node:
  - aip-node
  - extend-proxy

## Setup

Make sure your kubernetes cluster is up and helm is installed on your system.

Create kubernetes namespace where you want to install Imaging Console system

Below command will create namespace console
```
kubectl create ns console

```

Create Console storage
```
# A sample storage configuration is provided in console-pv.yaml and console-pvc.yaml.
# Edit those files before applying them:
#  -> adjust the physical path of each Persistent Volume to match your local folders
#  -> specify the node on which each local Persistent Volume will be created (node affinity)
# Important rule to follow regarding node affinity:
#          This storage configuration is based on Persistent Volumes of type "local",
#          meaning they have to be attached to a specific node. 
#          When customizing the node names in console-pv.yaml, make sure that:
#          - pv-console-db-data and pv-console-restapi-domains are placed on node defined in ConsoleHost.name variable
#          - pv-console-aip-node-cast and pv-console-aip-node-data are placed on same node (can be any node)
# To apply the configuration:

kubectl apply -f console-pv.yaml
kubectl apply -f console-pvc.yaml

# Check PV and PVC status:

kubectl get pv
kubectl get pvc

```

Run below helm command to install Console
```
helm install console --namespace console --set version=2.10.1 .

# Get pods status in kubernetes:

kubectl get pods -n console

# Once all pods are "Running", access Imaging Console on http://<ConsoleHost.name>:8081
# and keycloak on http://<ConsoleHost.name>:8086
```
