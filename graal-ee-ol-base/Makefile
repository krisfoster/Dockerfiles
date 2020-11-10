.PHONY: build

REPO := "krisfoster"
GRAAL_VER := "20.2.0"
JAVA_VER := "11"
IMG_NAME_BASE := "graal-ee-ol-base"

current_dir = $$PWD
IMG_NAME := "$(IMG_NAME_BASE):$(GRAAL_VER)-JDK$(JAVA_VER)"
TAR_FILE := "graalvm-ee-java$(JAVA_VER)-linux-amd64-$(GRAAL_VER).tar.gz"
LLVM_JAR := "llvm-toolchain-installable-java$(JAVA_VER)-linux-amd64-$(GRAAL_VER).jar"
NATIVE_IMAGE_JAR := "native-image-installable-svm-svmee-java$(JAVA_VER)-linux-amd64-$(GRAAL_VER).jar"

build: copy-install-files
	docker build \
		--build-arg GRAAL_VERSION=$(GRAAL_VER) \
		--build-arg JAVA_VERSION=$(JAVA_VER) \
		--force-rm \
		--no-cache \
		--squash \
		--compress \
		--pull \
		-f Dockerfile \
		-t $(REPO)/$(IMG_NAME) .
	rm -rf ./installs

tst:
	@echo $(IMG_NAME)
	@echo $(TAR_FILE)

copy-install-files:
	mkdir -p installs
	cp ../installs/$(TAR_FILE) ./installs
	cp ../installs/$(LLVM_JAR) ./installs
	cp ../installs/$(NATIVE_IMAGE_JAR) ./installs

run:
	docker run --rm -it -P $(REPO)/$(IMG_NAME) /bin/bash

push:
	docker login
	docker push $(REPO)/$(IMG_NAME)

clean:
	rm -rf ./installs
