---
title: Multi Cluster
date: 2022-03-24
categories:
    - kubernetes
tags:
    - kind
    - cilium
    - istio
    - submariner
autores:
    - Piotr Mińkowski
description: Vamos ver alguns materiais sobre Multi Clusters.
keywords:
    - kind
    - cilium
    - istio
    - submariner
slug: multicluster
---

{{% pageinfo %}}
Kind (***K**ubernetes **in** **D**ocker*) é uma ferramenta para executar clusters locais do Kubernetes usando contêineres do Docker como nodes do Kubernetes, foi projetado principalmente para testar o próprio Kubernetes, mas pode ser usado para desenvolvimento local ou [CI (*Continuous Integration*)](/blog/devops/ci-cd/).
{{% /pageinfo %}}

---

## [Kubernetes Multicluster with Kind and Cilium](https://piotrminkowski.com/2021/10/25/kubernetes-multicluster-with-kind-and-cilium/)

por [**Piotr Mińkowski**](/autores/piotr-mińkowski/) em 25/10/2021.

Aprenda como configurar um multicluster Kubernetes localmente com [Kind](https://kind.sigs.k8s.io/) e [Cilium](https://cilium.io/).

> Cilium é um software de código aberto para fornecer, proteger e observar a conectividade de rede entre cargas de trabalho de contêiner - nativo da nuvem e alimentado pela revolucionária tecnologia Kernel eBPF.

---

## [Multicluster Traffic Mirroring with Istio and Kind](https://piotrminkowski.com/2021/07/12/multicluster-traffic-mirroring-with-istio-and-kind/)

por [**Piotr Mińkowski**](/autores/piotr-mińkowski/) em 12/07/2021.

Aprenda como criar uma malha do [Istio](https://istio.io/) com espelhamento entre vários clusters Kubernetes em execução no [Kind](https://kind.sigs.k8s.io/). Implante o mesmo aplicativo nos dois clusters Kubernetes e, em seguida, espelhe o tráfego entre eles

> O Istio estende o Kubernetes para estabelecer uma rede programável e com reconhecimento de aplicativos usando o poderoso proxy de serviço Envoy. Trabalhando com Kubernetes e cargas de trabalho tradicionais, o Istio traz gerenciamento de tráfego padrão e universal, telemetria e segurança para implantações complexas.

---

## [Kubernetes Multicluster with Kind and Submariner](https://piotrminkowski.com/2021/07/08/kubernetes-multicluster-with-kind-and-submariner/)

por [**Piotr Mińkowski**](/autores/piotr-mińkowski/) em 08/07/2021.

Aprenda como criar vários clusters Kubernetes localmente e estabelecer comunicação direta entre eles com [Kind](https://kind.sigs.k8s.io/) e [Submariner](https://submariner.io/). O objetivo deste artigo é estabelecer comunicação direta entre pods executados em dois clusters Kubernetes diferentes.

---
