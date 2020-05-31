#

.PHONY: $(shell egrep -o ^[a-zA-Z_-]+: $(MAKEFILE_LIST) | sed 's/://')

default: help

nfs: ## Start NFS Server
	@echo NFSへのアクセス制限はかかっていません。ファイアウォールの設定を確認してください。
	@echo 必要なら -e PERMITTED="192.168.*.*" 等の制限をかけてください。
	docker run -d --rm --name nfs -p 2049:2049 -v nfs:/nfs --privileged -e SHARED_DIRECTORY=/nfs itsthenetwork/nfs-server-alpine:latest

stop_nfs: ### Stop NFS Server
	docker stop nfs

run: ## Run workspace
	helm install k8s-workspace k8s-workspace -f config.yaml

stop: ### Stop workspace
	helm uninstall k8s-workspace

values: ### Inspect values
	helm inspect values k8s-workspace > config.yaml.default

bash: ## Run Bash on workspace
	kubectl exec -it workspace /bin/bash

help: ## This help
	@grep -Eh '^[a-zA-Z_-]+:.*?## .*$$' $(MAKEFILE_LIST) | sort | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-30s\033[0m %s\n", $$1, $$2}'
