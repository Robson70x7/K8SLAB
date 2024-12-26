# Install kubectl

Download kubectl

```sh
curl -LO "https://dl.k8s.io/release/v1.24.2/bin/linux/amd64/kubectl"
```

installation

```sh
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
```

# Intall minikube

```sh
curl -LO https://storage.googleapis.com/minikube/releases/v1.26.0/minikube-linux-amd64
chmod +x minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
sudo usermod -aG docker $USER
```
