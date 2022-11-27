IMAGE_NAME=		fumiyas/pdfstudioviewer
IMAGE_REGISTRY=		FIXME

XAUTHORITY_SRC=		$(HOME)/.Xauthority
XAUTHORITY=		work/.Xauthority

include ../common/Makefile

## ======================================================================

INSTALLER=PDFStudioViewer_linux64.sh
INSTALLER_URL=https://download.qoppa.com/pdfstudioviewer/$(INSTALLER)

## ----------------------------------------------------------------------

IMAGE_DEPS=$(INSTALLER)

## ----------------------------------------------------------------------

RUN_DEPS=work $(XAUTHORITY)

RUN_CONTAINER_OPTS=\
	--interactive --tty --rm \
	--env DISPLAY=$(DISPLAY) \
	--env XAUTHORITY=/.Xauthority \
	--volume /tmp/.X11-unix/:/tmp/.X11-unix \
	--volume $(PWD)/$(XAUTHORITY):/.Xauthority:ro \
	$(NULL)
RUN_CMD=pdfstudioviewer

## ======================================================================

$(INSTALLER):
	$(CURL_CMD) --remote-name $(INSTALLER_URL)

work:
	mkdir -p -m 0700 "$@"

$(XAUTHORITY): Makefile work $(XAUTHORITY_SRC)
	rm -f $@.tmp
	set -e; \
	  umask 0077; \
	  touch $(XAUTHORITY) $@.tmp; \
	  setfacl \
	    $(shell sed -n "s/^`id -un`:\([0-9]*\):.*/-m u:\1:r--/p" /etc/subuid) \
	    $@.tmp \
	  ; \
	  true
	cp $(XAUTHORITY_SRC) $@.tmp
	mv $@.tmp $@
