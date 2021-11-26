IMAGE_NAME=		fumiyas/pdfstudioviewer
IMAGE_REGISTRY=		FIXME

include ../common/Makefile

## ======================================================================

INSTALLER=PDFStudioViewer_linux64.sh
INSTALLER_URL=https://download.qoppa.com/pdfstudioviewer/$(INSTALLER)

## ----------------------------------------------------------------------

IMAGE_DEPS=$(INSTALLER)

## ----------------------------------------------------------------------

RUN_DEPS=Xauthority

RUN_DOCKER_OPTS=\
	--env DISPLAY=$(DISPLAY) \
	--volume /tmp/.X11-unix/:/tmp/.X11-unix \
	--volume $(PWD)/Xauthority:/root/.Xauthority:ro \
	--

## ======================================================================

$(INSTALLER):
	$(CURL_CMD) --remote-name $(INSTALLER_URL)

Xauthority: $(HOME)/.Xauthority
	rm -f $@.tmp
	set -e; \
	  umask 0077; \
	  touch $(HOME)/.Xauthority $@.tmp; \
	  setfacl -m u:$(shell sed -n "s/^`id -un`:\([0-9]*\):.*/\1/p" /etc/subuid):r-- $@.tmp; \
	  true
	cp $(HOME)/.Xauthority $@.tmp
	mv $@.tmp $@
