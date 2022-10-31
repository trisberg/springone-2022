# SpringOne 2022: Getting started with Spring functions for Knative

## SpringOne 2022 Session

[Track: Beginner-Friendly Spring](https://springone.io/2022/sessions/getting-started-with-spring-functions-for-knative)

[Slides](SpringOne%202022%20-%20Java%20Functions%20for%20Knative.pdf)

[Demo app "quotes"](https://github.com/trisberg/quote)

[Function templates](https://github.com/trisberg/func-templates)

## Setup

Install the following:

### Kubernetes

- kubectl: https://kubernetes.io/docs/tasks/tools/#kubectl
- kind: https://kubernetes.io/docs/tasks/tools/#kind

### Knative

- https://knative.dev/docs/

- kn CLI: https://knative.dev/docs/install/quickstart-install/#install-the-knative-cli

- kn quickstart: https://knative.dev/docs/install/quickstart-install/#install-the-knative-quickstart-plugin 

- func: https://github.com/knative/func/blob/main/docs/installing_cli.md

## Quickstart

Create cluster:

```
kn quickstart kind 
```

## The first function 

### Create the `echo` function

Set a `FUNC_REGISTRY` environment variable with your preferred registry account, e.g. "docker.io/docker-id".

```
export FUNC_REGISTRY="<your preferred registry>"
func create echo -l springboot -t http
cd echo
```

### Build and deploy the `echo` function

This builds the function, pushes the image to the registry specified and deploys the function to your kind cluster.

#### For Intel processor based systems

```
func deploy --verbose
```

#### For ARM processor based systems using Jib

```
./mvnw compile com.google.cloud.tools:jib-maven-plugin:3.3.1:dockerBuild -Dimage=$FUNC_REGISTRY/echo
func deploy --build=false --image=$FUNC_REGISTRY/echo
```

### Invoke the `echo` function

```
func invoke --data "SpringOne 2022"
```

### Remove the `echo` function

```
func delete
```
