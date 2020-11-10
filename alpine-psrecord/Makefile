.PHONY: build

VER=0.1
current_dir = $$PWD
image_name = "krisfoster/alpine-psrecord"

build:
	docker build --build-arg --force-rm --compress --no-cache -t $(image_name):$(VER) .

run:
	docker run --rm -it -P $(image_name):$(VER) /bin/sh

push:
	docker login
	docker push $(image_name):$(VER)

clean:
