# Spark on kubernetes


Run helm
```
docker run  -e KUBECONFIG="/work/config/kubeconfig.yaml" -it --rm -v ${PWD}:/work -w /work --entrypoint /bin/sh dtzar/helm-kubectl:latest 
```
```
helm upgrade spark-cluster ./spark-cluster --namespace livy --values values.yaml --install --atomic  --wait --timeout 300s --cleanup-on-fail  --dry-run
alias k=kubectl
k port-forward service/livy-server 3030:80 --address 10.245.39.68,localhost
```

