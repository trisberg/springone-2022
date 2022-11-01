# Creating functions using `function-buildpacks-for-knative`

## Use the templates and buildpacks provided by the `function-buildpacks-for-knative` project

### An http `hire` function

Create the function:

```
func create hire -l java -t http-maven --repository https://github.com/vmware-tanzu/function-buildpacks-for-knative
```

Build the function:

```
func build -v
```

Deploy the function:

```
func deploy --build=false
```

Invoke the function:

```
func invoke --data '{"firstName":"John", "lastName":"Doe"}'
```
