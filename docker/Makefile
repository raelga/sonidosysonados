
TAG ?= $(shell git describe --tags)

DOCKERFILES = $(shell find * -type f -name Dockerfile)
IMAGES = $(subst /Dockerfile,,$(DOCKERFILES))

.PHONY: help

# Help

help:
	@$(MAKE) -pRrq -f $(lastword $(MAKEFILE_LIST)) : 2>/dev/null \
		| awk -v RS= -F: '/^# File/,/^# Finished Make data base/ {if ($$1 !~ "^[#.]") {print $$1}}' \
		| egrep -v -e '^[^[:alnum:]]' -e '^$@$$' | sort

$(addprefix .docker-set-,$(IMAGES)): .docker-set-%:
	$(eval registry		:= $(firstword $(subst /, ,$*)))
	$(eval repository	:= $(firstword $(subst /, ,$(subst $(registry)/,,$*))))
	$(eval image		:= $(lastword $(subst /, ,$*)))

$(addprefix build-,$(IMAGES)): build-%
	podman build -t $*:$(TAG) -t $*:latest -f $*/Dockerfile $*/context

$(addprefix push-,$(IMAGES)): push-%:
	podman push $*:$(TAG)
