.PYONY: help

DOCKER_IMAGE_NAME ?= nginx
DOCKER_PROJECT_NAME ?= harbor.jhee.co.kr/docker-base
DOCKER_IMAGE_VERSION ?= 1.25.2-perl-1
DOCKER_REPOSITORY ?= $(DOCKER_PROJECT_NAME)/$(DOCKER_IMAGE_NAME)
DOCKER_RUNNING_NAME ?= nginx-container

help: ## show this help
	@awk 'BEGIN {FS = ":.*?## "} /^[a-zA-Z_-]+:.*?## / {sub("\\\\n",sprintf("\n%22c"," "), $$2);printf "\033[36m%-20s\033[0m %s\n", $$1, $$2}' $(MAKEFILE_LIST)

build: ## Build an image from a Dockerfile
	@docker build -t $(DOCKER_REPOSITORY):$(DOCKER_IMAGE_VERSION) .

run: build ## Create and run a new container from an image
	@docker rm -f $(DOCKER_RUNNING_NAME)
	@docker run -d -p 8080:80 --name $(DOCKER_RUNNING_NAME) $(DOCKER_REPOSITORY):$(DOCKER_IMAGE_VERSION) --label app=$(DOCKER_RUNNING_NAME)

push: run ## Upload an image to a registry
	@docker push $(DOCKER_REPOSITORY):$(DOCKER_IMAGE_VERSION)
	@make clean

clean: ## Remove one or more resources
	@docker rm -f $(DOCKER_RUNNING_NAME)
	@docker rmi $(DOCKER_REPOSITORY):$(DOCKER_IMAGE_VERSION)
	@docker system prune --filter label=app=$(DOCKER_RUNNING_NAME)
