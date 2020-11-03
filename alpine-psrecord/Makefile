.PHONY: build

VER=0.1
current_dir = $$PWD
image_name = "alpine-psrecord"

build:
	docker build --build-arg --rm --squash --no-cache -t krisfoster/$(image_name):$(VER) . 

run:
	docker run --rm -it -P krisfoster/$(image_name):$(VER) /bin/sh

push:
	docker login
	docker push krisfoster/$(image_name):$(VER)

clean:
