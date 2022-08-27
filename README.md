# springone-2022

## Getting started with Spring functions for Knative

### Setup

Kubernetes:

- kubectl
- kubectx
- kind: https://kind.sigs.k8s.io/docs/user/quick-start/#installing-from-release-binaries
    ```
    [ $(uname -m) = arm64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.14.0/kind-darwin-arm64
    sudo install ./kind /usr/local/bin/kind
    ```

Knative:

- https://knative.dev/docs/

- kn CLI: https://knative.dev/docs/install/quickstart-install/#install-the-knative-cli
    ```
    install ~/Downloads/kn-darwin-arm64 ~/bin/kn
    ```

- kn quickstart: https://knative.dev/docs/install/quickstart-install/#install-the-knative-quickstart-plugin 
    ```
    install ~/Downloads/kn-quickstart-darwin-arm64 ~/bin/kn-quickstart
    ```

- kn func: https://github.com/knative-sandbox/kn-plugin-func/blob/main/docs/installing_cli.md#prebuilt-binary
    ```
    install ~/Downloads/func_darwin_arm64 ~/bin/kn-func
    ```

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
kn func create hello -l springboot -t http
cd hello
 # kn func build --builder-image gcr.io/buildpacks/builder:v1  --image docker.io/trisberg/hello --push --verbose
pack build trisberg/hello --path . --builder gcr.io/buildpacks/builder:v1 --publish
pack build trisberg/hello --path . --builder paketobuildpacks/builder:base --publish
kn func deploy --build disabled --image trisberg/hello --push=false
curl -w'\n' -H "Content-Type: text/plain" http://hello.default.127.0.0.1.sslip.io -d test
func invoke --target=http://hello.default.127.0.0.1.sslip.io --data test
```

### With Dockerfile

```
./mvnw clean package
docker build . -t trisberg/hello-arm
docker push trisberg/hello-arm
kn func deploy --build disabled --image trisberg/hello-arm --push=false
curl -w'\n' -H "Content-Type: text/plain" http://hello.default.127.0.0.1.sslip.io -d test
```

### With Jib

```
      <plugin>
        <groupId>com.google.cloud.tools</groupId>
        <artifactId>jib-maven-plugin</artifactId>
        <version>3.2.1</version>
        <configuration>
          <!-- <to>
            <image>trisberg/hello-jib</image>
          </to> -->
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

```
./mvnw compile jib:build -Dimage=trisberg/hello-jib
kn func deploy --build disabled --image trisberg/hello-jib --push=false
curl -w'\n' -H "Content-Type: text/plain" http://hello.default.127.0.0.1.sslip.io -d jib
```
