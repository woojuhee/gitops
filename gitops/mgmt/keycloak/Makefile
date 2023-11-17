.PYONY: help

HELM_GENERATED_PATH ?= kustomize/base/generated
HELM_VALUE_PATH ?= helm/values.yaml
HELM_CHART_VERSION ?= 17.3.1
NAMESPACE ?= mgmt-dev
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
	@helm repo update
	@rm -rf ./$(HELM_GENERATED_PATH)/*
	@helm template keycloak --namespace $(NAMESPACE) -f ./$(HELM_VALUE_PATH) --output-dir ./$(HELM_GENERATED_PATH) bitnami/keycloak --version $(HELM_CHART_VERSION)

build: ## Kustomize template build
	@kubectl kustomize ./kustomize/overlays/mgmt-dev/

sonarqube:
	@docker build \
		--build-arg GIT_BRANCH=$(GIT_BRANCH) \
		--add-host sonar.jerry.com:127.0.0.1 \
		-f Dockerfile-sonar \
		-t $(DOCKER_REPOSITORY):sonarqube .
	@docker rmi $(DOCKER_REPOSITORY):sonarqube
