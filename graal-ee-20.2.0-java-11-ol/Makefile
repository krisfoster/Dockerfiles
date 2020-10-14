.PHONY: slim-jim

chunky-monkey:
	docker build -t krisfoster/graalsc:graal-ee-java11-slim-20.2.0 .

slim-jim: copy-install-files
	docker build --rm --squash --no-cache -t krisfoster/graalsc:graal-ee-java11-slim-20.2.0 . 

copy-install-files:
	mkdir -p installs
	cp ../installs/* ./installs

run:
	docker run --rm -it -P krisfoster/graalsc:graal-ee-java11-slim-20.2.0 zsh

push:
	docker login
	docker push krisfoster/graalsc:graal-ee-java11-slim-20.2.0

clean:
	rm -rf ./installs
	docker image rm krisfoster/graalsc:graal-ee-java11-slim-20.2.0