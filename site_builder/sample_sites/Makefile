.PHONY: help
help: ## Display help message
	@grep -E '^[0-9a-zA-Z_-]+\.*[0-9a-zA-Z_-]+:.*?## .*$$' $(MAKEFILE_LIST) | sort | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-30s\033[0m %s\n", $$1, $$2}'

.PHONY: build_all
build_all: ## Build AVD Configs
	ansible-playbook -i ENGINEERING/inventory.yml ENGINEERING/playbooks/build.yml --diff
	ansible-playbook -i FINANCE/inventory.yml FINANCE/playbooks/build.yml --diff
	ansible-playbook -i MARKETING/inventory.yml MARKETING/playbooks/build.yml --diff
