.PHONY: slim-jim

graal_ver = "20.2.0"
current_dir = $$PWD
image_name = "graal-ee-java11-ubuntu"

chunky-monkey: copy-install-files
	@echo "Building layered image - it was made that way..."
	docker build --build-arg GRAAL_VERSION=$(graal_ver) -t krisfoster/$(image_name):$(graal_ver) .

slim-jim: copy-install-files
	docker build --rm --squash --no-cache --build-arg GRAAL_VERSION=$(graal_ver) -t krisfoster/$(image_name):$(graal_ver) . 

copy-install-files:
	mkdir -p installs
	cp ../installs/* ./installs

run:
	docker run --rm -it -P krisfoster/$(image_name):$(graal_ver) zsh

push:
	docker login
	docker push krisfoster/$(image_name):$(graal_ver)

clean:
	rm -rf ./installs
	#docker image rm krisfoster/$(image_name):$(graal_ver)