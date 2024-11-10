# Task 5: Simple Application Deployment with Helm

## Install Kubernetes Cluster using k3s

1. Create k3s cluster on the control plane node:

```bash
# X.X.X.X is a public ip address of the control plane node
sudo curl -sfL https://get.k3s.io | INSTALL_K3S_EXEC="--disable traefik --tls-san X.X.X.X" sh -s - --write-kubeconfig-mode 644
```

## Install Helm

1. Run commands to install Helm on Debian/Ubuntu Linux:

```bash
curl https://baltocdn.com/helm/signing.asc | gpg --dearmor | sudo tee /usr/share/keyrings/helm.gpg > /dev/null
sudo apt-get install apt-transport-https --yes
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/helm.gpg] https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
sudo apt-get update
sudo apt-get install helm
```

2. Add Bitnami repo

```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
```

3. Add ENV variable KUBECONFIG for the helm util:

```bash
export KUBECONFIG=/etc/rancher/k3s/k3s.yaml
```

## Prepare To  Install Wordpress

1. Clone this repository:

```bash
git clone https://github.com/ru-ab/devops-task-5.git
```

2. Install dependencies:

```bash
helm dependency build wordpress
```

3. Modify environment variables in `values.yaml` file if needed:

- Wordpress
  - wordpressUsername
  - wordpressPassword
  - wordpressEmail
- MySQL
  - rootPassword 
  - username
  - password
  - database

## Deploy Wordpress Chart

Deploy the helm chart to the k3s cluster from the previously created folder:

```bash
helm install wordpress ./wordpress
```

## Verify Installation

Use the link http://k3s-public-ip to open it in your browser.
