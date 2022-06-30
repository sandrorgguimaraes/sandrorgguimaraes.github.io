---
title: Armazenamento
date: 2022-06-29
weight: 80
description: Entendendo como funciona o armazenamento de dados nos cluster Kubernetes.
---

## Conceitos sobre armazenamento em Containers

> Link's úteis:
>
> - [Doc Docker - Execute seu aplicativo em produção - Gerenciar dados do aplicativo - Visão geral do armazenamento](https://docs.docker.com/storage/)
> - [Doc Docker - Execute seu aplicativo em produção - Gerenciar dados do aplicativo - Armazenar dados em contêineres](https://docs.docker.com/storage/storagedriver/)

## Volumes

O sistema de arquivos de um Container vive apenas enquanto o Container existir. Portanto, quando um Container é encerrado e reiniciado, as alterações no sistema de arquivos são perdidas.

A abstração de `Volumes` do Kubernetes resolve este problema, fornecendo um armazenamento mais consistente e independente do Container, o que é especialmente importante para aplicativos com estado, como bancos de dados.

> Link's úteis:
>
> - [Doc K8S - Conceitos - Armazenar - Volumes](https://kubernetes.io/docs/concepts/storage/volumes/)
> - [Doc K8S - Conceitos - Armazenar - Volumes # Plugins](https://kubernetes.io/docs/concepts/storage/volumes/#out-of-tree-volume-plugins)

## Persistent Volumes (PV)

Um `Persistent Volume` (PV) é uma parte do armazenamento no cluster que foi provisionado por um administrador ou provisionado dinamicamente usando `Storage Class`.

É um recurso no cluster, assim como um nó é um recurso de cluster.

`PVs` são plugins de volume como Volumes, mas têm um ciclo de vida independente de qualquer Pod individual que use o `PV`.

Esse objeto de API captura os detalhes da implementação do armazenamento, seja NFS, iSCSI ou um sistema de armazenamento específico do provedor de nuvem.

> Link's úteis:
>
> - [Doc K8S - Conceitos - Armazenar - Persistent Volumes](https://kubernetes.io/docs/concepts/storage/persistent-volumes/)
> - [Doc K8S - Tarefas - Configurar pods e contêineres - Configurar um pod para usar um Persistent Volume](https://kubernetes.io/docs/tasks/configure-pod-container/configure-persistent-volume-storage/)

## Persistent Volumes Claims (PVC)

Um `Persistent Volume Claim` (PVC) é uma solicitação de armazenamento feita por um usuário, é semelhante a um Pod.

Enquanto os pods consomem recursos do nó, os `PVCs` consomem recursos dos `PV`.

> Link's úteis:
>
> - [Doc K8S - Conceitos - Armazenar - Persistent Volumes Claims](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#persistentvolumeclaims)

## Storage Class

Um `Storage Class` fornece uma maneira para os administradores descreverem as "classes" de armazenamento que eles oferecem.

Classes diferentes podem ser mapeadas para níveis de qualidade de serviço, políticas de backup ou políticas arbitrárias determinadas pelos administradores de cluster.

> Link's úteis:
>
> - [Doc K8S - Conceitos - Armazenar - Storage Class](https://kubernetes.io/docs/concepts/storage/storage-classes/)
> - [Doc K8S - Tarefas - Administrar um cluster - Altere o Storage Class padrão](https://kubernetes.io/docs/tasks/administer-cluster/change-default-storage-class/)
