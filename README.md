# Deploy Prometheus and Grafana in an EKS cluster

1. Create EKS cluster
```bash
eksctl create cluster --name eks-prometheus-grafana --node-type t2.micro --nodes 2
```

2. Install [Metrics Server](https://docs.aws.amazon.com/eks/latest/userguide/metrics-server.html)
```bash
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
kubectl get deployment metrics-server -n kube-system
```

3. Deploy [Prometheus using Helm](https://docs.aws.amazon.com/eks/latest/userguide/prometheus.html)
```bash
kubectl create namespace prometheus
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm upgrade -i prometheus prometheus-community/prometheus \
    --namespace prometheus \
    --set alertmanager.persistentVolume.storageClass="gp2",server.persistentVolume.storageClass="gp2"
```

4. Check prometheus pods status
```bash
kubectl get pods -n prometheus
```

5. Forward prometheus port to your local computer on port 9090
```bash
kubectl --namespace=prometheus port-forward deploy/prometheus-server 9090
```

