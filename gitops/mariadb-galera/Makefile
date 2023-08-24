.PYONY: help

DOCKER_IMAGE_NAME ?= openstack-admin-console-ui
DOCKER_PROJECT_NAME ?= docker.io/openstack-pre
DOCKER_IMAGE_VERSION ?= $(shell git describe)
DOCKER_REPOSITORY ?= $(DOCKER_PROJECT_NAME)/$(DOCKER_IMAGE_NAME)
HELM_GENERATED_PATH ?= kustomize/base/generated
HELM_VALUE_PATH ?= helm/values.yaml
HELM_CHART_VERSION ?= 9.0.5
NAMESPACE ?= dev
GIT_BRANCH ?= master

help: ## show this help
	@awk 'BEGIN {FS = ":.*?## "} /^[a-zA-Z_-]+:.*?## / {sub("\\\\n",sprintf("\n%22c"," "), $$2);printf "\033[36m%-20s\033[0m %s\n", $$1, $$2}' $(MAKEFILE_LIST)

check-git-clean: ## Check git
	@if [ -z "`git status --porcelain .`" ]; \
		then \
		echo "Working directory is clean"; \
		else \
		echo "Working directory is not clean. "; exit 1; \
	fi
generated: ## Helm chart template generated
	@helm repo add bitnami https://charts.bitnami.com/bitnami
	@helm repo update
	@rm -rf ./$(HELM_GENERATED_PATH)/*
	@helm template mariadb --namespace $(NAMESPACE) -f ./$(HELM_VALUE_PATH) --output-dir ./$(HELM_GENERATED_PATH) bitnami/mariadb-galera --version $(HELM_CHART_VERSION)

build-image: ## Build image
	@docker build -t $(DOCKER_REPOSITORY):latest .
	@docker tag $(DOCKER_REPOSITORY):latest $(DOCKER_REPOSITORY):$(DOCKER_IMAGE_VERSION)

push-image: ## check-git-clean build-image ## Push image
	@docker push $(DOCKER_REPOSITORY):$(DOCKER_IMAGE_VERSION)

run-image: build-image ## Run image
	@docker run --rm -po 8080:1234 \
	$(DOCKER_REPOSITORY):$(DOCKER_IMAGE_VERSION)

sonarqube:
	@docker build \
		--build-arg GIT_BRANCH=$(GIT_BRANCH) \
		--add-host sonar.jerry.com:127.0.0.1 \
		-f Dockerfile-sonar \
		-t $(DOCKER_REPOSITORY):sonarqube .
	@docker rmi $(DOCKER_REPOSITORY):sonarqube
