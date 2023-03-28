VERSION := "v0.5.2"

build:	
	pip-compile --output-file=requirements.txt requirements.in
	docker build -t samew/recommendationservice:$(VERSION) .
	docker push samew/recommendationservice:$(VERSION)