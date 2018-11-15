-include env_make

VERSION = $(shell basename `pwd`)
NGINX_VER = $(shell echo "${VERSION}" | grep -oE '[0-9.]+')

REPO = docksal/web
NAME = docksal-$(VERSION)

.PHONY: build test push shell run start stop logs clean release

build:
	fin docker build -t $(REPO):$(VERSION) --build-arg NGINX_VER=$(NGINX_VER) .

test:
	IMAGE=$(REPO):$(VERSION) NAME=$(NAME) ./tests/test.bats

push:
	fin docker push $(REPO):$(VERSION)

shell:
	fin docker run --rm --name $(NAME) -it $(PORTS) $(VOLUMES) $(ENV) $(REPO):$(VERSION) /bin/sh

run:
	fin docker run --rm --name $(NAME) $(PORTS) $(VOLUMES) $(ENV) $(REPO):$(VERSION)

start:
	fin docker run -d --name $(NAME) $(PORTS) $(VOLUMES) $(ENV) $(REPO):$(VERSION)

stop:
	fin docker stop $(NAME)

logs:
	fin docker logs $(NAME)

clean:
	fin docker rm -f $(NAME)

release: build
	make push -e VERSION=$(VERSION)

default: build
