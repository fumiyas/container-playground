DOCKERFILE=		Containerfile

DOCKER=			docker
DOCKER_ENV=
DOCKER_OPTS=

DOCKER_CMD=		$(DOCKER_X_ENV) $(DOCKER_ENV) $(DOCKER) $(DOCKER_X_OPTS) $(DOCKER_OPTS)

DOCKER_BUILD_OPTS=
DOCKER_BUILD_CMD=	$(DOCKER_CMD) image build --file $(DOCKERFILE) $(DOCKER_BUILD_X_OPTS) $(DOCKER_BUILD_OPTS)

DOCKER_RUN_OPTS=
DOCKER_RUN_CMD=		$(DOCKER_CMD) container run $(DOCKER_RUN_X_OPTS) $(DOCKER_RUN_OPTS)

ifneq (,$(findstring docker,$(DOCKER)))
DOCKER_BUILDKIT=	1
DOCKER_X_ENV=		DOCKER_BUILDKIT=$(DOCKER_BUILDKIT)
DOCKER_BUILD_X_OPTS=	--progress=plain
endif

## ----------------------------------------------------------------------

CURL=			curl
CURL_OPTS=		--xattr
CURL_CMD=		$(CURL) $(CURL_OPTS)

## ======================================================================

.POHNY: default
default: help

.POHNY: help
help:
	@ echo 'Usage: $(MAKE) <image|run>'

.POHNY: clean
clean:
	$(RM) *.tmp $(IMAGE_DEPS) $(RUN_DEPS)

.POHNY: image
image: $(IMAGE_DEPS)
	$(DOCKER_BUILD_CMD) --tag $(IMAGE_NAME) .

.POHNY: run
run: $(RUN_DEPS)
	$(DOCKER_RUN_CMD) \
	  --interactive \
	  --tty \
	  --rm \
	  $(RUN_DOCKER_OPTS) \
	  $(IMAGE_NAME) \
	  /bin/bash \
	;
