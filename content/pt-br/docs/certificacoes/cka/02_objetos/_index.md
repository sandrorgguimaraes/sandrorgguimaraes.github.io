---
title: Objetos Básicos
date: 2022-05-25
weight: 20
description: Conhecendo os objetos básicos de um cluster Kubernetes.
resources:
- src: "**.{png,jpg}"
  title: "Image #:counter"
---

## POD's

São as menores unidades de computação implantáveis ​​que você pode criar e gerenciar no Kubernetes.

Um Pod modela um "host lógico" específico do aplicativo: ele contém um ou mais contêineres de aplicativos que são relativamente fortemente acoplados.

Além dos contêineres de aplicativos, um Pod pode conter [init contêineres](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/) que são executados durante a inicialização do Pod, e também podemos injetar [contêineres efêmeros](https://kubernetes.io/docs/concepts/workloads/pods/ephemeral-containers/) para depuração se seu cluster oferecer isso.

{{< imgproc pods Fill "668x277" >}} {{< /imgproc >}}

Link's úteis:

- [Documentação do Kubernetes - Conceitos - Cargas de trabalho - Pods](https://kubernetes.io/docs/concepts/workloads/pods/)
- [Documentação do Kubernetes - Tutoriais - Aprenda o básico do Kubernetes - Explorar seu aplicativo - Como visualizar pods e nós](https://kubernetes.io/docs/tutorials/kubernetes-basics/explore/explore-intro/)
- [Vivendo & Aprendendo - Kubernetes - Pod's](../../../../blog/kubernetes/pods/)

## ReplicaSets

Tem como finalidade manter um conjunto estável de réplicas de pods idênticos em execução a qualquer momento.

Link's úteis:

- [Documentação do Kubernetes - Conceitos - Cargas de trabalho - Recursos de carga de trabalho - ReplicaSets](https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/)

## Deployments

Permite atualizações declarativas para Pods e ReplicaSets.

Você descreve um estado desejado em um Deployment e o Deployment Controller altera o estado atual para o estado desejado a uma taxa controlada.

{{< imgproc deployments Fill "813x346" >}} {{< /imgproc >}}
Fonte.: [Kubernetes Pod naming convention](https://faun.pub/kubernetes-pod-naming-convention-78272fcc53ed).

Link's úteis:

- [Documentação do Kubernetes - Conceitos - Cargas de trabalho - Recursos de carga de trabalho - Deployments](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)
- [Documentação do Kubernetes - Tutoriais - Aprenda o básico do Kubernetes - Implantar um aplicativo - Usando kubectl para criar um Deployment](https://kubernetes.io/docs/tutorials/kubernetes-basics/deploy-app/deploy-intro/)

## Services

## Namespaces
