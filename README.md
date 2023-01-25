# Spark on kubernetes


Run helm
```
docker run  -e KUBECONFIG="/work/config/kubeconfig.yaml" -it --rm -v ${PWD}:/work -w /work --entrypoint /bin/sh dtzar/helm-kubectl:latest 
```
```
helm upgrade spark-cluster ./spark-cluster --create-namespace --namespace livy --values values.yaml --install --atomic  --wait --timeout 900s --cleanup-on-fail --debug --dry-run
helm plugin install https://github.com/helm/helm-mapkubeapis
helm mapkubeapis --dry-run spark-cluster -n livy
alias k=kubectl
k config set-context --current --namespace=livy
k port-forward service/livy-server 3030:80 --address 10.245.39.68,localhost
```

Registry-de.rke.az.platform.kotomatic.io/livy-admin 

```
livy-admin:
  kubernetesClusterDomain: cluster.local
  livyAdmin:
    livyAdmin:
      image:
        repository: registry-de.rke.az.platform.kotomatic.io/livy-admin  # the API manager
        tag: latest
      apiKey: accesstoken
      livyServer: http://livy-server.livy.svc.cluster.local # change the livy internal server 
      resources:
        limits:
          cpu: 500m
          memory: 128Mi
    replicas: 1
  service:
    ports:
    - port: 80
      targetPort: 80
    type: NodePort
  ingress:
    spec:
      host: azure.host
    annotations:
      kubernetes.io/ingress.class: nginx
      
```

```
curl -X 'POST' \
  'http://azure.host/sessions' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "kind": "spark",
  "proxyUser": "david"
}'

curl -X 'GET' \
  'http://azure.host/sessions' \
  -H 'accept: application/json' \
  -H 'access_token: accesstoken'

curl -X 'GET' \
  'http://azure.host/sessions/0' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json'
```