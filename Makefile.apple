# =========================
# Apple "container" Makefile
# =========================
# Usage:
#   make list
#   make build
#   make clean
#   make run
#   make restart
#
# Notes:
# - Apple `container run --name X` will FAIL if a container named X already exists
#   (even if stopped). So `run` proactively removes the named container first.
# - `buildkit` is expected to be running; we never stop/remove it.

# =========================
# App configuration
# =========================
APP_NAME     ?= app-5350
APP_IMAGE    ?= 5350:latest
APP_REPO     ?= 5350
DOCKERFILE   ?= Dockerfile.arm64

HOST_PORT    ?= 80
CONT_PORT    ?= 9080

# =========================
# Internal helpers
# =========================
# Match containers by NAME or IMAGE repo prefix (e.g., 5350 or 5350:latest).
# We parse the first two columns from `container list`:
#   ID/NAME   IMAGE
APP_IDS = $(shell \
	container list 2>/dev/null | \
	awk 'NR>1 {print $$1, $$2}' | \
	awk '$$1=="$(APP_NAME)" || $$2 ~ "^$(APP_REPO)(:|$$)" {print $$1}' \
)

# =========================
# Targets
# =========================
.PHONY: list status build stop rm clean run restart portcheck

list:
	@container list

status:
	@echo "App name:      $(APP_NAME)"
	@echo "App image:     $(APP_IMAGE)"
	@echo "Dockerfile:    $(DOCKERFILE)"
	@echo "Port mapping:  $(HOST_PORT):$(CONT_PORT)"
	@echo "Matched container IDs:"
	@echo "$(APP_IDS)"

# ---- Build image (ARM64)
build:
	@echo "Building $(APP_IMAGE) using $(DOCKERFILE)"
	@container build -t $(APP_IMAGE) -f $(DOCKERFILE) .
	@echo "Build complete"

# ---- Stop running app container(s) (by name or image match)
stop:
	@if [ -z "$(APP_IDS)" ]; then \
		echo "No running app containers found"; \
	else \
		for id in $(APP_IDS); do \
			echo "Stopping $$id"; \
			container stop $$id || true; \
		done; \
	fi

# ---- Remove app container(s) found by name or image match
rm:
	@if [ -z "$(APP_IDS)" ]; then \
		echo "No app containers to remove"; \
	else \
		for id in $(APP_IDS); do \
			echo "Removing $$id"; \
			container rm $$id || true; \
		done; \
	fi

# ---- Stop + remove
clean: stop rm

# ---- Run app (idempotent: remove existing named container first)
run:
	@echo "Ensuring no existing container named $(APP_NAME)..."
	@container rm $(APP_NAME) 2>/dev/null || true
	@echo "Running $(APP_NAME) on $(HOST_PORT) -> $(CONT_PORT)"
	@container run -d \
		--name $(APP_NAME) \
		-p $(HOST_PORT):$(CONT_PORT) \
		$(APP_IMAGE)
	@container list

# ---- Full cycle
restart: build clean run

# ---- Debug port usage (host-side)
portcheck:
	@echo "Checking who is listening on host port $(HOST_PORT)..."
	@sudo lsof -nP -iTCP:$(HOST_PORT) -sTCP:LISTEN || true
