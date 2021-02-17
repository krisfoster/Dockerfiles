.PHONY: build

REPO := "krisfoster"
GRAAL_VER := "21.0.0.2"
JAVA_VER := "8"
IMG_NAME_SUPER_BASE := "graal-ee-ol-base"
IMG_NAME_BASE := "graal-ee-ol-base-debug"

current_dir = $$PWD
IMG_NAME := "$(IMG_NAME_BASE):$(GRAAL_VER)-JDK$(JAVA_VER)"
TAR_FILE := "graalvm-ee-java$(JAVA_VER)-linux-amd64-$(GRAAL_VER).tar.gz"
LLVM_JAR := "llvm-toolchain-installable-java$(JAVA_VER)-linux-amd64-$(GRAAL_VER).jar"
NATIVE_IMAGE_JAR := "native-image-installable-svm-svmee-java$(JAVA_VER)-linux-amd64-$(GRAAL_VER).jar"
IMG_NAME_SUPER := "$(IMG_NAME_SUPER_BASE):$(GRAAL_VER)-JDK$(JAVA_VER)"

build:
	docker build \
		--build-arg IMG_NAME_SUPER=$(IMG_NAME_SUPER) \
		--build-arg IMG_NAME_BASE=$(IMG_NAME_BASE) \
		--compress \
		--pull \
		-f Dockerfile \
		-t $(REPO)/$(IMG_NAME) .
	rm -rf ./installs

tst:
	@echo $(IMG_NAME)
	@echo $(TAR_FILE)

run:
	docker run --rm -it -P $(REPO)/$(IMG_NAME) /bin/bash

push:
	docker login
	docker push $(REPO)/$(IMG_NAME)

clean:
	rm -rf ./installs
