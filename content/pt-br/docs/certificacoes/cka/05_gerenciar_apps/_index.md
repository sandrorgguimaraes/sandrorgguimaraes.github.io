---
title: Gerenciar Ciclos de Vida de Aplicações
date: 2022-06-06
weight: 50
description: Entendendo como gerenciar o ciclo de vida das aplicações
---

## Rolling Updates e Rollbacks

## Configurando aplicações

### Comandos e Argumentos

### Variáveis de ambiente

### ConfigMaps

### Secrets

## Pods com multiplos containers

## InitContainers

## Self Healing (auto cura) das aplicações

O Kubernetes oferece suporte de self healing (auto recuperação) a aplicativos por meio de [ReplicaSets](../02_objetos/#replicasets).

O [ReplicaSets](../02_objetos/#replicasets) ajuda a garantir que um POD seja recriado automaticamente quando o aplicativo dentro do POD trava, isso ajuda a garantir que réplicas suficientes do aplicativo estejam em execução o tempo todo.

O Kubernetes fornece suporte adicional para verificar a integridade dos aplicativos executados em PODs e realizar as ações necessárias por meio de [Liveness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/#define-a-liveness-command) e [Readiness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/#define-readiness-probes), porém esses 2 (dois) tópicos são para o exame [Certified Kubernetes Application Developers (CKAD)](https://www.cncf.io/certification/ckad/), não são abordados nesta certificação para CKA.
