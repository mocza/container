SHELL := /bin/bash
.PHONY: clean-secrets mkdir-secrets get-secrets secrets env

all: secrets 

secrets: clean-secrets mkdir-secrets get-secrets

clean-secrets:
#	@[[ -d secrets ]] && rm -rf secrets || true
	@if [ -d secrets ]; then \
		rm -rf secrets; \
	fi

mkdir-secrets:
	@mkdir -p secrets

get-secrets:
	@cd secrets; \
	docker run --rm -it -e "BWS_ACCESS_TOKEN" bitwarden/bws secret list --color no | jq -c '.[]' | \
	while read -r json; do \
		key=$$(echo $$json | jq -r '.key'); \
		value=$$(echo $$json | jq -r '.value'); \
		echo "$$value" > "$$key"; \
	done

# Portainer deploying from docker compose doesn't seem to find files in the current work directory
env:
	@echo "CWD=$(PWD)" > .env
