# Install kpack and builder that can build native image for functions

Based on the [kpack Tutorial](https://github.com/pivotal/kpack/blob/main/docs/tutorial.md)

> Note: all builds will be done in `default` namespace

## Clone this repo and cd to `kpack` directory

```sh
git clone https://github.com/trisberg/springone-2022.git
cd springone-2022/kpack
```

## Install `kpack`

```sh
kubectl apply -f https://github.com/pivotal/kpack/releases/download/v0.7.1/release-0.7.1.yaml
```

## Set up `default` namespace for builds

### Create secret for pushing and pulling images

> Note: this example uses DockerHub

```
kubectl create secret docker-registry registry-credentials \
    --docker-username=$DOCKER_ID \
    --docker-password=$DOCKER_PASSWORD \
    --docker-server=https://index.docker.io/v1/ \
    --namespace default
```

### Create service account for builds

```sh
kubectl apply -f build-service-account.yaml
```

### Create ClusterStore and ClusterStack

```sh
kubectl apply -f cluster-store.yaml
kubectl apply -f cluster-stack.yaml
```

### Create builder

```sh
kubectl apply -f builder.yaml
```

### Create image resource

> Note: update the `tag` to match your Docker ID and image name

```sh
kubectl apply -f quote-image.yaml
```
