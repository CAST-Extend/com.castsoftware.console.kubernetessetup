# CAST Imaging Console

CAST Console Enterprise Edition is a Web Application which provides the services required to configure and run CAST analyses remotely, on multiple analysis machines. It supports the full analysis process from registering applications to Imaging publication. This setup contains the components required to install CAST Console Enterprise Edition Services.

## Important Note

Out of the 5 pods used by Console, 4 of them must be attached to the same node of the cluster, called the ConsoleHost:

  - postgres
  - keycloak
  - dashboards
  - aip-service-registry
  - aip-gateway

The remaining pod can be placed on any node:
  - aip-node

ConsoleHost must be set in values.yaml (ConsoleHost.name).

## Pre-requisites

- Kubernetes
- helm

## Setup

Make sure your kubernetes cluster is up and helm is installed on your system.

Create kubernetes namespace where you want to install Imaging Console system

Below command will create namespace console
```
kubectl create ns console

```

Create Imaging storage
```
# A sample storage configuration is provided in console-pv.yaml and console-pvc.yaml.
# Edit the files before applying them:
#  -> adjust the physical path of each Persistent Volume to match local folders
#  -> specify the host name of the node on which the local Persistent Volumes will be created
# Important rule to follow regarding nodes:
#          This storage configuration is based on Persistent Volumes of type "local",
#          meaning they have to be attached to a specific node. 
#          When customizing the node names in console-pv.yaml, make sure that:
#          - pv-console-db-data and pv-console-restapi-domains are on node ConsoleHost.name
#          - pv-console-aip-node-cast and pv-console-aip-node-data are on same node (can be any node)
#          Furthermore, the value for ConsoleHost.name defined in values.yaml should match
#          the node selected for pv-console-db-data and pv-console-restapi-domains.
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
