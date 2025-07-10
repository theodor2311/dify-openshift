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

### Post-Deployment Fixes
Based on testing, the current version of Dify may require additional fixes to function correctly. These steps include:
- Removing specific tables and sequences from the PostgreSQL database to avoid errors during the Flask db upgrade.
- Perform the Flask db upgrade.
Use the following commands to apply these fixes:

```shell
oc exec -n dify $(oc get po -lapp=dify-postgres -oname -n dify) -- sh -c '
psql -d "$POSTGRES_DB" -c "
DROP SEQUENCE IF EXISTS task_id_sequence;
DROP SEQUENCE IF EXISTS taskset_id_sequence;
DROP TABLE IF EXISTS celery_taskmeta;
DROP TABLE IF EXISTS celery_tasksetmeta;
"'

oc exec -n dify $(oc get po -lapp=dify-api -oname -n dify) -- flask db upgrade
```

### How to access Dify

An OpenShift route will be created to provide external access to Dify. Run the following command to retrieve the URL:

```shell
oc get route -n dify
```

Once the application is accessible, you can create your account using the default initial password, which is set to `password` in the Dify shared config map.

