###########################################
# Run AVD with various tags               #
# #########################################

.PHONY: help
help: ## Display help message
	@grep -E '^[0-9a-zA-Z_-]+\.*[0-9a-zA-Z_-]+:.*?## .*$$' $(MAKEFILE_LIST) | sort | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-30s\033[0m %s\n", $$1, $$2}'

# .PHONY: anta
# anta: ## Run ANTA playbook
# 	ansible-playbook playbooks/anta_validate.yml

.PHONY: build
build: ## Build AVD Configs
	ansible-playbook playbooks/build.yml --diff

.PHONY: cvaas_facts
cvaas_facts: ## Get the facts
	ansible-playbook playbooks/cvaas_facts.yml --ask-vault-pass

.PHONY: cvaas
cvaas: ## Build AVD and push configs to CVaaS
	ansible-playbook playbooks/cvaas_deploy.yml --ask-vault-pass

# .PHONY: deploy
# deploy: ## Push configs to devices via eAPI
# 	ansible-playbook playbooks/deploy.yml

.PHONY: facts
facts: ## Get facts from devices
	ansible-playbook playbooks/eos_facts.yml -vvv

.PHONY: snapshot
snapshot: ## Take snapshot of fabric
	ansible-playbook playbooks/eos_snapshot.yml
