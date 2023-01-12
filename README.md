# Spark on kubernetes


Run helm
```
docker run  -e KUBECONFIG="/work/config/kubeconfig.yaml" -it --rm -v ${PWD}:/work -w /work --entrypoint /bin/sh dtzar/helm-kubectl:latest 
```
```
helm upgrade spark-cluster ./spark-cluster --create-namespace --namespace livy --values values.yaml --install --atomic  --wait --timeout 300s --cleanup-on-fail --debug --dry-run
alias k=kubectl
k config set-context --current --namespace=livy
k port-forward service/livy-server 3030:80 --address 10.245.39.68,localhost
```

```
helm upgrade keycloak ./keycloak --create-namespace --namespace keycloak --values values.yaml --install --atomic  --wait --timeout 300s --cleanup-on-fail --dry-run
 apk add --update coreutils
 echo $(kubectl get secret --namespace keycloak  keycloak -o jsonpath="{.data.admin-password}" | base64 -d)
```
