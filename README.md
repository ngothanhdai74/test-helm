# Add Rancher repo
helm repo add rancher-stable https://releases.rancher.com/server-charts/stable
 
# Create a namespace for Rancher
kubectl create namespace cattle-system
Step 2: Install CertManager
# Install the CustomResourceDefinition resources separately
kubectl apply --validate=false -f https://github.com/jetstack/cert-manager/releases/download/v1.0.4/cert-manager.crds.yaml
 
# Create the namespace for cert-manager
kubectl create namespace cert-manager
 
# Add the Jetstack Helm repository
helm repo add jetstack https://charts.jetstack.io
 
# Update your local Helm chart repository cache
helm repo update
 
# Install the cert-manager Helm chart
helm install \
  cert-manager jetstack/cert-manager \
  --namespace cert-manager \
  --version v1.0.4
Step 3: Install Rancher
hostname=rancher.kimconnect.com
helm install rancher rancher-stable/rancher \
  --namespace cattle-system \
  --set hostname=$hostname
 
# Ran into this problem
Error: chart requires kubeVersion: < 1.20.0-0 which is incompatible with Kubernetes v1.20.2
 
# Workaround: Install K3s instead of K8s
curl -sfL https://get.k3s.io | sh -
# OR
curl https://get.k3s.io | INSTALL_K3S_VERSION=v1.19.7+k3s1 sh - # Install a specific version