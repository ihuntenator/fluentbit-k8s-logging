# k8s fluentbit logging



## Create kustomize layout

```
helm repo add fluentbit https://fluent.github.io/helm-charts
helm repo update
```

```
helm fetch \
--untar \
--untardir charts \
fluent/fluent-bit
```

```
mkdir -p base/resources base/patches overlays && mv charts/fluent-bit/values.yaml base/
```

```
helm template \
--output-dir base \
--namespace logging \
--values base/values.yaml \
logging \
charts/fluent-bit
```

```
mv base/fluent-bit/templates/* base/resources && rm -rf base/fluent-bit && rm -rf charts
```

```
cat <<EOF > base/resources/namespace.yaml
apiVersion: v1
kind: Namespace
metadata:
  name: logging
EOF
```

```
cd base && kustomize create --autodetect --recursive ./
```

```
cd base && kubectl kustomize --enable-helm
```


## Tree

```
├── README.md
├── base
│   ├── kustomization.yaml
│   ├── patches
│   │   └── fluentd.conf
│   ├── resources
│   │   ├── clusterrole.yaml
│   │   ├── clusterrolebinding.yaml
│   │   ├── configmap.yaml
│   │   ├── daemonset.yaml
│   │   ├── namespace.yaml
│   │   ├── service.yaml
│   │   ├── serviceaccount.yaml
│   │   └── tests
│   │       └── test-connection.yaml
│   └── values.yaml
└── overlays
```

