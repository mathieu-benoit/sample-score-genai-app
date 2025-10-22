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

## Run Docker Compose.
.PHONY: compose-up
compose-up:
	docker compose up --build -d --remove-orphans

## Test directly llama Docker Model.
.PHONY: llama-model-test
llama-model-test:
	docker model run ai/llama3.2:1B-Q8_0 "Explain Docker in one sentence"