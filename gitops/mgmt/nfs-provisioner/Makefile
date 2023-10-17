.PYONY: help

HELM_GENERATED_PATH ?= kustomize/base/generated
HELM_VALUE_PATH ?= helm/values.yaml
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
	@helm repo add nfs-subdir-external-provisioner https://kubernetes-sigs.github.io/nfs-subdir-external-provisioner/
	@helm repo update
	@mkdir -p ./$(HELM_GENERATED_PATH)/
	@rm -rf ./$(HELM_GENERATED_PATH)/*
	@helm template nfs-client --namespace $(NAMESPACE) -f ./$(HELM_VALUE_PATH) --output-dir ./$(HELM_GENERATED_PATH) nfs-subdir-external-provisioner/nfs-subdir-external-provisioner  

build: ## Kustomize template build
	@kubectl kustomize ./kustomize/overlays/mgmt-dev/

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
