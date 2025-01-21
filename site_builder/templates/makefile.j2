###########################################
# Run AVD with various tags               #
# #########################################

.PHONY: help
help: ## Display help message
	@grep -E '^[0-9a-zA-Z_-]+\.*[0-9a-zA-Z_-]+:.*?## .*$$' $(MAKEFILE_LIST) | sort | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-30s\033[0m %s\n", $$1, $$2}'

.PHONY: build
build: ## Build AVD Configs
	ansible-playbook playbooks/build.yml --diff

.PHONY: cvaas_facts
cvaas_facts: ## Get the facts
	ansible-playbook playbooks/cvaas_facts.yml --ask-vault-pass

# .PHONY: cvaas
# cvaas: ## Build AVD and push configs to CVaaS
# 	ansible-playbook playbooks/cvaas_deploy.yml
