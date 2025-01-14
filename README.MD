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


### Guia de boas praticas kubernets

* Sempre realize administração por arquivos de manifesto

* Evite execução imperativas, direto no terminal.

* Sempre faça revisão e auditoria nos arquivos

* Utilize sempre um controlador de versões

* Faça a separação por camadas Layers (Frontend, backend, etc) no controle de versão.

* Sempre crie um recurso por arquivo de manifesto .yml. 

* Busque regiões para ter a menor latencia

* Evite ter diferentes versões da aplicação em diferentes regiões

## Exemplo de estrutura

```sh
production/
    frontend/
        current/
            frontend-deployment.yml
            frontend-service.yml
        v2/
            frontend-deployment.yml
            frontend-service.yml
        v3/
            frontend-deployment.yml
            frontend-service.yml
```
