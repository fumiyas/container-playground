IMAGE_NAME=		fumiyas/pdfstudioviewer
IMAGE_REGISTRY=		FIXME

ifndef XAUTHORITY
XAUTHORITY=		$(HOME)/.Xauthority
endif
XAUTHORITY_ONCE=	work/.Xauthority

## ======================================================================

INSTALLER=PDFStudioViewer_linux64.sh
INSTALLER_URL=https://download.qoppa.com/pdfstudioviewer/$(INSTALLER)

## ----------------------------------------------------------------------

IMAGE_DEPS=$(INSTALLER)

## ----------------------------------------------------------------------

RUN_DEPS=work xauthority


RUN_CONTAINER_OPTS=\
	--hostname `uname -n` \
	--interactive --tty --rm \
	--env LANG=C.UTF-8 \
	--env DISPLAY \
	--env XMODIFIERS \
	--env XAUTHORITY=/.Xauthority \
	--volume /tmp/.X11-unix/:/tmp/.X11-unix \
	--volume $(PWD)/$(XAUTHORITY_ONCE):/.Xauthority:ro \
	--volume $(PWD)/work:/work \
	$(NIL)
RUN_CMD=pdfstudioviewer

## ======================================================================

include ../common/Makefile

## ----------------------------------------------------------------------

$(INSTALLER):
	$(CURL_CMD) --remote-name $(INSTALLER_URL)

work:
	@mkdir -p -m 0700 "$@"
	@setfacl \
	  -m default:user:`id -u`:rwx \
	  -m user:0:rwx \
	  $(shell sed -n "s/^`id -un`:\([0-9]*\):.*/-m user:\1:rwx/p" /etc/subuid) \
	  $(shell sed -n "s/^dockremap:\([0-9]*\):.*/-m user:\1:rwx/p" /etc/subuid) \
	  "$@" \
	;

.PHONY: xauthority
xauthority: work
	@ set -ex; \
	  xauth="$(XAUTHORITY_ONCE)"; \
	  xauth_tmp="$$xauth.$$$$.tmp"; \
	  xauth -f "$(XAUTHORITY)" extract "$$xauth_tmp" "$(DISPLAY)"; \
	  xauth -f "$$xauth_tmp" generate "$(DISPLAY)" . trusted timeout 30; \
	  setfacl \
	    -m user:0:rw \
	    $(shell sed -n "s/^`id -un`:\([0-9]*\):.*/-m user:\1:rw/p" /etc/subuid) \
	    $(shell sed -n "s/^dockremap:\([0-9]*\):.*/-m user:\1:rw/p" /etc/subuid) \
	    "$$xauth_tmp" \
	  ; \
	  mv "$$xauth_tmp" "$$xauth"; \
	  true
