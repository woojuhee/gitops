.PYONY: help

NAMESPACE ?= palworld
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

build: ## Kustomize template build
	@kubectl kustomize ./kustomize/overlays/$(NAMESPACE)/

sonarqube:
	@docker build \
		--build-arg GIT_BRANCH=$(GIT_BRANCH) \
		--add-host sonar.jerry.com:127.0.0.1 \
		-f Dockerfile-sonar \
		-t $(DOCKER_REPOSITORY):sonarqube .
	@docker rmi $(DOCKER_REPOSITORY):sonarqube
