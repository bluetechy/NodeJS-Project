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
dc-up: dc-down
	docker-compose -p ${PROJECT_NAME} -f docker-compose-${PROJECT_ENV}.yml up
# make dc-down
dc-down:
	docker-compose -p ${PROJECT_NAME} -f docker-compose-${PROJECT_ENV}.yml down
	docker-compose -p ${PROJECT_NAME} -f docker-compose-${PROJECT_ENV}.yml rm -f

########################################################################
# RULES FOR KUBERNETES
########################################################################

# make k8s-build
k8s-build:
	skaffold build -f skaffold-${PROJECT_ENV}.yaml --skip-tests=true
# make k8s-up
k8s-up: k8s-down k8s-build
	helm install ../../charts/service-graph --namespace ${PROJECT_ENV} --name=${PROJECT_NAME} --values=../../charts/service-graph/values-${PROJECT_ENV}.yaml --set repository.rootDir="${ROOT_PATH}"
# make k8s-down
k8s-down:
	(helm delete ${PROJECT_NAME} --purge || true)
# make k8s-inspect
k8s-inspect:
	helm install ../../charts/service-graph --namespace ${PROJECT_ENV} --dry-run --values=../../charts/service-graph/values-${PROJECT_ENV}.yaml --set repository.rootDir="${ROOT_PATH}" --debug
# make k8s-log
k8s-log:
	kubetail "^.*depl.*" --regex

########################################################################
# INSTALLATION FOR KUBERNETES
########################################################################

#
# Create or replace registry secret
#
create-secret:
	kubectl delete secret --ignore-not-found ${AWS_SECRET_NAME}
	kubectl create secret docker-registry ${AWS_SECRET_NAME} \
 		--docker-server=https://${AWS_ACCOUNT}.dkr.ecr.${AWS_REGION}.amazonaws.com \
 		--docker-username=AWS \
 		--docker-password="${AWS_TOKEN}" \
 		--docker-email="${AWS_EMAIL}"

install-brew:
	/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

install-helm:
	curl -L https://get.helm.sh/helm-v2.14.3-darwin-amd64.tar.gz | tar xz
	sudo mv darwin-amd64/helm /usr/local/bin/helm
	helm init --upgrade

install-coredns:
	helm install stable/coredns --name coredns --namespace=kube-system

# make installl-skaffold
install-skaffold:
	curl -Lo skaffold https://storage.googleapis.com/skaffold/releases/latest/skaffold-darwin-amd64
	chmod +x skaffold
	sudo mv skaffold /usr/local/bin

# https://github.com/helm/charts/tree/master/stable/prometheus-operator
install-prometheus:
	helm install stable/prometheus-operator --name prometheus-operator --namespace monitoring

# tiller get installed in --namespace=kube-system
install-tiller:
	helm init --upgrade

update-skaffold: install-skaffold

# https://github.com/johanhaleby/kubetail
install-kubetail:
	brew tap johanhaleby/kubetail && brew install --HEAD kubetail
