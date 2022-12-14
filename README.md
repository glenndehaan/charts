# Glenn de Haan - Helm Charts Repository

## Usage

[Helm](https://helm.sh) must be installed to use the charts.
Please refer to Helm's [documentation](https://helm.sh/docs) to get started.

Once Helm has been set up correctly, add the repo as follows:

```shell
helm repo add glenndehaan https://glenndehaan.github.io/charts
```

If you had already added this repo earlier, run `helm repo update` to retrieve the latest versions of the packages.
You can then run `helm search repo glenndehaan` to see the charts.

To install a chart:
```shell
helm install <chart-name> glenndehaan/<chart-name>
```

To uninstall a chart:
```shell
helm delete <chart-name>
```
