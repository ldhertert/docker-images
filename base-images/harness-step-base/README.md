# Summary

A base image for harness plugins and steps that contains common utilities and configuration.

Packages included:
* bash
* curl
* wget
* git
* nano
* sudo
* python
* less
* docker cli
* httpie
* kubectl
* pwsh
* jq
* yq
* golang
* github cli
* helm
* go-template

# Building

```bash
docker build --pull --rm --build-arg ALPINE_VERSION=3.16.1 -f "Dockerfile" -t ldhertert/harness-step-base:alpine "." 
docker tag ldhertert/harness-step-base:alpine ldhertert/harness-step-base:latest
```

Show packages with available updates

`docker run --rm ldhertert/harness-step-base bash -c "apk update -q; apk -u list"`
