.PYONY: help


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
	@read -p "Enter a value for project: " userInput; \
	echo "You entered: $$userInput"; \
	kubectl kustomize ./kustomize/overlays/$$userInput

deploy: ## Kustomize template deploy
	@read -p "Enter a value for project: " userInput; \
        echo "You entered: $$userInput"; \
        kubectl kustomize ./kustomize/overlays/$$userInput | kubectl apply -f -
