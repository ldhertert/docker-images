# Summary

A base image for harness plugins and steps that contains common utilities and configuration.

Packages included:
* bash
* curl
* wget
* git
* nano
* sudo
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
docker build --target slim -t ldhertert/base:slim  ./base
docker build -t ldhertert/base  ./base

docker push ldhertert/base
docker push ldhertert/base:slim
```