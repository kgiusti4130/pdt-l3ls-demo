###########################################
#     Run various Ansible playbooks       #
###########################################

.PHONY: help
help: ## Display help message
	@grep -E '^[0-9a-zA-Z_-]+\.*[0-9a-zA-Z_-]+:.*?## .*$$' $(MAKEFILE_LIST) | sort | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-30s\033[0m %s\n", $$1, $$2}'

.PHONY: site
site: ## Create all config
	ansible-playbook ./playbooks/site_builder.yml
	ansible-playbook ./playbooks/create_ztp_configs.yml
	ansible-playbook ./playbooks/create_dhcpd_configs.yml

# .PHONY: makefile
# makefile: ## makefile
# 	ansible-playbook playbooks/create_makefile.yml
