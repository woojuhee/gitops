.PYONY: help

HELM_GENERATED_PATH ?= kustomize/base/generated
HELM_VALUE_PATH ?= helm/values.yaml
HELM_CHART_VERSION ?= v24.3.0
NAMESPACE ?= gpu-operator
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
	@helm repo add nvidia https://helm.ngc.nvidia.com/nvidia
	@rm -rf ./$(HELM_GENERATED_PATH)/*
	@helm template gpu-operator --namespace $(NAMESPACE) -f ./$(HELM_VALUE_PATH) --output-dir ./$(HELM_GENERATED_PATH) nvidia/gpu-operator --version $(HELM_CHART_VERSION)

build: ## Kustomize template build
	@kubectl kustomize ./kustomize/overlays/$(NAMESPACE)/

deploy:
	@kubectl apply -k ./kustomize/overlays/$(NAMESPACE)/
