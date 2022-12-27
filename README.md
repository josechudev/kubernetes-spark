# Spark on kubernetes


Run helm
```
docker run -p 8998:8998 -e KUBECONFIG="/work/config/kubeconfig.yaml" -it --rm -v ${PWD}:/work -w /work --entrypoint /bin/sh dtzar/helm-kubectl:latest 
```