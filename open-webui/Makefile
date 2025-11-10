# Disable all the default make stuff
MAKEFLAGS += --no-builtin-rules
.SUFFIXES:

## Display a list of the documented make targets
.PHONY: help
help:
	@echo Documented Make targets:
	@perl -e 'undef $$/; while (<>) { while ($$_ =~ /## (.*?)(?:\n# .*)*\n.PHONY:\s+(\S+).*/mg) { printf "\033[36m%-30s\033[0m %s\n", $$2, $$1 } }' $(MAKEFILE_LIST) | sort

.PHONY: .FORCE
.FORCE:

## Test directly llama Docker Model.
.PHONY: llama-model-test
llama-model-test:
	# docker model run ai/gemma3 "Tell me a joke about container"
	# docker model run ai/llama3.2:1B-Q8_0 "Explain Docker in one sentence"
	# ollama run gemma3 "What's in this image? /Users/jmorgan/Desktop/smile.png"
	docker model run ai/smollm2:135M-Q4_0 "Explain Docker in one sentence"

WORKLOAD_NAME = open-webui
CONTAINER_NAME = open-webui
CONTAINER_IMAGE = ${WORKLOAD_NAME}:test

## Deploy score-dmr.yaml
.PHONY: dmr-up
dmr-up:
	score-compose init \
		--no-sample \
		--provisioners https://raw.githubusercontent.com/score-spec/community-provisioners/refs/heads/main/llm-model/score-compose/10-dmr-llm-model-via-service-provider.provisioners.yaml
	score-compose generate score-dmr.yaml \
		--image ghcr.io/open-webui/open-webui:latest-slim \
		--publish 8080:${CONTAINER_NAME}:8080
	docker compose up -d --wait --remove-orphans

## Deploy score-dmr.yaml and test localhost:8080
.PHONY: dmr-test
dmr-test: dmr-up
	curl http://localhost:8080

## Deploy score-drm-ro.yaml
.PHONY: dmr-ro-up
dmr-ro-up:
	score-compose init \
		--no-sample \
		--provisioners https://raw.githubusercontent.com/score-spec/community-provisioners/refs/heads/main/llm-model/score-compose/10-dmr-llm-model-via-service-provider.provisioners.yaml
	score-compose generate score-dmr-ro.yaml \
		--image ghcr.io/open-webui/open-webui:latest-slim \
		--publish 8080:${CONTAINER_NAME}:8080
	sudo yq e -i '.services.${WORKLOAD_NAME}-${CONTAINER_NAME}.read_only = true' compose.yaml
	docker compose up -d --wait --remove-orphans

## Deploy score-ollama.yaml
.PHONY: ollama-up
ollama-up:
	score-compose init \
		--no-sample \
		--patch-templates https://raw.githubusercontent.com/score-spec/community-patchers/refs/heads/main/score-compose/ollama.tpl \
		--provisioners https://raw.githubusercontent.com/score-spec/community-provisioners/refs/heads/main/llm-model/score-compose/10-ollama-llm-model-service.provisioners.yaml
	score-compose generate score-ollama.yaml \
		--image ghcr.io/open-webui/open-webui:latest-slim \
		--publish 8080:${CONTAINER_NAME}:8080
	docker compose up -d --wait --remove-orphans

## Deploy score-ollama-ro.yaml
.PHONY: ollama-ro-up
ollama-ro-up:
	score-compose init \
		--no-sample \
		--patch-templates https://raw.githubusercontent.com/score-spec/community-patchers/refs/heads/main/score-compose/ollama.tpl \
		--provisioners https://raw.githubusercontent.com/score-spec/community-provisioners/refs/heads/main/llm-model/score-compose/10-ollama-llm-model-service.provisioners.yaml
	score-compose generate score-ollama-ro.yaml \
		--image ghcr.io/open-webui/open-webui:latest-slim \
		--publish 8080:${CONTAINER_NAME}:8080
	sudo yq e -i '.services.${WORKLOAD_NAME}-${CONTAINER_NAME}.read_only = true' compose.yaml
	docker compose up -d --wait --remove-orphans

## Test Docker Compose.
.PHONY: compose-test
compose-test: 
	curl http://localhost:8080

## Delete the containers running via compose down.
.PHONY: compose-down
compose-down:
	docker compose down -v --remove-orphans || true
