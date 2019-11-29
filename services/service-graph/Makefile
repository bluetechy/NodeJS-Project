########################################################################
# CONSTANTS FOR MAKEFILE
########################################################################

PROJECT_UID = project
PROJECT_ENV = dev
PROJECT_NAME = ${PROJECT_UID}-${PROJECT_ENV}

ROOT_PATH=`echo $(PWD) | rev | cut -d'/' -f3- | rev`

########################################################################
# RULES FOR KUBERNETES
########################################################################

# make k8s-create-namespace
k8s-create-namespace:
	kubectl create namespace ${PROJECT_ENV}
	kubectl config set-context --current --namespace=${PROJECT_ENV}

# make k8s-switch-namespace
k8s-switch-namespace:
	kubectl config set-context --current --namespace=${PROJECT_ENV}

# make k8s-graph-build
k8s-graph-build:
	-git submodule update --init --recursive
	skaffold build -f skaffold-${PROJECT_ENV}.yaml --skip-tests=true

# make k8s-graph-up
k8s-graph-up: k8s-graph-down k8s-graph-build
	helm install ../../charts/service-graph --namespace ${PROJECT_ENV} --name=${PROJECT_NAME} --values=../../charts/service-graph/values-${PROJECT_ENV}.yaml --set repository.rootDir="${ROOT_PATH}"

# make k8s-graph-down
k8s-graph-down:
	(helm delete ${PROJECT_NAME} --purge || true) &> /dev/null

# make k8s-graph-inspect
k8s-graph-inspect:
	helm install ../../charts/service-graph --namespace ${PROJECT_ENV} --dry-run --values=../../charts/service-graph/values-${PROJECT_ENV}.yaml --set repository.rootDir="${ROOT_PATH}" --debug

# make k8s-graph-log
k8s-graph-log:
	kubetail "^.*depl.*" --regex --namespace ${PROJECT_ENV}

# make k8s-graph-cleanup
k8s-graph-cleanup:
	kubectl delete --all services --namespace=${PROJECT_ENV} --now
	kubectl delete --all deployments --namespace=${PROJECT_ENV} --now
	kubectl delete --all pods --namespace=${PROJECT_ENV} --now

# make k8s-ps-cleanup
k8s-ps-cleanup:
	docker stop $$(docker ps -a -q)
	docker rm $$(docker ps -a -q)

# make k8s-gui-up
k8s-gui-up:
	kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0-beta1/aio/deploy/recommended.yaml
	kubectl proxy

# make k8s-gui-down
k8s-gui-down:
	kubectl delete -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0-beta1/aio/deploy/recommended.yaml

# make k8s-gui-open
k8s-gui-open:
	open http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/

# make k8s-create-admin
k8s-create-admin:
	kubectl apply -f dashboard-adminuser.yaml
	kubectl -n kube-system describe secret $(kubectl -n kube-system get secret | grep admin-user | awk '{print $1}')

# make k8s-show-admin-token
k8s-show-admin-token:
	kubectl -n kube-system describe secret $(kubectl -n kube-system get secret | grep admin-user | awk '{print $1}')

# make k8s-services
k8s-services:
	kubectl get services --namespace=${PROJECT_ENV}

# make k8s-deployments
k8s-deployments:
	kubectl get deployments --namespace=${PROJECT_ENV}

# make k8s-pods
k8s-pods:
	kubectl get pods --namespace=${PROJECT_ENV}

########################################################################

#https://skaffold.dev/docs/references/cli/

skaffold-graph-up:
	skaffold dev -f skaffold-${PROJECT_ENV}.yaml --no-prune=true
skaffold-graph-up-debug:
	skaffold dev -f skaffold-${PROJECT_ENV}.yaml --no-prune=true -v debug
skaffold-graph-down:
	skaffold delete -f skaffold-${PROJECT_ENV}.yaml
skaffold-graph-inspect:
	helm install ../../charts/service-graph --dry-run --values=../../charts/service-graph/values-${PROJECT_ENV}.yaml --set repository.rootDir="${ROOT_PATH}" --debug
skaffold-graph-log:
	kubetail "^.*depl.*" --regex --namespace ${PROJECT_ENV}

########################################################################
# INSTALLATION FOR KUBERNETES
########################################################################

# make install-brew
install-brew:
	/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

# make install-eksctl
install-eksctl:
	curl --silent --location "https://github.com/weaveworks/eksctl/releases/download/latest_release/eksctl_$$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
	sudo mv /tmp/eksctl /usr/local/bin

# make install-helm
install-helm:
	curl -L https://get.helm.sh/helm-v2.14.3-darwin-amd64.tar.gz | tar xz
	sudo mv darwin-amd64/helm /usr/local/bin/helm

# make install-tiller
install-tiller:
	helm init --upgrade

# make install-coredns
install-coredns:
	helm install stable/coredns --name coredns --namespace=kube-system

# make install-skaffold
install-skaffold:
	curl -Lo skaffold https://storage.googleapis.com/skaffold/releases/latest/skaffold-darwin-amd64
	chmod +x skaffold
	sudo mv skaffold /usr/local/bin

# make update-skaffold
update-skaffold: install-skaffold

# https://github.com/johanhaleby/kubetail
# make install-kubetail
install-kubetail:
	brew tap johanhaleby/kubetail && brew install --HEAD kubetail
