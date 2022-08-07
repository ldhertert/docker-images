Need to build out some dockerfiles for this, however I wanted to document how to push/pull files to an OCI repo using ORAS.

Installation and other docs: https://oras.land/cli/

# Pushing

```
mkdir -p input
echo "hello world" > input/artifact.txt
oras push ghcr.io/ldhertert/oras-image:v1 ./input/
```

# Pulling

```
oras pull ghcr.io/ldhertert/oras-image:v1 -o ./output
```
