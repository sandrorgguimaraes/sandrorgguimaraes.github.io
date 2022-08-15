---
title: Gerenciar Ciclos de Vida de Aplicações
date: 2022-06-06
weight: 50
description: Entendendo como gerenciar o ciclo de vida das aplicações em um cluster Kubernetes.
---

## Rolling Updates e Rollbacks

- `Rolling Updates` permitem que a atualização de um [Deployment](../02_objetos/#deployments) ocorra com tempo de inatividade zero, atualizando incrementalmente as instâncias de novos [Pods](../02_objetos/#pods).

> Link's úteis:
>
> - [Doc K8S - Conceitos - Cargas de trabalho - Recursos de carga de trabalho - Deployments # Rolling Update](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#rolling-update-deployment)
> - [Doc K8S - Tutoriais - Aprenda o básico do Kubernetes - Atualize seu aplicativo - Executando um Rolling Update](https://kubernetes.io/docs/tutorials/kubernetes-basics/update/update-intro/)
> - [Doc K8S - Tarefas - Gerenciar daemons de cluster - Executar um Rolling Update em um DaemonSet](https://kubernetes.io/docs/tasks/manage-daemon/update-daemon-set/)

- `Rollbacks` permitem retornar a uma versão anterior de um [Deployment](../02_objetos/#deployments), por padrão o Kubernetes mantém um histórico de 10 versões.

> Link's úteis:
>
> - [Doc K8S - Conceitos - Cargas de trabalho - Recursos de carga de trabalho - Deployments # Rollback](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#rolling-update-deployment)
> - [Doc K8S - Tarefas - Gerenciar daemons de cluster - Executar um Rollback em um DaemonSet](https://kubernetes.io/docs/tasks/manage-daemon/rollback-daemon-set/)

## Configurando aplicações

Veremos abaixo algumas formas de permitir a portabilidade e a reusabilidade das imagens de container, usando a abordagem de separação do código da aplicação das configurações e variáveis que podemos passar durante a inicialização dos [Pods](../02_objetos/#pods).

### Comandos e Argumentos

O `comando` e os `argumentos` que definimos na especificação do Pod substituem o `comando` e os `argumentos` padrão fornecidos pela imagem do contêiner.

Se definirmos `argumentos`, mas não definirmos um `comando`, o `comando` padrão será usado com seus *novos* `argumentos`.

> Link's úteis:
>
> - [Doc K8S - Tarefas - Injetar dados em aplicativos - Definir um comando e argumentos para um contêiner](https://kubernetes.io/docs/tasks/inject-data-application/define-command-argument-container/)

### Variáveis de ambiente

Ao criarmos um [Pod](../02_objetos/#pods), podemos definir `variáveis ​​de ambiente` para os containers executados no [Pod](../02_objetos/#pods), para tanto inclua o campo `env` ou `envFrom` na especificação do container.

> Link's úteis:
>
> - [Doc K8S - Tarefas - Injetar dados em aplicativos - Definir variáveis ​​de ambiente para um contêiner](https://kubernetes.io/docs/tasks/inject-data-application/define-environment-variable-container/)
> - [Doc K8S - Tarefas - Injetar dados em aplicativos - Expor informações do Pod ao container por meio de variáveis ​​de ambiente](https://kubernetes.io/docs/tasks/inject-data-application/environment-variable-expose-pod-information/)

### ConfigMaps

Um `ConfigMap` é um objeto de API usado para armazenar *dados não confidenciais* em pares de chave-valor.

Os [Pods](../02_objetos/#pods) podem consumir `ConfigMaps` como variáveis ​​de ambiente, argumentos de linha de comando ou como arquivos de configuração em um volume.

> Link's úteis:
>
> - [Doc K8S - Conceitos - Configuração - ConfigMaps](https://kubernetes.io/docs/concepts/configuration/configmap/)
> - [Doc K8S - Tarefas - Configurar pods e contêineres - Configurar um pod para usar um ConfigMap](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/)

### Secrets

`Secrets` são semelhantes aos `ConfigMaps` mas destinam-se especificamente a manter *dados confidenciais*.

> Link's úteis:
>
> - [Doc K8S - Conceitos - Configuração - Secrets](https://kubernetes.io/docs/concepts/configuration/secret/)
> - [Doc K8S - Tarefas - Gerenciando Secrets](https://kubernetes.io/docs/tasks/configmap-secret/)
> - [Doc K8S - Tarefas - Injetar dados em aplicativos - Distribua credenciais com segurança usando Secrets](https://kubernetes.io/docs/tasks/inject-data-application/distribute-credentials-secure/)

## Pods com multiplos containers

Os [Pods](../02_objetos/#pods) são projetados para oferecer suporte a vários processos cooperativos (como containers) que formam uma unidade de serviço coesa.

Os containers em um [Pod](../02_objetos/#pods) são colocados e co-agendados automaticamente na mesma máquina física ou virtual no cluster, podem compartilhar recursos e dependências, comunicar-se entre si e coordenar quando e como eles serão encerrados.

> Link's úteis:
>
> - [Doc K8S - Conceitos - Cargas de trabalho - Pods # Como os pods gerenciam vários contêineres](https://kubernetes.io/docs/concepts/workloads/pods/#how-pods-manage-multiple-containers)
> - [Blog K8S - O kit de ferramentas do sistema distribuído: Padrões para contêineres compostos](https://kubernetes.io/blog/2015/06/the-distributed-system-toolkit-patterns/)

## InitContainers

Os `initContainers` são containers especializados que são executados antes dos containers de aplicativos em um [Pod](../02_objetos/#pods), eles são exatamente como os containers normais, exceto que:

- Os `initContainers` sempre são executados até a conclusão.
- Cada `initContainers` deve ser concluído com êxito antes que o próximo seja iniciado.

> Link's úteis:
>
> - [Doc K8S - Conceitos - Cargas de trabalho - Pods # Init Containers](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/)
> - [Doc K8S - Tarefas - Configurar pods e contêineres - Configurar inicialização do Pod](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-initialization/)
> - [Doc K8S - Tarefas - Monitoramento, Logs e Debug - Troubleshooting de aplicativos - Debug Init Containers](https://kubernetes.io/docs/tasks/debug/debug-application/debug-init-containers/)

## Self Healing (auto cura) das aplicações

O Kubernetes oferece suporte de self healing (auto recuperação) a aplicativos por meio de [ReplicaSets](../02_objetos/#replicasets).

O [ReplicaSets](../02_objetos/#replicasets) ajuda a garantir que um [Pod](../02_objetos/#pods) seja recriado automaticamente quando o aplicativo dentro do [Pod](../02_objetos/#pods) trava, isso ajuda a garantir que réplicas suficientes do aplicativo estejam em execução o tempo todo.

O Kubernetes fornece suporte adicional para verificar a integridade dos aplicativos executados em [Pods](../02_objetos/#pods) e realizar as ações necessárias por meio de [Liveness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/#define-a-liveness-command) e [Readiness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/#define-readiness-probes), porém esses 2 (dois) tópicos são para o exame [Certified Kubernetes Application Developers (CKAD)](https://www.cncf.io/certification/ckad/), não são abordados nesta certificação para CKA.
