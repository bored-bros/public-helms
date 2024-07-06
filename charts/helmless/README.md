# Helmless

The `helmless` chart takes a list of Kubernetes resources and merges each resource with a default `metadata.labels` map and installs the result.

The Kubernetes resources can be "manifest" ones defined under the `resources` key, or "templated" ones defined under the `templates` key.

Some use cases for this chart include Helm-based installation and
maintenance of resources of kinds:
- LimitRange
- PriorityClass
- Secret
- CRs

## Usage

[Helm](https://helm.sh) must be installed to use the charts.  Please refer to
Helm's [documentation](https://helm.sh/docs) to get started.

Once Helm has been set up correctly, add the repo as follows:

  helm repo add my-repo https://ohayak.github.io/helm-charts

If you had already added this repo earlier, run `helm repo update` to retrieve
the latest versions of the packages.  You can then run `helm search repo my-repo` to see the charts.

To install the helmless chart:

    helm install my-chart my-repo/helmless

To uninstall the chart:

    helm delete my-chart

### Manifest resources

#### STEP 1: Create a yaml file containing your manifest resources.

```
# manifest-priority-classes.yaml
resources:
  - apiVersion: scheduling.k8s.io/v1beta1
    kind: PriorityClass
    metadata:
      name: common-critical
    value: 100000000
    globalDefault: false
    description: "This priority class should only be used for critical priority common pods."
```

#### STEP 2: Install your manifest resources.

```
helm install manifest-priority-classes itscontained/manifest -f manifest-priority-classes.yaml
```

### Templated resources

#### STEP 1: Create a yaml file containing your templated resources.

```
# values.yaml

templates:
- |
  apiVersion: v1
  kind: Secret
  metadata:
    name: common-secret
  stringData:
    mykey: {{ .Values.mysecret }}
```

#### STEP 2: Install your templated resources.

```
helm install mysecret manifest -f values.yaml
```