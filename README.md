# Install
```
helm repo add bitnami https://charts.bitnami.com/bitnami

helm install minio bitnami/minio -f values.yaml --version 12.8.5 --create-namespace -n minio 
```



# Grant permission
ref. https://www.freekb.net/Article?id=4200
```
oc adm policy add-scc-to-user privileged -z minio
```

# Create route
```
oc create route edge minio-route --service minio --hostname minio-argo.apps.pump-test --port minio-console --path=/ --insecure-policy=Allow 
```


# Permission denied
```
❯ k logs minio-55bff87799-fwjkl
 13:18:11.29
 13:18:11.29 Welcome to the Bitnami minio container
 13:18:11.29 Subscribe to project updates by watching https://github.com/bitnami/containers
 13:18:11.29 Submit issues and feature requests at https://github.com/bitnami/containers/issues
 13:18:11.29
 13:18:11.29 INFO  ==> ** Starting MinIO setup **
/opt/bitnami/scripts/libminio.sh: line 364: /bitnami/minio/data/.root_user: Permission denied
```

# Enable volumePermissions
```
volumePermissions:
  ## @param volumePermissions.enabled Enable init container that changes the owner and group of the persistent volume(s) mountpoint to `runAsUser:fsGroup`
  ##
  enabled: true
  image:
    registry: docker.io
    repository: bitnami/os-shell
    tag: 11-debian-11-r69
    pullPolicy: IfNotPresent
```