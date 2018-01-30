# bzz.network
Kubernetes Setup for bzz.network

## Getting started
1. Install `kops` and `kubectl`

**macOS Homebrew**
```
brew update
brew install kubectl
brew install kops
```

**macOS Manual**
```
curl -LO https://github.com/kubernetes/kops/releases/download/1.7.1/kops-darwin-amd64
chmod +x kops-darwin-amd64
sudo mv kops-darwin-amd64 /usr/local/bin/kops

curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/darwin/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
```

**Linux**
```
curl -LO https://github.com/kubernetes/kops/releases/download/1.7.1/kops-linux-amd64
chmod +x kops-linux-amd64
sudo mv kops-linux-amd64 /usr/local/bin/kops

curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
```

2. Configure AWS keys
We need access to the cluster's state-store in S3 and EC2 if we want to configure the cluster. Let's setup our AWS keys using aws-cli and named profiles to connect to different clusters (dev, stage, prod). You can do this by doing:
```
aws configure --profile dev # asks for AWS key, secret and region (us-west-2)
export AWS_PROFILE=dev
```
Note: the key you use needs to have S3 read only access (the aws keys from backend config.yml should work). EC2 full access will be needed if you want to edit the cluster itself

3. Use kops to configure kubectl
```
export KOPS_STATE_STORE=s3://state-store.dev.hyundaidrive.com
kops export kubecfg dev.k8s.local
```
This will set up kops to use the dev cluster and export the config for kubectl. If you want to use stage or prod then just replace `dev` with the proper environment

4. Check if kubectl can talk to the cluster
```
kubectl get nodes # list nodes
```

## Commonly used commands:

Logs:
```
kubectl logs [pod_name] -f
```
SSH:
```
kubectl exec -it [pod_name] sh  
```

# Further Reading
Our wiki - https://github.com/async-la/k8s-hma-backend/wiki
kops - https://github.com/kubernetes/kops
kubernetes cheatsheet - https://kubernetes.io/docs/reference/kubectl/cheatsheet/
