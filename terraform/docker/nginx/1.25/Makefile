.PYONY: help

DOCKER_IMAGE_NAME ?= nginx
DOCKER_PROJECT_NAME ?= docker.io
#DOCKER_PROJECT_NAME ?= harbor.jhee.co.kr/docker
DOCKER_IMAGE_VERSION ?= 1.25.2-perl
DOCKER_REPOSITORY ?= $(DOCKER_PROJECT_NAME)/$(DOCKER_IMAGE_NAME)
DOCKER_RUNNING_NAME ?= nginx-container

help: ## show this help
	@awk 'BEGIN {FS = ":.*?## "} /^[a-zA-Z_-]+:.*?## / {sub("\\\\n",sprintf("\n%22c"," "), $$2);printf "\033[36m%-20s\033[0m %s\n", $$1, $$2}' $(MAKEFILE_LIST)

fmt: ## Format your configuration
	@terraform fmt

validate: ## Validate your configuration
	@terraform validate

init: ## Initializing a configuration
	@terraform init

plan: ## Preview the changes
	@terraform plan

apply: plan ## Apply the configuration
	@terraform apply -auto-approve

show: ## Inspect the current state
	@terraform show

list: ## List of the resources in your project's
	@terraform state list

destroy: ## Destroy all remote objects managed by a particular Terraform configuration
	@terraform destroy

docker-run:
	@docker start $(DOCKER_RUNNING_NAME) || docker run -d -p 8080:80 --name $(DOCKER_RUNNING_NAME) $(DOCKER_REPOSITORY):$(DOCKER_IMAGE_VERSION)

docker-stop:
	@docker stop $(DOCKER_RUNNING_NAME)
