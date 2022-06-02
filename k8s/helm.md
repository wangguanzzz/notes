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