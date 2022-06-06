## what is helm
- helm chart
- least invasive change
- running vs desired state
- release tracking

* helm install
* helm status
* helm upgrade
* helm rollback
* helm uninstall

## working with chart repositories
fetch chart
```
helm fetch stable/spark
```

to make a repo index.file, it will create a index.yaml in directory
```
helm repo index ./test/ 
```

## updating release in helm
upgrade example
```
helm upgrade Demo-release -set alpine.image=1.1.6
```

## get into helm chart
somechart/
  Chart.yaml
  LICENSE
  README.md
  values.yaml
  values.schema.json
  charts/
  crds/ (contains custom resource definitions (CRDs) )
  templates/
  templates/NOTES.txt

use helm to create a chart
```
helm create demo
```

dry run
```
helm install <name> <path> --dry-run
```

## modifying charts
- inline using set
- passing a file ( override char's values.yaml)
```
helm install demo <path> -f newvalues.yaml
```
- customize the chart

helm show values

## understand the language of chart
based on the go templating language
```
{{ .Values.imge.registry}}
```
. at the start indicates the top level ( the chart folder)

```
{{- if .Values.securityContext.enabled }}
```

pipe can be used
```
{{.Values.firstOne | quote }}
```
https://pkg.go.dev/text/template#hdr-Functions

把values写在values.yaml里面， 把本chart要部署的一些东西写在tempaltes里面， 包括deployment, service之类。 里面的值用 template language 覆盖
subchart 放在 charts文件夹里面


## working with subcharts
subchart cannot access parent charts.
but parent chart can override subchart values.

global values can be accessed by both the parent and any subcharts
```yaml
global:
  thisvalue: 'something'
```

## implementing pre- and post- actions
- load data , eg. load secret
- manipulate data eg. backup a database instance and restore it post-upgrade
- stage env, eg allow service and ohter k8s objects to be manipulated at that time

* annotations define a hook
* hooks are part of a chart
* hooks have weights
* hooks fail everything
* multiple hooks can be defined
* hook objects stand alone

## testing charts
- password validation
- service validation
- configuration validation

## testing charts
/<chart>/templates/tests/*.yaml
created automatically when chart is created

run the test template
```
$helm test demo
```

## creating and using libraries
a library is a shared template that can be used to prevent repetivitive code

add a dependency in Chart.yaml, 
```yaml
dependencies:
- name: common
  version: "^0.0.5"
  repository: "https://charts.helm.sh/incubator/"
```
then it will download into charts folder ( run it in the chart folder)
```
helm dependency update
```
没什么用，其实就是substitue 一些常用部分
## validating charts
一下命令会生成两个文件,一个是 .tgz, 一个是 a provenance file
```
helm package -sign
```

verify command
```
helm verify <tgz file>
```

## adding RBAC
helm 运行的权限取决与 kubectl 正处于的context

## storage backend
- configmap
- secret
- SQL