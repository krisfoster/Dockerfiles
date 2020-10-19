.PHONY: chunky-monkey

current_dir = $$PWD
image_ver = "19.2.1"
image_name = "sql-developer"

chunky-monkey: copy-install-files
	docker build -t krisfoster/$(image_name):$(image_ver) .

slim-jim: copy-install-files
	docker build --rm --squash --no-cache -t krisfoster/$(image_name):$(image_ver) . 

copy-install-files:
	mkdir -p installs
	cp ../installs/sqldeveloper* ./installs

run:
	docker run --rm -it -P krisfoster/$(image_name):$(image_ver)

push:
	docker login
	docker push krisfoster/$(image_name):$(image_ver)

clean:
	rm -rf ./installs
	#docker image rm krisfoster/$(image_name):$(image_ver)