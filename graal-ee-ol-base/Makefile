.PHONY: build

REPO="krisfoster"
GRAAL_VER = "20.2.0"
JAVA_VER = "11"
current_dir = $$PWD
IMG_NAME_BASE = "graal-ee-ol-base"
IMG_NAME = "$(IMG_NAME_BASE):$(GRAAL_VER)-JDK$(JAVA_VER)"


build: copy-install-files
	docker build \
		--build-arg GRAAL_VERSION=$(GRAAL_VER) \
		--build-arg JAVA_VERSION=$(JAVA_VER) \
		--force-rm \
		--squash \
		--no-cache \
		--compress \
		--pull \
		-f Dockerfile \
		-t $(REPO)/$(IMG_NAME) .
	rm -rf ./installs

copy-install-files:
	mkdir -p installs
	cp ../installs/* ./installs

run:
	docker run --rm -it -P $(REPO)/$(IMG_NAME) /bin/bash

push:
	docker login
	docker push $(REPO)/$(IMG_NAME)

clean:
	rm -rf ./installs
