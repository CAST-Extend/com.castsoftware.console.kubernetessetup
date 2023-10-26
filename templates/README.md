# CAST Imaging Console

CAST Console Enterprise Edition is a Web Application which provides the services required to configure and run CAST analyses remotely, on multiple analysis machines. It supports the full analysis process from registering applications to Imaging publication. This setup contains the components required to install CAST Console Enterprise Edition Services.

## Pre-requisites

- Kubernetes
- helm

## Setup

Make sure your kubernetes cluster is up and helm is installed on your system.

Create kubernetes namespace where you want to install Imaging Console system

Below command will create namespace imaging
```
kubectl create ns console

```

Create Imaging storage
```
# A sample storage configuration is provided in console-pv.yaml and console-pvc.yaml.
# Edit the files before applying them:
#  -> specify the host name of the node on which the local Persistent Volumes will be created
#     (replace <imaging-host> in each 3 Persistent Volume specification)
#  -> adjust the physical path of each Persistent Volume to match local folders on <imaging-host>
# IMPORTANT NOTE: this storage configuration is based on Persistent Volumes of type "local".
#                 As "local" Persistent Volumes are by nature attached to a specific node. 
#                 When changing pod names in pv definitions, make sure that the pv which are currently
#                 grouped on same node will remain grouped the same way with the new pod names.
#                 Furthermore, the value for ConsoleHost.name defined in values.yaml should match
#                 the node selected in the nodeAffinity section of pv-console-db-data and pv-console-restapi-domains.
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
