
build:
	go mod tidy
	docker build . -t samew/shippingservice:v0.5.2 --platform linux/amd64
	docker push samew/shippingservice:v0.5.2

clean:
	go mod tidy
	docker build . -t samew/shippingservice:v0.5.2 --no-cache --progress plain --platform linux/amd64
	docker push samew/shippingservice:v0.5.2 --platform linux/amd64