

**StatefulSet**
================

Um StatefulSet é um tipo de recurso no Kubernetes que gerencia aplicações stateful, ou seja, aplicações que mantêm estado entre as reinicializações. Ele é semelhante a um Deployment, mas com algumas diferenças importantes.
Um StatefulSet precisa de um Service para poder ser criado.

**Características**
-----------------

*   **Manutenção de estado**: Os StatefulSets mantêm o estado das aplicações entre as reinicializações, o que é fundamental para aplicações que precisam manter dados consistentes.
*   **Ordem de inicialização**: Os StatefulSets garantem que as réplicas sejam inicializadas em ordem sequencial, o que é importante para aplicações que precisam de uma ordem específica de inicialização.
*   **Nomes de réplicas**: Os StatefulSets atribuem nomes únicos às réplicas, o que permite que as aplicações se comuniquem entre si de forma eficiente.
*   **Volumes persistentes**: Os StatefulSets suportam volumes persistentes, o que permite que as aplicações armazenem dados de forma persistente.

**Como funciona**
-----------------

1.  **Criação**: Um StatefulSet é criado com um manifesto YAML ou JSON que define a configuração da aplicação.
2.  **Incialização**: O Kubernetes inicia as réplicas do StatefulSet em ordem sequencial, garantindo que cada réplica seja inicializada antes de iniciar a próxima.
3.  **Manutenção de estado**: O StatefulSet mantém o estado das réplicas entre as reinicializações, garantindo que as aplicações mantenham seus dados consistentes.
4.  **Atualização**: O StatefulSet suporta atualizações de forma controlada, garantindo que as réplicas sejam atualizadas de forma ordenada e sem interrupções.

**Exemplo de manifesto**
------------------------

```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: meu-statefulset
spec:
  serviceName: "meu-servico"
  replicas: 3
  selector:
    matchLabels:
      app: meu-app
  template:
    metadata:
      labels:
        app: meu-app
    spec:
      containers:
      - name: meu-container
        image: meu-imagem
        volumeMounts:
        - name: meu-volume
          mountPath: /mnt/data
  volumeClaimTemplates:
  - metadata:
      name: meu-volume
    spec:
      accessModes: [ "ReadWriteOnce" ]
      resources:
        requests:
          storage: 1Gi
```

**Adicionando ao arquivo selecionado**
--------------------------------------

Para adicionar o StatefulSet ao seu arquivo de manifesto, basta copiar e colar o exemplo acima e adaptá-lo às suas necessidades específicas.

Lembre-se de que é importante entender as características e como funciona o StatefulSet antes antes de Kubernetes antes
