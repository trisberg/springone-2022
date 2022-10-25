# springone-2022

## Getting started with Spring functions for Knative

### Setup

Kubernetes:

- kubectl: https://kubernetes.io/docs/tasks/tools/#kubectl
- kind: https://kubernetes.io/docs/tasks/tools/#kind

Knative:

- https://knative.dev/docs/

- kn CLI: https://knative.dev/docs/install/quickstart-install/#install-the-knative-cli

- kn quickstart: https://knative.dev/docs/install/quickstart-install/#install-the-knative-quickstart-plugin 

- func: https://github.com/knative/func/blob/main/docs/installing_cli.md


### Quickstart

Create cluster:

```
kn quickstart kind 
```

In separate terminal:

```
sudo kubectl port-forward -n kourier-system  svc/kourier 80:80
```

### Get started

```
export FUNC_REGISTRY=docker.io/<your-docker-id>
func create echo -l springboot -t http
cd echo
func build --verbose
func deploy --build=false
curl -w'\n' -H "Content-Type: text/plain" http://echo.default.127.0.0.1.sslip.io -d test
func invoke --target=http://echo.default.127.0.0.1.sslip.io --data test
```

<!-- ### With Dockerfile

```
export DOCKER_ID=<your-docker-id>
./mvnw clean package
docker build . -t $DOCKER_ID/echo-arm
docker push $DOCKER_ID/echo-arm
kn func deploy --build disabled --image $DOCKER_ID/echo-arm --push=false
curl -w'\n' -H "Content-Type: text/plain" http://echo.default.127.0.0.1.sslip.io -d test
``` -->

### With Jib

Add this plugin to the `pom.xml`:
```
      <plugin>
        <groupId>com.google.cloud.tools</groupId>
        <artifactId>jib-maven-plugin</artifactId>
        <version>3.2.1</version>
        <configuration>
          <from>
            <image>eclipse-temurin:11-jre</image>
            <platforms>
              <platform>
                <architecture>amd64</architecture>
                <os>linux</os>
              </platform>
              <platform>
                <architecture>arm64</architecture>
                <os>linux</os>
              </platform>
            </platforms>
          </from>
        </configuration>
      </plugin>
```

Then build the project:

```
export DOCKER_ID=<your-docker-id>
./mvnw compile jib:build -Dimage=$DOCKER_ID/echo-jib
kn func deploy --build disabled --image $DOCKER_ID/echo-jib --push=false
curl -w'\n' -H "Content-Type: text/plain" http://echo.default.127.0.0.1.sslip.io -d jib
```
