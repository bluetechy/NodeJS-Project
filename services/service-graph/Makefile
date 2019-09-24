########################################################################
# CONSTANTS FOR MAKEFILE
########################################################################

PROJECT_ENV = dev
PROJECT_NAME = project-${PROJECT_ENV}

ROOT_PATH=`echo $(PWD) | rev | cut -d'/' -f3- | rev`

########################################################################
# RULES FOR DOCKER COMPOSE
########################################################################

# make dc-up
dc-app-up: dc-app-down
	docker-compose -p ${PROJECT_NAME} -f docker-compose-${PROJECT_ENV}.yml up
# make dc-down
dc-app-down:
	docker-compose -p ${PROJECT_NAME} -f docker-compose-${PROJECT_ENV}.yml down
	docker-compose -p ${PROJECT_NAME} -f docker-compose-${PROJECT_ENV}.yml rm -f

########################################################################
# RULES FOR KUBERNETES
########################################################################

# make k8s-app-build
k8s-app-build:
	skaffold build -f skaffold-${PROJECT_ENV}.yaml --skip-tests=true
# make k8s-app-up
k8s-app-up: k8s-app-down k8s-app-build
	helm install ../../charts/service-graph --namespace ${PROJECT_ENV} --name=${PROJECT_NAME} --values=../../charts/service-graph/values-${PROJECT_ENV}.yaml --set repository.rootDir="${ROOT_PATH}"
# make k8s-app-down
k8s-app-down:
	(helm delete ${PROJECT_NAME} --purge || true)
# make k8s-app-inspect
k8s-app-inspect:
	helm install ../../charts/service-graph --namespace ${PROJECT_ENV} --dry-run --values=../../charts/service-graph/values-${PROJECT_ENV}.yaml --set repository.rootDir="${ROOT_PATH}" --debug
# make k8s-app-log
k8s-app-log:
	kubetail "^.*depl.*" --regex

########################################################################
# INSTALLATION FOR KUBERNETES
########################################################################

# make create-admin
create-admin:
	kubectl apply -f dashboard-adminuser.yaml
	kubectl -n kube-system describe secret $(kubectl -n kube-system get secret | grep admin-user | awk '{print $1}')

# make show-admin-token
show-admin-token:
	kubectl -n kube-system describe secret $(kubectl -n kube-system get secret | grep admin-user | awk '{print $1}')

# make create-namespace
create-namespace:
	kubectl create namespace ${PROJECT_ENV}
	kubectl config set-context --current --namespace=${PROJECT_ENV}

# make install-brew
install-brew:
	/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

# make install-helm
install-helm:
	curl -L https://get.helm.sh/helm-v2.14.3-darwin-amd64.tar.gz | tar xz
	sudo mv darwin-amd64/helm /usr/local/bin/helm
	helm init --upgrade

# make install-coredns
install-coredns:
	helm install stable/coredns --name coredns --namespace=kube-system

# make install-skaffold
install-skaffold:
	curl -Lo skaffold https://storage.googleapis.com/skaffold/releases/latest/skaffold-darwin-amd64
	chmod +x skaffold
	sudo mv skaffold /usr/local/bin

# make install-tiller
install-tiller:
	helm init --upgrade

# make update-skaffold
update-skaffold: install-skaffold

# https://github.com/johanhaleby/kubetail
# make install-kubetail
install-kubetail:
	brew tap johanhaleby/kubetail && brew install --HEAD kubetail

########################################################################
# USING GUI FOR KUBERNETES
########################################################################

# https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/

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
