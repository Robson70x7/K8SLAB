# RBAC no Kubernetes
=====================

O RBAC (Role-Based Access Control) é um recurso do Kubernetes que permite controlar o acesso aos recursos do cluster com base em funções e permissões. Com o RBAC, você pode definir quais usuários ou grupos têm permissão para realizar ações específicas em um cluster.

###  Como funciona o RBAC
O RBAC no Kubernetes é baseado em três componentes principais:

* Roles: São conjuntos de permissões que podem ser atribuídas a usuários ou grupos.
* RoleBindings: São associações entre roles e usuários ou grupos.
ClusterRoles: São conjuntos de permissões que podem ser atribuídas a usuários ou grupos em todo o cluster.

### Tipos de Roles
Existem dois tipos de roles no Kubernetes:

* Role: É um conjunto de permissões que pode ser atribuído a usuários ou grupos em um namespace específico.

* ClusterRole: É um conjunto de permissões que pode ser atribuído a usuários ou grupos em todo o cluster.

### Criando Roles
Para criar um role, você pode usar o comando kubectl create role. Por exemplo:

```bash
kubectl create role my-role --verb=get --resource=pods
```
Esse comando cria um role chamado my-role que tem permissão para realizar a ação get no recurso `pods`.

### Verbos para Roles
O kubernetes utiliza alguns verbos espécificos para sinalizar qual é o tipo de permissão que será atribuido. Segue abaixo os tipo utilizados

<table>
<thead>
<tr>
<td>Verbo</td>
<td>Descrição</td>
</tr>
</thread>
<tbody>
<tr>
<td>Get</td>
<td>Permisão de Leitura de um recurso especifico. Ex: kubectl get pod pod-id</td>
</tr>
<tr>
<td>list</td>
<td>Permisão de Leitura para conseguir listar todos os recursos crirados. 
Ex: kubectl get pods --all</td>
</tr>
<tr>
<td>create</td>
<td>Permisão de criação</td>
</tr>
<tr>
<td>update</td>
<td>Permisão de atualização</td>
</tr>
<tr>
<td>pacth</td>
<td>Permisão de atualização parcial</td>
</tr>
<tr>
<td>delete</td>
<td>Permisão de exclusão</td>
</tr>
<tr>
<td>deletecollection</td>
<td>Permisão de exclusão massiva (--all)</td>
</tr>
<tr>
<td>watch</td>
<td>Permisão de monitoramento de comandos</td>
</tr>
</tbody>
</table>

### Recursos da role
Em uma role você especifica quais recursos os verbos irá controlar o permissionamento, como por exemplo 
`Pods` `Configmaps` `Services` `Secrets` Èndpoints` etc.


## Comandos das aulas

### Gerando certificados

Vamos criar os certificados necessários para utilizalos no RBAC.
Vamos utilizar o `openssl`para geração dos mesmos.

### auditor.key
```sh
openssl genrsa -out auditor.key 2048

openssl req -new -key auditor.key -out auditor.csr -subj "/CN=auditor/O=MyCompany"

openssl x509 -req -in auditor.csr -CA ~/.minikube/ca.crt -CAkey ~/.minikube/ca.key -CAcreateserial -out auditor.crt -days 365

kubectl config set-credentials auditor --client-certificate=auditor.crt --client-key=auditor.key

kubectl config set-context auditor-context --cluster=minikube --user=auditor
```

### Fluxo de validação

Para conseguir visualizar a diferença dos recursos seguir o seguinte fluxo.

1. Antes de aplicar os aruivos de Roles e RoleBinds, mude para o context criado no comando acima, com o seguinte comando
`kubectl config use-context auditor-context`

2. Agora para validar que o usuario não tem acesso nenhum no cluester, tente rodar algum comando como por exemplo: `kubectl get po`, o resultado será um forbiden.

3. Você também pode rodar alguns comandos para validar se tem acesso a algum recurso 
```sh
kubectl auth can-i '*' '*' --all-namespaces
kubectl auth can-i get pods -n=default 
kubectl auth can-i list pods
kubectl auth can-i watch pods
kubectl auth can-i get svc
kubectl auth can-i list svc
kubectl auth can-i watch svc
```
Você verá que o resultado até o momento será `no`, pois o usuario ainda não tem acesso.

4. Agora vamos aplicar o arquivo, porém este contexto não tem permissão de criação, então vamos voltar para o contexto principal que no caso é o minikube.
``` sh
kubectl config use-context minikube
kubectl apply -f RBAC/auditor-role.yml
```

5. Como este arquivo não da permissão de criação, para fim de testes vamos criar 2 pods de exemplo.
```sh
kubectl run --image nginx test-1
kubectl run --image nginx test-2
kubectl get po
```
Você deve visualizar os novos pods criados.

6. Agora podemos voltar para o contexto de auditor e refazer os testes do auth can-i. 
Também vamos tentar listar os pods criados anteriormente, tente fazer todos os verbos liberados `get` `list` `watch`.

7. Por fim vamos limpar o cluster.

```sh
kubectl config use-context minikube
kubectl delete -f RBAC/auditor-role.yml
kubectl delete pod --all
kubectl config delete-context auditor-context
kubectl config unset users.auditor
# remover certifiados, caso queira
rm -r RBAC/certificate
```




