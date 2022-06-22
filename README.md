# Kubectl

[![Preview](https://serhiy.s3.eu-central-1.amazonaws.com/Github_repo/kubectl/logo.png)](https://cloud.google.com)

[![Release Images](https://github.com/StarUbiquitous/kubectl/actions/workflows/image.yml/badge.svg)](https://github.com/StarUbiquitous/kubectl/actions/workflows/image.yml)
[![SDK version upgrade](https://github.com/StarUbiquitous/kubectl/actions/workflows/upgrader.yaml/badge.svg)](https://github.com/StarUbiquitous/kubectl/actions/workflows/upgrader.yaml)
[![Tests](https://github.com/StarUbiquitous/kubectl/actions/workflows/tests.yaml/badge.svg)](https://github.com/StarUbiquitous/kubectl/actions/workflows/tests.yaml)

![Platform](https://img.shields.io/badge/Platform-linux%2Famd64-brightgreen?style=flat-square&logo=linux)
![Platform](https://img.shields.io/badge/Platform-linux%2Farm64-brightgreen?style=flat-square&logo=linux)
![GitHub last commit (branch)](https://img.shields.io/github/last-commit/starubiquitous/kubectl/master?style=flat-square)
[![LICENSE](https://img.shields.io/badge/License-Anti%20996-blue.svg?style=flat-square)](https://github.com/996icu/996.ICU/blob/master/LICENSE)
[![LICENSE](https://img.shields.io/badge/License-Apache--2.0-green.svg?style=flat-square)](LICENSE-APACHE)
[![996.icu](https://img.shields.io/badge/Link-996.icu-red.svg?style=flat-square)](https://996.icu)

GitHub Action for interacting with kubectl ([k8s](https://kubernetes.io))

## Usage
To use kubectl put this step into your workflow:

### Authorization with config file
```yaml
- uses: starubiquitous/kubectl@master
  env:
    KUBE_CONFIG: ${{ secrets.KUBE_CONFIG }}
  with:
    args: get pods
```

### Authorization with credentials
```yaml
- uses: starubiquitous/kubectl@master
  env:
    KUBE_HOST: ${{ secrets.KUBE_HOST }}
    KUBE_CERTIFICATE: ${{ secrets.KUBE_CERTIFICATE }}
    KUBE_USERNAME: ${{ secrets.KUBE_USERNAME }}
    KUBE_PASSWORD: ${{ secrets.KUBE_PASSWORD }}
  with:
    args: get pods
```

### Authorization with a bearer token
```yaml
- uses: starubiquitous/kubectl@master
  env:
    KUBE_HOST: ${{ secrets.KUBE_HOST }}
    KUBE_CERTIFICATE: ${{ secrets.KUBE_CERTIFICATE }}
    KUBE_TOKEN: ${{ secrets.KUBE_TOKEN }}
  with:
    args: get pods
```

## Environment variables
All these variables need to authorize to kubernetes cluster.  
I recommend using secrets for this.

### KUBECONFIG file
First options its to use [kubeconfig file](https://kubernetes.io/docs/concepts/configuration/organize-cluster-access-kubeconfig/).  

For this method `KUBE_CONFIG` required.  
You can find it: `cat $HOME/.kube/config | base64 `.

Optionally you can switch the [context](https://kubernetes.io/docs/tasks/access-application-cluster/configure-access-multiple-clusters/) (the cluster) if you have few in kubeconfig file. Passing specific context to `KUBE_CONTEXT`. To see the list of available contexts do: `kubectl config get-contexts`.

| Variable | Type |
| --- | --- |
| KUBE_CONFIG | string (base64) |
| KUBE_CONTEXT | string |

### KUBECONFIG file
Another way to authenticate in the cluster is [HTTP basic auth](https://kubernetes.io/docs/reference/access-authn-authz/authentication/).
  
For this you need to pass:
- host (IP only, without protocol)
- username
- password
- cluster CA certificate

| Variable | Type |
| --- | --- |
| KUBE_HOST | string |
| KUBE_USERNAME | string |
| KUBE_PASSWORD | string |
| KUBE_CERTIFICATE | string |

## Example
```yaml
name: Get pods
on: [push]

jobs:
  deploy:
    name: Deploy
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v1
      - uses: starubiquitous/kubectl@master
        env:
          KUBE_CONFIG: ${{ secrets.KUBE_CONFIG }}
        with:
          args: get pods
```

```yaml
name: Get pods
on: [push]

jobs:
  deploy:
    name: Deploy
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v1
      - uses: starubiquitous/kubectl@master
        env:
          KUBE_CONFIG: ${{ secrets.KUBE_CONFIG }}

      - uses: starubiquitous/kubectl@master
        with:
          args: get pods
```

## Versions
If you need a specific version of kubectl, make a PR with a specific version number.
After accepting PR the new release will be created.   
To use a specific version of kubectl use:

```yaml
- uses: starubiquitous/kubectl@1.14.3
  env:
    KUBE_CONFIG: ${{ secrets.KUBE_CONFIG }}
  with:
    args: get pods
```

## Licence
[MIT License](https://github.com/actions-hub/kubectl/blob/master/LICENSE)
