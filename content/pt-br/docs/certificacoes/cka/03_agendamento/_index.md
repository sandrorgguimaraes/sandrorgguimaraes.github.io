---
title: Agendamento
date: 2022-05-25
weight: 30
description: Entendendo como o agendamento ou 'scheduling' funciona em cluster Kubernetes.
---

Um agendador observa os [Pods](../02_objetos/#pods) recém-criados que não têm [Node](../../../../blog/kubernetes/nodes) atribuído. Para cada Pod que o agendador descobre, o agendador se torna responsável por encontrar o melhor Node para esse Pod ser executado, uma vez concluído esse processo o [Kubelet](../01_componentes/#kubelet) então coloca o pod em execução.

O escalonador chega a esta decisão de posicionamento levando em consideração os princípios de escalonamento descritos abaixo.

## Agendamento manual

Defina diretamente na especificação do [Pods](../02_objetos/#pods) em qual [Node](../../../../blog/kubernetes/nodes) ele será executado.

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

## Node Selectors

## Node Affinity

## Taints e Tolerations X Node Affinity

## Resources Requirements e Limits

## DaemonSets

## Static Pods

## Multiplos Kubernetes Schedulers
