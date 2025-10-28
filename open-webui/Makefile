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

## Run main.go.
.PHONY: go-up
go-up:
	go run main.go

## Test directly llama Docker Model.
.PHONY: llama-model-test
llama-model-test:
	# docker model run ai/gemma3 "Tell me a joke about container"
	# docker model run ai/llama3.2:1B-Q8_0 "Explain Docker in one sentence"
	docker model run ai/smollm2:135M-Q4_0 "Explain Docker in one sentence"

WORKLOAD_NAME = open-webui
CONTAINER_NAME = open-webui
CONTAINER_IMAGE = ${WORKLOAD_NAME}:test

.score-compose/state.yaml:
	score-compose init \
		--no-sample \
		--provisioners ../score-provisioners/dmr-llm-model-via-service-provider.provisioners.yaml

compose.yaml: score.yaml .score-compose/state.yaml Makefile
	score-compose generate score.yaml \
		--image ghcr.io/open-webui/open-webui:main-slim \
		--publish 8080:${CONTAINER_NAME}:8080

## Generate a compose.yaml file from the score spec and launch it.
.PHONY: compose-up
compose-up: compose.yaml
	docker compose up --build -d --remove-orphans

## Test Docker Compose.
.PHONY: compose-test
compose-test: compose-up
	sleep 5
	curl http://localhost:8080
