
build:
	docker build src -t samew/cartservice:v0.5.2 --platform linux/amd64
	docker push samew/cartservice:v0.5.2

clean:
	docker build src -t samew/cartservice:v0.5.2 --no-cache --progress plain --platform linux/amd64
	docker push samew/cartservice:v0.5.2 --platform linux/amd64