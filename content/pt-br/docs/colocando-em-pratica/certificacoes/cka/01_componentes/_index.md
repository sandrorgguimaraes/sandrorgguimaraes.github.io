---
title: Componentes Básicos
date: 2022-05-25
weight: 10
description: Conhecendo os componentes básicos de um cluster Kubernetes.
resources:
- src: "**.{png,jpg}"
  title: "Image #:counter"
---

Um [cluster](https://en.wikipedia.org/wiki/Computer_cluster) Kubernetes consiste em um conjunto de máquinas de trabalho (físicas ou virtuais), chamadas [`Node` ou `Nó`](https://kubernetes.io/docs/concepts/architecture/nodes/) que executam aplicativos em contêiner.

Em essência existem 2 (dois) tipos de nós em um cluster Kubernetes:

- Os `nós workers` que hospedam os Pods que são os componentes da carga de trabalho do aplicativo.
- Os `nós masters` ou `control plane` que gerenciam os `nós works` e os pods no cluster.

Em ambiente de produção, o `control plane` geralmente é executado em vários computadores e um cluster geralmente executa vários `nós workers` fornecendo tolerância a falhas e alta disponibilidade.

Em ambientes de estudo podemos ter apenas uma máquina exercendo as duas funções.

## Visão geral da arquitetura de um cluster k8s

{{< imgproc arquitetura_k8s Fill "839x410" >}} {{< /imgproc >}}
Fonte.: [Kubernetes — Architecture Overview](https://medium.com/devops-mojo/kubernetes-architecture-overview-introduction-to-k8s-architecture-and-understanding-k8s-cluster-components-90e11eb34ccd).

> Link's úteis:
>
> - [Doc K8S - Conceitos - Visão geral](https://kubernetes.io/docs/concepts/overview/)
> - [Doc K8S - Conceitos - Arquitetura de cluster](https://kubernetes.io/docs/concepts/architecture/)

## Componentes dos Master Nodes ou Control Plane

### ETCD

É um armazenamento do tipo `chave-valor` distribuído e fortemente consistente que fornece uma maneira confiável de armazenar dados que precisam ser acessados ​​por um sistema distribuído ou cluster de máquinas. Ele lida com as eleições do líder durante as partições da rede e pode tolerar falhas de máquina, mesmo no nó líder.

> Link's úteis:
>
> - [Site oficial](https://etcd.io/)

### API Server

O servidor de API do Kubernetes valida e configura dados para os objetos de API que incluem pods, services, replicaSets e outros.

O API Server atende às operações REST/kubectl e fornece o frontend para o estado compartilhado do cluster por meio do qual todos os outros componentes interagem.

> Link's úteis:
>
> - [Doc K8S - Referência - Ferramentas de componentes - kube-apiserver](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-apiserver/)

### Controller Manager

No Kubernetes, controladores são ciclos de controle que observam o estado do cluster, e então fazem ou requisitam mudanças onde e quando necessário.

Cada controlador tenta mover o *estado atual* do cluster mais perto do *estado desejado*.

> Link's úteis:
>
> - [Doc K8S - Conceitos - Arquitetura do Kubernetes - Controladores](https://kubernetes.io/pt-br/docs/concepts/architecture/controller/)
> - [Doc K8S - Referência - Ferramentas de componentes - kube-controller-manager](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-controller-manager/)

### Scheduler

Observa os pods recém-criados sem nenhum nó atribuído e seleciona um nó para executá-los.

Os fatores levados em consideração para as decisões de agendamento incluem:

- Requisitos de recursos individuais e coletivos;
- Hardware/software/política de restrições;
- Especificações de afinidade e antiafinidade;
- Localidade dos dados;
- Interferência entre cargas de trabalho; e
- Prazos.

> Link's úteis:
>
> - [Doc K8S - Conceitos - Agendamento, Preempção e Despejo - Agendador do Kubernetes](https://kubernetes.io/docs/concepts/scheduling-eviction/kube-scheduler/)
> - [Doc K8S - Referência - Ferramentas de componentes - kube-scheduler](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-scheduler/)
> - [Vivendo & Aprendendo - Kubernetes - Scheduler](../../../../vivendo-e-aprendendo/kubernetes/scheduler/)

## Componentes dos Worker Nodes

### Kubelet

*É o principal agente* executado em cada worker node. O kubelet usa um conjunto de PodSpecs fornecidos por meio de vários mecanismos (principalmente por meio do apiserver) e garante que os contêineres descritos nesses PodSpecs estejam em execução e íntegros.

O kubelet não gerencia contêineres que não foram criados pelo Kubernetes.

> Link's úteis:
>
> - [Doc K8S - Referência - Ferramentas de componentes - kubelet](https://kubernetes.io/docs/reference/command-line-tools-reference/kubelet/)

### Kube Proxy

É um proxy de rede executado em cada nó no cluster, implementando parte do conceito de serviço do Kubernetes.

O `kube-proxy` mantém regras de rede nos nós, estas regras permitem a comunicação de rede com os pods a partir de sessões de rede dentro ou fora do cluster.

> Link's úteis:
>
> - [Doc K8S - Referência - Ferramentas de componentes - kube-proxy](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-proxy/)

### Container runtime

É o software responsável pela execução dos contêineres.

O Kubernetes oferece suporte a ambientes de container runtime como o [containerd](https://containerd.io/docs/), o [CRI-O](https://cri-o.io/#what-is-cri-o) e qualquer outra implementação do [Kubernetes CRI (Container Runtime Interface)](https://github.com/kubernetes/community/blob/master/contributors/devel/sig-node/container-runtime-interface.md).
