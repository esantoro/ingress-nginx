# Upgrading

!!! important
    No matter the method you use for upgrading, _if you use template overrides,
    make sure your templates are compatible with the new version of ingress-nginx_.

## Without Helm

To upgrade your ingress-nginx installation, it should be enough to change the version of the image
in the controller Deployment.

I.e. if your deployment resource looks like (partial example):

```yaml
kind: Deployment
metadata:
  name: nginx-ingress-controller
  namespace: ingress-nginx
spec:
  replicas: 1
  selector: ...
  template:
    metadata: ...
    spec:
      containers:
        - name: nginx-ingress-controller
          image: k8s.gcr.io/ingress-nginx/controller:v0.41.2@sha256:1f4f402b9c14f3ae92b11ada1dfe9893a88f0faeb0b2f4b903e2c67a0c3bf0de
          args: ...
```

simply change the `0.41.2` tag to the version you wish to upgrade to.
The easiest way to do this is e.g. (do note you may need to change the name parameter according to your installation):

```
kubectl set image deployment/nginx-ingress-controller \
  nginx-ingress-controller=k8s.gcr.io/ingress-nginx/controller:v0.41.2@sha256:1f4f402b9c14f3ae92b11ada1dfe9893a88f0faeb0b2f4b903e2c67a0c3bf0de \
  -n ingress-nginx
```

For interactive editing, use `kubectl edit deployment nginx-ingress-controller -n ingress-nginx`.

## With Helm

If you installed ingress-nginx using the Helm command in the deployment docs so its name is `ngx-ingress`,
you should be able to upgrade using

```shell
helm upgrade --reuse-values ngx-ingress ingress-nginx/ingress-nginx
```

### Migrating from stable/nginx-ingress

See detailed steps in the upgrading section of the `ingress-nginx` chart [README](https://github.com/kubernetes/ingress-nginx/blob/master/charts/ingress-nginx/README.md#migrating-from-stablenginx-ingress).
