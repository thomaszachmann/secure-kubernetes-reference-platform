.PHONY: fmt lint test run run-inv-tm fetch-kubeconfig

ANSIBLE_PLAYBOOK=ansible-playbook
INVENTORY ?= inventory.yml
PLAYBOOK=deploy-rke2.yml

fmt:
	@echo "Formatting..."
	@yamllint -d "{extends: default, rules: {line-length: disable}}" . || true

lint:
	@echo "Linting..."
	@ansible-lint -qq deploy-rke2.yml deploy-keepalived.yml || true

test:
	@echo "Testing..."
	ansible-playbook -i $(INVENTORY) --syntax-check deploy-rke2.yml
	ansible-playbook -i $(INVENTORY) --syntax-check deploy-keepalived.yml

run:
	@echo "Running..."
	@read -p "Enter username: " user; read -p "Enter vault password: " pass; \
	echo $$pass | ansible-playbook -i $(INVENTORY) -b -u $$user deploy-rke2.yml --diff --check --vault-pass-file=/bin/cat; \
	echo $$pass | ansible-playbook -i $(INVENTORY) -b -u $$user deploy-keepalived.yml --diff --check --vault-pass-file=/bin/cat

run-inv-tm:
	@$(MAKE) run INVENTORY=inventory_tm.yml

fetch-kubeconfig:
	@read -p "Enter username: " user; \
	ansible -u $$user -b -i $(INVENTORY) rke2-master-01 -m fetch --become -a "src=/root/.kube/config dest=./kubeconfig.yml flat=true"
