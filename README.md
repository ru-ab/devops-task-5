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

2. Add ENV variable KUBECONFIG for the helm util:

```bash
export KUBECONFIG=/etc/rancher/k3s/k3s.yaml
```

## Create Wordpress Chart

1. Add Bitnami repo

```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
```

2. Download the wordpress chart from Bitnami:

```bash
helm pull bitnami/wordpress
```

3. Extract files from the downloaded archive:

```bash
tar -xvf wordpress-x.x.x.tgz
```

4. Modify `wordpressUsername` and `wordpressPassword` values in the **values.yaml** file.

## Deploy Wordpress Chart

Deploy the helm chart to the k3s cluster from the previously created folder:

```bash
helm install wordpress ./wordpress
```

## Verify Installation

Use the link http://k3s-public-ip to open it in your browser.
