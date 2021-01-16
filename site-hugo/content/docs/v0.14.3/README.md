# <img src="img/sonobuoy-logo.png" alt="Sonobuoy logo" width="400px" > [![CircleCI](https://circleci.com/gh/vmware-tanzu/sonobuoy.svg?style=svg)](https://circleci.com/gh/vmware-tanzu/sonobuoy)

## [Overview][oview]

Sonobuoy is a diagnostic tool that makes it easier to understand the
state of a Kubernetes cluster by running a set of plugins (including [Kubernetes][k8s] conformance
tests) in an accessible and non-destructive manner. It is a customizable,
extendable, and cluster-agnostic way to generate clear, informative reports
about your cluster.

Its selective data dumps of Kubernetes resource objects and cluster nodes allow
for the following use cases:

* Integrated end-to-end (e2e) [conformance-testing][e2e]
* Workload debugging
* Custom data collection via extensible plugins

Sonobuoy supports 3 Kubernetes minor versions: the current release and 2 minor versions before. Sonobuoy is currently versioned to track the Kubernetes minor version to clarify the support matrix. For example, Sonobuoy v0.14.x would support Kubernetes 1.14.x, 1.13.x, and 1.12.x.

> Note: You can skip this version enforcement by running Sonobuoy with the `--skip-preflight` flag.

## Prerequisites

* Access to an up-and-running Kubernetes cluster. If you do not have a cluster,
  we recommend following the [AWS Quickstart for Kubernetes][quickstart] instructions.

* An admin `kubeconfig` file, and the KUBECONFIG environment variable set.

* For some advanced workflows it may be required to have `kubectl` installed. See [installing via Homebrew (MacOS)][brew] or [building
  the binary (Linux)][linux].

## Installing

Download one of the releases directly from [here][releases].

Alternatively, you can install the CLI by running:

```
go get -u -v github.com/vmware-tanzu/sonobuoy
```

Golang version 1.12 or greater is recommended. Golang can be installed via
[gimme][gimme].

## Getting Started
To launch conformance tests (ensuring [CNCF][cncf] conformance) and wait until they are finished run:
```
sonobuoy run --wait
```

> Note: Using `--mode quick` will significantly shorten the runtime of Sonobuoy. It runs just a single test, helping to quickly validate your Sonobuoy and Kubernetes configuration.

Get the results from the plugins (e.g. e2e test results):
```
results=$(sonobuoy retrieve)
```

Inspect results for test failures.  This will list the number of tests failed and their names:
```
sonobuoy e2e $results
```

You can also extract the entire contents of the file to get much more [detailed data][snapshot] about your cluster.

Sonobuoy creates a few resources in order to run and expects to run within its
own namespace.

Deleting Sonobuoy entails removing it's namespace as well as a few cluster
scoped resources.

```
sonobuoy delete --wait
```

> Note: The --wait option ensures the Kubernetes namespace is deleted, avoiding conflicts if another Sonobuoy run is started quickly.

### Monitoring Sonobuoy during a run
You can check on the status of each of the plugins running with:
```
sonobuoy status
```

You can also inspect the logs of all Sonobuoy containers:
```
sonobuoy logs
```

## More information

[The documentation][docs] provides further information about:
 * [conformance tests][conformance]
 * [plugins][plugins]
 * Testing of [air gapped clusters][airgap].
 * [Customization][gen] of YAML prior to running.
 * The [Sonobuoy config file][sonobuoyconfig] file and how to edit it.

## Troubleshooting

If you encounter any problems that the documentation does not address, [file an
issue][issue].

## Known Issues

### Leaked End-to-end namespaces

There are some Kubernetes e2e tests that may leak resources. Sonobuoy can
help clean those up as well by deleting all namespaces prefixed with `e2e`:
```
sonobuoy delete --all
```

### Run on Google Cloud Platform (GCP)

Sonobuoy requires admin permissions which won't be automatic if you are running via Google Kubernetes Engine (GKE) cluster. You must first create an admin role for the user under which you run Sonobuoy:

```
kubectl create clusterrolebinding <your-user-cluster-admin-binding> --clusterrole=cluster-admin --user=<your.google.cloud.email@example.org>
```

## Contributing

Thanks for taking the time to join our community and start contributing! We
welcome pull requests. Feel free to dig through the [issues][issue] and jump in.

### Before you start

* Please familiarize yourself with the [Code of Conduct][coc] before
  contributing.
* See [CONTRIBUTING.md][contrib] for instructions on the developer certificate
  of origin that we require.
* There is a [Slack channel][slack] if you want to
  interact with other members of the community

## Changelog

See [the list of releases][releases] to find out about feature changes.

[airgap]: airgap.md
[brew]: https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-with-homebrew-on-macos
[cncf]: https://github.com/cncf/k8s-conformance#certified-kubernetes
[coc]: https://github.com/vmware-tanzu/sonobuoy/blob/master/CODE_OF_CONDUCT.md
[contrib]: https://github.com/vmware-tanzu/sonobuoy/blob/master/CONTRIBUTING.md
[conformance]: conformance-testing.md
[docs]: https://github.com/vmware-tanzu/sonobuoy/tree/master/docs
[e2e]: conformance-testing.md
[gen]: gen.md
[gimme]: https://github.com/travis-ci/gimme
[issue]: https://github.com/vmware-tanzu/sonobuoy/issues
[k8s]: https://github.com/kubernetes/kubernetes
[linux]: https://kubernetes.io/docs/tasks/tools/install-kubectl/#tabset-1
[oview]: https://youtu.be/k-P4hXdruRs?t=9m27s
[plugins]: plugins.md
[quickstart]: https://aws.amazon.com/quickstart/architecture/vmware-kubernetes/
[releases]: https://github.com/vmware-tanzu/sonobuoy/releases
[slack]: https://kubernetes.slack.com/messages/sonobuoy
[snapshot]: snapshot.md
[sonobuoyconfig]: sonobuoy-config.md
[wait]: wait.md