---
title: Agendamento
date: 2022-05-25
weight: 30
description: Entendendo como o agendamento ou 'scheduling' funciona em cluster Kubernetes.
---

Um agendador observa os [Pods](../02_objetos/#pods) recém-criados que não têm [Node](https://kubernetes.io/docs/concepts/architecture/nodes/) atribuído. Para cada Pod que o agendador descobre, o agendador se torna responsável por encontrar o melhor [Node](https://kubernetes.io/docs/concepts/architecture/nodes/) para esse Pod ser executado, uma vez concluído esse processo o [Kubelet](../01_componentes/#kubelet) do node escolhido então coloca o pod em execução.

O escalonador chega a esta decisão de posicionamento levando em consideração os princípios de escalonamento descritos abaixo.

## Agendamento manual

Defina diretamente na especificação do [Pods](../02_objetos/#pods) em qual [Node](https://kubernetes.io/docs/concepts/architecture/nodes/) ele será executado.

> Link's úteis:
>
> - [Documentação do Kubernetes - Conceitos - Agendamento, Preempção e Despejo - Atribuindo pods a nós - nodeName](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#nodename)

## Labels e Selectors

- Labels:

  - São pares de chave/valor anexados a objetos, como pods.
  - Devem ser usados ​​para especificar atributos de identificação de objetos que são significativos e relevantes para os usuários, mas não implicam diretamente na semântica do sistema central.
  - Podem ser usados ​​para organizar e selecionar subconjuntos de objetos.
  - Podem ser anexados a objetos no momento da criação e posteriormente adicionados e modificados a qualquer momento.
  - Cada chave deve ser exclusiva para um determinado objeto.

Ao contrário de [nomes e UIDs](https://kubernetes.io/docs/concepts/overview/working-with-objects/names/), *as labels não fornecem exclusividade*. Em geral, espera-se que muitos objetos tenham a(s) mesma(s) label(s).

- Selectors

Através de um label `selector`, o cliente/usuário pode identificar um conjunto de objetos. O label selector é a primitiva de agrupamento principal no Kubernetes.

> Link's úteis:
>
> - [Documentação do Kubernetes - Conceitos - Visão geral - Trabalhando com objetos do Kubernetes - Labels e Selectors](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/)

## Taints e Tolerations

Enquanto [Node Affinity](#node-affinity) é uma propriedade dos Pods que os atrai para um conjunto de nós (como uma preferência ou um requisito rígido), as `Taints` são o oposto, elas *permitem a um nó repelir um conjunto de pods*.

As `Tolerations` *são aplicadas aos pods e permitem (mas não exigem)* que os pods sejam agendados em nós com `Taints` correspondentes.

As `Taints` e as `Tolerations` *trabalham juntas para garantir* que os pods não sejam agendados em nós inadequados.

Uma ou mais Taints pode(m) ser aplicadas a um nó, isso marca que o nó *não deve aceitar* nenhum pod que não ***tolere*** essas Taints.

> Link's úteis:
>
> - [Documentação do Kubernetes - Conceitos - Agendamento, Preempção e Despejo - Taints e Tolerations](https://kubernetes.io/docs/concepts/scheduling-eviction/taint-and-toleration/)

## Node Selectors

`nodeSelector` é a forma mais simples recomendada de restrição de seleção de nó.

Ao adicionar o campo `nodeSelector` à especificação do pod e definir as [labels de nós](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#built-in-node-labels) que deseja que o nó de destino tenha, o Kubernetes agenda o pod apenas em nós que tenham cada uma das labels especificadas.

> Link's úteis:
>
> - [Documentação do Kubernetes - Conceitos - Agendamento, Preempção e Despejo - Atribuindo pods a nós - nodeSelector](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#nodeselector)
> [Documentação do Kubernetes - Tarefas - Configurar pods e contêineres - Atribuir pods a nós](https://kubernetes.io/docs/tasks/configure-pod-container/assign-pods-nodes/)

## Node Affinity

A `Node Affinity` é conceitualmente semelhante a [nodeSelector](#node-selectors), permitindo restringir em quais nós o pod pode ser agendado com base em [labels de nós](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#built-in-node-labels).

Existem dois tipos de `Node Affinity`:

- `requiredDuringSchedulingIgnoredDuringExecution`: o agendador *não pode agendar* o pod a menos que a regra seja atendida. Isso funciona como [nodeSelector](#node-selectors), *mas com uma sintaxe mais expressiva*.
- `preferredDuringSchedulingIgnoredDuringExecution`: o agendador *tenta encontrar* um nó que atenda à regra. Se um nó correspondente não estiver disponível, o agendador *ainda agendará* o pod.

> Link's úteis:
>
> - [Documentação do Kubernetes - Conceitos - Agendamento, Preempção e Despejo - Atribuindo pods a nós - Node affinity](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#node-affinity)
> - [Documentação do Kubernetes - Conceitos - Agendamento, Preempção e Despejo - Atribuindo pods a nós - Affinity and anti-affinity](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#affinity-and-anti-affinity)
> - [Documentação do Kubernetes - Tarefas - Configurar pods e contêineres - Atribuir pods a nós usando afinidade de nó](https://kubernetes.io/docs/tasks/configure-pod-container/assign-pods-nodes-using-node-affinity/)

## Taints e Tolerations X Node Affinity

## Resources Requirements e Limits

Quando você define um Pod, você pode opcionalmente especificar quanto de cada recurso um Pod precisa, os recursos mais comuns a serem especificados são [CPU](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/#meaning-of-cpu) e [memória (RAM)](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/#meaning-of-memory); existem outros [tipos de recursos](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/#resource-types).

Quando você especifica o `requests` de um recurso para contêineres em um pod, o [kube-scheduler](../01_componentes/#scheduler) usa essas informações para decidir em qual nó colocar o pod.

Quando você especifica um `limits` de recurso para um contêiner, o [kubelet](../01_componentes/#kubelet) impõe esses `limits` para que o contêiner em execução não tenha permissão para usar mais desse recurso do que o `limits` definido.

O [kubelet](../01_componentes/#kubelet) também reserva pelo menos a quantidade de `requests` desse recurso do sistema *especificamente* para esse contêiner usar.

> Link's úteis:
>
> - [Documentação do Kubernetes - Conceitos - Configuração - Gerenciamento de recursos para pods e contêineres](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/)
> - [Documentação do Kubernetes - Tarefas - Configurar pods e contêineres - Atribuir recursos de memória a contêineres e pods](https://kubernetes.io/docs/tasks/configure-pod-container/assign-memory-resource/)
> - [Documentação do Kubernetes - Tarefas - Administrar um cluster - Gerenciar recursos de memória, CPU e API](https://kubernetes.io/docs/tasks/administer-cluster/manage-resources/)

## DaemonSets

Um `DaemonSet` garante que todos (ou alguns) nós executem uma cópia em um pod.

À medida que os nós são adicionados ao cluster, os pods são adicionados a eles. À medida que os nós são removidos do cluster, esses pods são coletados como lixo.

A exclusão de um `DaemonSet` limpará os pods que ele criou.

Alguns usos típicos de um `DaemonSet` são:

- Executando um daemon de armazenamento de cluster em cada nó.
- Executando um daemon de coleta de logs em cada nó.
- Executando um daemon de monitoramento de nó em cada nó.

> Link's úteis:
>
> - [Documentação do Kubernetes - Conceitos - Cargas de trabalho - Recursos de carga de trabalho - DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/)
> - [Documentação do Kubernetes - Tarefas - Gerenciar daemons de cluster](https://kubernetes.io/docs/tasks/manage-daemon/)

## Static Pods

Os `Static Pods` são gerenciados diretamente pelo daemon [kubelet](../01_componentes/#kubelet) em um nó específico, sem o [API Server](../01_componentes/#api-server) observando-os.

Enquanto a maioria dos Pods é gerenciada pelo [Control Plane](../01_componentes/#componentes-dos-master-nodes-ou-control-plane) (por exemplo, um [Deployments](../02_objetos/#deployments)), para `Static Pods`, o [kubelet](../01_componentes/#kubelet) supervisiona diretamente cada `Static Pods` (e o reinicia se falhar).

> Link's úteis:
>
> - [Documentação do Kubernetes - Conceitos - Cargas de trabalho - Pods](https://kubernetes.io/docs/concepts/workloads/pods/#static-pods)
> - [Documentação do Kubernetes - Tarefas - Configurar pods e contêineres - Criar pods estáticos](https://kubernetes.io/docs/tasks/configure-pod-container/static-pod/)

## Multiplos Kubernetes Schedulers

Se o agendador padrão não atender às suas necessidades, você pode implementar seu próprio agendador. Além disso, você pode até executar vários agendadores simultaneamente ao lado do agendador padrão e instruir o Kubernetes sobre qual agendador usar para cada um de seus pods.

> Link's úteis:
>
> - [Documentação do Kubernetes - Tarefas - Estender Kubernetes - Configurar vários agendadores](https://kubernetes.io/docs/tasks/extend-kubernetes/configure-multiple-schedulers/)
