---
title: Manutenção do Cluster
date: 2022-06-09
weight: 60
description: Entendendo como podemos realizar a manutenção de um cluster Kubernetes.
---

## Drain, Cordon e Uncordon

Os comandos abaixo vão nos permitir efetuar a manutenção nos [Nodes](https://kubernetes.io/docs/concepts/architecture/nodes/) de forma controlada, mitigando possíveis problemas às aplicações.

- `cordon` == **marcar** o Node como não agendável;
  - Impedindo que novos Pods sejam agendados no mesmo.
- `drain` == **drenar** o Node.
  - Marca o Node como não agendável (igual o `cordon`) e;
  - Remove todos os Pods em execução, exceto os DaemonSets, criando-os em outros Nodes.
- `uncordon` == **desmarcar** o Node como não agendável.
  - Permitindo que *novas cargas* de trabalho / Pods sejam agendadas no mesmo.

> Link's úteis:
>
> - [Doc K8S - Conceitos - Arquitetura de cluster - Nodes # Administração manual de Nodes](https://kubernetes.io/docs/concepts/architecture/nodes/#manual-node-administration)
> - [Doc K8S - Tarefas - Administrar um cluster - Drain em um Nó com segurança](https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/)

## Atualizando a versão do Kubernetes

A maneira como atualizamos um cluster Kubernetes depende de como foi implantado inicialmente e das alterações realizadas posteriormente.

Em alto nível, as etapas são:

- Atualizar os [Nodes Control Plane](../01_componentes/#componentes-dos-master-nodes-ou-control-plane) (um Node por vez);
- Atualizar os [Nodes Workes](../01_componentes/#componentes-dos-worker-nodes) (um Node por vez);
- Atualizar os clientes como o `kubectl;
- Ajustar manifestos e outros recursos com base nas alterações da API que acompanham a nova versão do Kubernetes;

> Link's úteis:
>
> - [Doc K8S - Tarefas - Administrar um cluster - Atualizar um cluster](https://kubernetes.io/docs/tasks/administer-cluster/cluster-upgrade/)
> - [Doc K8S - Tarefas - Administrar um cluster - Administração com kubeadm - Como fazer upgrade de clusters kubeadm](https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade/)

## Backup e Restore ETCD

> Link's úteis:
>
> - [Doc K8S - Tarefas - Administrar um cluster - Operando clusters ETCD para Kubernetes # Fazendo o backup](https://kubernetes.io/docs/tasks/administer-cluster/configure-upgrade-etcd/#backing-up-an-etcd-cluster)
> - [Doc ETCD - Versões - v3.5 - Guia de Operações - Disaster Recovery](https://etcd.io/docs/v3.5/op-guide/recovery/)
