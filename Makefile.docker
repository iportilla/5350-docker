# =========================
# Docker Makefile
# =========================
# This Makefile provides a simple, repeatable workflow for building and running
# this app with Docker (Docker Desktop, Colima, or Linux Docker Engine).
#
# Common use:
#   make restart
#   make logs
#   make shell
#
# NOTE:
# - Docker *can* reuse a container name if you remove the old container first.
# - This Makefile removes an existing container named APP_NAME before running.

# -------------------------
# App configuration
# -------------------------
APP_NAME     ?= app-5350
APP_IMAGE    ?= 5350:latest
DOCKERFILE   ?= Dockerfile
HOST_PORT    ?= 80
CONT_PORT    ?= 9080

# Optional environment file (uncomment to use)
# ENV_FILE    ?= .env

# -------------------------
# Helpers
# -------------------------
# Find containers that match APP_NAME or were created from APP_IMAGE
APP_IDS = $(shell docker ps -a --format '{{.ID}} {{.Names}} {{.Image}}' | 	awk '$$2=="$(APP_NAME)" || $$3=="$(APP_IMAGE)" {print $$1}')

# -------------------------
# Targets
# -------------------------
.PHONY: help list status build stop rm clean run restart logs shell portcheck

help:
	@echo "Targets:"
	@echo "  make list       - list containers"
	@echo "  make build      - build image"
	@echo "  make run        - run container (detached)"
	@echo "  make logs       - follow logs"
	@echo "  make shell      - open a shell in the running container"
	@echo "  make clean      - stop + remove matching containers"
	@echo "  make restart    - build + clean + run"
	@echo "  make portcheck  - show what is listening on HOST_PORT"

list:
	@docker ps -a

status:
	@echo "APP_NAME=$(APP_NAME)"
	@echo "APP_IMAGE=$(APP_IMAGE)"
	@echo "DOCKERFILE=$(DOCKERFILE)"
	@echo "PORTS=$(HOST_PORT):$(CONT_PORT)"
	@echo "Matched container IDs:"
	@echo "$(APP_IDS)"

build:
	@echo "Building $(APP_IMAGE) using $(DOCKERFILE)"
	@docker build -t $(APP_IMAGE) -f $(DOCKERFILE) .

stop:
	@if [ -z "$(APP_IDS)" ]; then 		echo "No matching containers to stop"; 	else 		for id in $(APP_IDS); do 			echo "Stopping $$id"; 			docker stop $$id >/dev/null || true; 		done; 	fi

rm:
	@if [ -z "$(APP_IDS)" ]; then 		echo "No matching containers to remove"; 	else 		for id in $(APP_IDS); do 			echo "Removing $$id"; 			docker rm $$id >/dev/null || true; 		done; 	fi

clean: stop rm

run:
	@echo "Ensuring no existing container named $(APP_NAME)..."
	@docker rm -f $(APP_NAME) >/dev/null 2>&1 || true
	@echo "Running $(APP_NAME) on $(HOST_PORT) -> $(CONT_PORT)"
	@docker run -d 		--name $(APP_NAME) 		-p $(HOST_PORT):$(CONT_PORT) 		$(APP_IMAGE)
	@docker ps --filter "name=$(APP_NAME)"

restart: build clean run

logs:
	@docker logs -f $(APP_NAME)

shell:
	@docker exec -it $(APP_NAME) sh

portcheck:
	@echo "Checking who is listening on host port $(HOST_PORT)..."
	@sudo lsof -nP -iTCP:$(HOST_PORT) -sTCP:LISTEN || true
