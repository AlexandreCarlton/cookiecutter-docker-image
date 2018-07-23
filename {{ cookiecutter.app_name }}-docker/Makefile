# TODO: Take in the app_name, and take in the image using the app as a suggestion.
IMAGE_NAME := {{ cookiecutter.author.lower() }}/{{ cookiecutter.app_name }}
VERSION := {{ cookiecutter.version }}

VCS_REF := $(shell git rev-parse --short HEAD)

all: image

image: {{ cookiecutter.app_name }}-image.tar
.PHONY: image

{{ cookiecutter.app_name }}-image.tar: Dockerfile
	docker build \
		--label org.label-schema.build-date="$(shell date --rfc-3339=seconds)" \
		--label org.label-schema.name="{{ cookiecutter.app_name }}" \
		--label org.label-schema.description="{{ cookiecutter.description }}" \
		--label org.label-schema.url="{{ cookiecutter.url }}" \
		--label org.label-schema.vcs-url="{{ cookiecutter.vcs_url }}" \
		--label org.label-schema.vcs-ref="$(VCS_REF)" \
		--label org.label-schema.version="$(VERSION)" \
		--label org.label-schema.schema-version="1.0" \
		--tag $(IMAGE_NAME):build \
		.
	docker save --output="{{ cookiecutter.app_name }}-image.tar" $(IMAGE_NAME):build

push: {{ cookiecutter.app_name }}-image.tar
	docker load --input="{{ cookiecutter.app_name }}-image.tar"
	docker tag $(IMAGE_NAME):build $(IMAGE_NAME):$(VCS_REF)
	docker tag $(IMAGE_NAME):build $(IMAGE_NAME):$(VERSION)
	docker push $(IMAGE_NAME):$(VCS_REF)
	docker push $(IMAGE_NAME):$(VERSION)
.PHONY: push
