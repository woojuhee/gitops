.PYONY: help

DOCKER_IMAGE_NAME ?= mariadb-galera
DOCKER_PROJECT_NAME ?= harbor.jhee.co.kr/docker-devops
DOCKER_IMAGE_VERSION ?= 11.0.3-debian-11-r7-1
DOCKER_REPOSITORY ?= $(DOCKER_PROJECT_NAME)/$(DOCKER_IMAGE_NAME)
DOCKER_RUNNING_NAME ?= mariadb-galera

help: ## show this help
	@awk 'BEGIN {FS = ":.*?## "} /^[a-zA-Z_-]+:.*?## / {sub("\\\\n",sprintf("\n%22c"," "), $$2);printf "\033[36m%-20s\033[0m %s\n", $$1, $$2}' $(MAKEFILE_LIST)

build: ## Build an image from a Dockerfile
	@docker build -t $(DOCKER_REPOSITORY):$(DOCKER_IMAGE_VERSION) .

run: build ## Create and run a new container from an image
	@docker rm -f $(DOCKER_RUNNING_NAME)
	@docker run -d -p 3306:3306 -e ALLOW_EMPTY_PASSWORD=yes \
	-e MARIADB_ROOT_PASSWORD=root \
	-e MARIADB_GALERA_CLUSTER_NAME=galera -e BITNAMI_DEBUG=true \
	--name $(DOCKER_RUNNING_NAME) $(DOCKER_REPOSITORY):$(DOCKER_IMAGE_VERSION)

push: run ## Upload an image to a registry
	@docker push $(DOCKER_REPOSITORY):$(DOCKER_IMAGE_VERSION)
	@make clean

clean: ## Remove one or more resources
	@docker stop $(DOCKER_RUNNING_NAME)
	@docker rm -f $(DOCKER_RUNNING_NAME)
	@docker rmi $(DOCKER_REPOSITORY):$(DOCKER_IMAGE_VERSION)
#	@docker system prune -a -f
