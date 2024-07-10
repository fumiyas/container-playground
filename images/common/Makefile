CONTAINERFILE=		Containerfile

DOCKER=			docker
DOCKER_BUILDKIT=	1
ROOTLESSKIT=		rootlesskit

PODMAN=			podman
BUILDAH=		buildah

## ======================================================================

C_MANAGER=		$(DOCKER)
C_MANAGER_ENV=
C_MANAGER_OPTS=
C_MANAGER_CMD=		$(C_MANAGER_X_ENV) $(C_MANAGER_ENV) $(C_MANAGER) $(C_MANAGER_X_OPTS) $(C_MANAGER_OPTS)

C_MANAGER_RUN_ENV=
C_MANAGER_RUN_OPTS=
C_MANAGER_RUN_CMD=	$(C_MANAGER_X_RUN_ENV) $(C_MANAGER_RUN_ENV) $(C_MANAGER_CMD) run $(C_MANAGER_RUN_X_OPTS) $(C_MANAGER_RUN_OPTS)

## ----------------------------------------------------------------------

C_BUILDER_ENV=
C_BUILDER_OPTS=
C_BUILDER_CMD=		$(C_BUILDER_X_ENV) $(C_BUILDER_ENV) $(C_BUILDER) $(C_BUILDER_X_OPTS) $(C_BUILDER_OPTS)

C_BUILDER_BUILD_ENV=
C_BUILDER_BUILD_OPTS=
C_BUILDER_BUILD_CMD=	$(C_BUILDER_BUILD_X_ENV) $(C_BUILDER_BUILD_ENV) $(C_BUILDER_CMD) build --file $(CONTAINERFILE) $(C_BUILDER_BUILD_X_OPTS) $(C_BUILDER_BUILD_OPTS)

C_BUILDER_PUSH_ENV=
C_BUILDER_PUSH_OPTS=
C_BUILDER_PUSH_CMD=	$(C_BUILDER_PUSH_X_ENV) $(C_BUILDER_PUSH_ENV) $(C_BUILDER_CMD) push $(C_BUILDER_PUSH_X_OPTS) $(C_BUILDER_PUSH_OPTS)

## ----------------------------------------------------------------------

ifneq (,$(findstring docker,$(C_MANAGER)))
C_BUILDER=		$(C_MANAGER)
C_UNSHARE=		$(ROOTLESSKIT)
endif

ifneq (,$(findstring podman,$(C_MANAGER)))
C_BUILDER=		$(BUILDAH)
C_UNSHARE=		$(BUILDAH) unshare
endif

ifneq (,$(findstring docker,$(C_BUILDER)))
C_BUILDER_X_ENV=	DOCKER_BUILDKIT=$(DOCKER_BUILDKIT)
C_BUILDER_BUILD_X_OPTS=	--progress=plain
endif

## ----------------------------------------------------------------------

CURL=			curl
CURL_OPTS=		--xattr
CURL_CMD=		$(CURL) $(CURL_OPTS)

## ----------------------------------------------------------------------

RUN_CONTAINER_OPTS?=	--interactive --tty --rm
RUN_CMD?=		bash

## ======================================================================

.PHONY: default
default: help

.PHONY: help
help:
	@ echo 'Usage: $(MAKE) <image|run>'

.PHONY: clean
clean:
	$(RM) *.tmp $(IMAGE_DEPS) $(RUN_DEPS)

.PHONY: image
image: $(IMAGE_DEPS)
	$(C_BUILDER_BUILD_CMD) --tag $(IMAGE_NAME) .

.PHONY: push
push:
	$(C_BUILDER_PUSH_CMD) $(IMAGE_NAME)

.PHONY: run
run: $(RUN_DEPS)
	$(C_MANAGER_RUN_CMD) \
	  $(RUN_CONTAINER_OPTS) \
	  $(IMAGE_NAME) \
	  $(RUN_CMD) \
	;
