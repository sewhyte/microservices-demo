VERSION := "v0.5.2"

build:	
	pip-compile --output-file=requirements.txt requirements.in
	docker build -t samew/loadgenerator:$(VERSION) .
	docker push samew/loadgenerator:$(VERSION)

clean:	
	pip-compile --output-file=requirements.txt requirements.in
	docker build -t samew/loadgenerator:$(VERSION) . --no-cache
	docker push samew/loadgenerator:$(VERSION)
