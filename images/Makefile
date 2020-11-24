USER=goffinet
TAG=latest

.DEFAULT_GOAL := all

build:
	docker build -t $(USER)/${IMAGE}:${TAG} -f ${IMAGE}/Dockerfile .

push:
	docker push $(USER)/${IMAGE}:${TAG}

build_and_push: build push

build_all:
	IMAGE=centos-8-ansible-docker-host $(MAKE) build
	IMAGE=ansible-controller $(MAKE) build

push_all:
	IMAGE=centos-8-ansible-docker-host $(MAKE) push
	IMAGE=ansible-controller $(MAKE) push

all: build_all push_all
