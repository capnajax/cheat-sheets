# Pod cheat sheet

## Calling APIs from within a pod

First set up a service account with the necessary role bindings to do what you
need to do. Add the service account to the template spec (not the containers)

```yaml
apiVersion: batch/v1
kind: Pod
spec:
  template:
    spec:
      containers:
      - name: my-pod
        ⋮
      restartPolicy: OnFailure
      serviceAccountName: certificates-job-sa
      volumes:
      - name: certs
        ⋮
```

Calling from within a container. All the necessary info is automatically mounted

```sh
# Point to the internal API server hostname
APISERVER=https://kubernetes.default.svc

# Path to ServiceAccount token
SERVICEACCOUNT=/var/run/secrets/kubernetes.io/serviceaccount

# Read this Pod's namespace
NAMESPACE=$(cat ${SERVICEACCOUNT}/namespace)

# Read the ServiceAccount bearer token
TOKEN=$(cat ${SERVICEACCOUNT}/token)

# Reference the internal certificate authority (CA)
CACERT=${SERVICEACCOUNT}/ca.crt

# Explore the API with TOKEN
curl --cacert ${CACERT} --header "Authorization: Bearer ${TOKEN}" -X GET ${APISERVER}/api
```
