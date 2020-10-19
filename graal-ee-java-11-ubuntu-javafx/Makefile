.PHONY: chunky-monkey

graal_ver = "20.2.0"
current_dir = $$PWD
image_name = "graal-ee-java11-ubuntu-javafx"

chunky-monkey: copy-install-files
	docker build -t krisfoster/$(image_name):$(graal_ver) .

slim-jim: copy-install-files
	docker build --rm --squash --no-cache -t krisfoster/$(image_name):$(graal_ver) . 

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