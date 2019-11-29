# NodeJS-Project

This is a sample "hello world" NodeJS project.

To startup the service graph:
  - `cd services/service-graph && make k8s-graph-up`

To call the frontend application:
 - `curl -X GET http://localhost:48080`

To shutdown the service graph:
  - `cd services/service-graph && make k8s-graph-down`

## Kubernetes Commands:

See also https://kubernetes.io/docs/reference/kubectl/cheatsheet/

To create a new `namespace` (e.g. where `prod` is the new namespace):

* `kubectl create namespace prod`

List all namespaces:

* `kubectl get namespaces`

To get information about the `namespace` (where `prod` is the namespace):

* `kubectl describe namespace prod`

To switch the `namespace` context (e.g. where `prod` is the namespace):

* `kubectl config set-context --current --namespace=prod`

To get the current `namespace` context:

* `kubectl config current-context`

To delete a `namespace` (e.g. where `prod` is the namespace):

* `kubectl delete namespace prod`

-----------------------

To treat any context as local by default (see https://skaffold.dev/docs/concepts/):

* `skaffold config set --global local-cluster true`

To treat a specific context as local:

* `skaffold config set --kube-context my-profile local-cluster true`

To build/start the service graph (similar to the `docker-compose -f compose.yaml up` command):

* `skaffold dev`

To tear down the service graph (if `skaffold` is running, you make need to do `CTRL+Z` first to get to the command prompt):

* `skaffold delete`

To manually `apply` (i.e. `add`) a new spec/manifest file to a running service graph:

* `kubectl apply -f manifest.yml`

-----------------------

To install `helm`, including `tiller` (using the `Makefile` in the `service-graph` folder):

* `make install-helm`

To install `tiller` (using the `Makefile` in the `service-graph` folder):

* `make install-tiller`

To install `coredns` using `helm`:

* `make install-coredns` (using the `Makefile` in the `service-graph` folder):

-----------------------

To list the nodes on the host machine:

* `kubectl get nodes`

-----------------------

To list all pods:

* `kubectl get pods`
* `kubectl get pods -o wide`

To view a pod's configuration details (e.g. where `mypod` is the name of the pod):

* `kubectl describe pods mypod`

To get a pod's configuration details as JSON (e.g. where `mypod` is the name of the pod):

* `kubectl get pods -o json mypod`

To view the logs for a pod (e.g. where `mypod` is the name of the pod):

* `kubectl logs mypod`

To delete a pod using its name (e.g. where `mypod` is the name of the pod):

* `kubectl delete pod mypod`
* `kubectl delete pod mypod --now` (immediately, with no graceful shutdown)

To delete a pod using a reference to its manifest file:

* `kubectl delete -f manifest.yaml`

To delete all pods:

* `kubectl delete pods --all`

-----------------------

To list all deployments:

* `kubectl get deployments`
* `kubectl get deployments -o wide`

To view a deployment's configuration details (e.g. where `mydepl` is the name of the deployment):

* `kubectl describe pods mydepl`

To get a deployment's configuration details as JSON (e.g. where `mydepl` is the name of the deployment):

* `kubectl get deployments -o json mydepl`

To delete a deployment using its name (e.g. where `mydepl` is the name of the deployment):

* `kubectl delete deployment mydepl`
* `kubectl delete deployment mydepl --now` (immediately, with no graceful shutdown)

To delete a deployment using a reference to its manifest file:

* `kubectl delete -f manifest.yaml`

To delete all deployments:

* `kubectl delete deployments --all`

-----------------------

To list all services:

* `kubectl get services`
* `kubectl get services -o wide`

To view a service's configuration details (e.g. where `mysvc` is the name of the service):

* `kubectl describe services mysvc`

To get a service's configuration details as JSON (e.g. where `mysvc` is the name of the service):

* `kubectl get services -o json mysvc`

To delete a service using its name (e.g. where `mysvc` is the name of the service):

* `kubectl delete service mysvc`
* `kubectl delete service mysvc --now` (immediately, with no graceful shutdown)

To delete a service using a reference to its manifest file:

* `kubectl delete -f manifest.yaml`

To delete all services:

* `kubectl delete services --all`

-----------------------

To create a new helm chart (e.g. where `mychart` is the name of the chart):

* `helm create mychart`

To delete a helm chart (e.g. where `mychart` is the name of the chart):

* `helm delete  --purge "mychart"`

To preview how helm chart templates render (e.g. where `mychart` is the name of the chart):

* `helm install ./mychart --values=values.yaml --dry-run --debug`

Also, see https://kubernetes.io/docs/reference/kubectl/cheatsheet/
