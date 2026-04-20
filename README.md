# Dify on OpenShift

Deploy [Dify](https://dify.ai/) on OpenShift

This repository is a fork of [Winson-030/dify-kubernetes](https://github.com/Winson-030/dify-kubernetes) with modifications to enable it to run on OpenShift.

## Changes Made

- Converted all `hostPath` volumes to `persistent volumes`.
- Added service accounts for all deployments and statefulsets.
- Added role bindings to grant `anyuid` permissions to the necessary service accounts.
- Added OpenShift route for ingress

## How to Use

### Deploy Dify

To deploy Dify on OpenShift, run the following command:

```shell
kubectl apply -f https://raw.githubusercontent.com/theodor2311/dify-openshift/refs/heads/upgrade/dify-version-100/dify-deployment.yaml
```

### How to access Dify

An OpenShift route will be created to provide external access to Dify. Run the following command to retrieve the URL:

```shell
oc get route -n dify
```

Once the application is accessible, you can create your account using the default initial password, which is set to `password` in the Dify shared config map.

