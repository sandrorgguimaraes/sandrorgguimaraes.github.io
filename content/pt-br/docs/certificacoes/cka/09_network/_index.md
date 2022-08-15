---
title: Network
date: 2022-07-04
weight: 90
description: Entendendo como funciona a rede (network) nos cluster Kubernetes.
resources:
- src: "**.{png,jpg}"
  title: "Image #:counter"
---

## Conceitos básicos de Redes

Abaixo alguns tópicos básicos para direcionar o entendimento sobre o funcionamento de uma rede de computadores:

### Geral

- [Switching / Routing / Gateway](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/docs/09-Networking/02-Pre-requisite-Switching-Routing-Gateways.md);
- [DNS](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/docs/09-Networking/03-Pre-requisite-DNS.md);
- [coreDNS](https://github.com/coredns/coredns);

### Linux / Container

- [Network Namespaces no Linux](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/docs/09-Networking/05-Pre-requisite-Network-Namespace.md);
- [Docker Network](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/docs/09-Networking/06-Pre-requisite-Docker-Networking.md);

> Link's úteis:
>
> - [Vivendo & Aprendendo - Linux - Container Networking](/blog/linux/container-networking/)

### Kubernetes

- [Plugin do coreDNS para o Kubernetes](https://coredns.io/plugins/kubernetes/);
- [Especificação do serviço DNS para o Kubernetes](https://github.com/kubernetes/dns/blob/master/docs/specification.md);
- [Container Network Interface(CNI)](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/docs/09-Networking/07-Pre-requisite-CNI.md);

## Cluster Networking

A rede é uma parte central do Kubernetes, e pode ser um desafio entender exatamente como ela deve funcionar, existem 4 problemas de rede distintos para resolver:

- Comunicações de container a container altamente acopladas: isso é resolvido pelos Pods e comunicação `localhost`.
- Comunicações `Pod-to-Pod`.
- Comunicações `Pod-to-Service`.
- Comunicações externas ao serviço.

> Link's úteis:
>
> - [Doc K8S - Conceitos - Estendendo Kubernetes - Cluster Networking](https://kubernetes.io/docs/concepts/cluster-administration/networking/)
> - [Doc K8S - Conceitos - Services, Load Balancing, and Networking # O modelo de rede do Kubernetes](https://kubernetes.io/docs/concepts/services-networking/#the-kubernetes-network-model)
> - [Vivendo & Aprendendo - Kubernetes - Network](/blog/kubernetes/network/)

## Pod Networking

Cada Pod recebe um endereço IP exclusivo para cada família de endereços. Cada container do Pod compartilha o `network namespace`, incluindo o endereço IP e as portas de rede.

Dentro do Pod, os containers se comunicam entre si usando `localhost`.

Quando os contêineres em um Pod se comunicam com entidades fora do Pod, eles devem coordenar como usam os recursos de rede compartilhados (como portas).

> Link's úteis:
>
> - [Doc K8S - Conceitos - Cargas de trabalho - Pods # Networking](https://kubernetes.io/docs/concepts/workloads/pods/#pod-networking)
> - [Doc K8S - Conceitos - Serviços, balanceamento de carga e rede - DNS para Pods](https://kubernetes.io/docs/concepts/services-networking/dns-pod-service/#pods)

## Container Networking Interface (CNI) no Kubernetes

O Kubernetes oferece suporte a plugins *Container Network Interface* (CNI) para redes de cluster, escolha um que seja compatível com o cluster e que atenda às suas necessidades, diferentes plugins estão disponíveis (tanto de código aberto quanto de código fechado) no ecossistema Kubernetes.

Um plug-in CNI é necessário para implementar o [modelo de rede Kubernetes](https://kubernetes.io/docs/concepts/services-networking/#the-kubernetes-network-model).

- **IP Address Management in the Kubernetes Cluster**

{{< imgproc net3 Fit "880x470" >}} {{< /imgproc >}}
Fonte.: [Curso - Certified Kubernetes Administrator (CKA) with Practice Tests](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/docs/09-Networking/15-ipam-weave.md).

> Link's úteis:
>
> - [Doc K8S - Conceitos - Administração de cluster - Extensões de computação, armazenamento e rede - Network Plugins](https://kubernetes.io/docs/concepts/extend-kubernetes/compute-storage-net/network-plugins/)
> - [Doc K8S - Conceitos - Administração de cluster - Instalando Addons](https://kubernetes.io/docs/concepts/cluster-administration/addons/)
> - [Doc WeaveWorks - Integrating Kubernetes via the Addon](https://www.weave.works/docs/net/latest/kubernetes/kube-addon/)

## Service Networking

Uma maneira abstrata de expor um aplicativo em execução em um *conjunto de Pods* é através de um `Service` ou serviço de rede.

Com o Kubernetes, você não precisa modificar seu aplicativo para usar um mecanismo de descoberta de serviço desconhecido. O Kubernetes fornece aos Pods seus próprios endereços IP e *um único nome DNS* para um conjunto de pods e pode balancear a carga entre eles.

> Link's úteis:
>
> - [Doc K8S - Conceitos - Serviços, balanceamento de carga e rede - Services](https://kubernetes.io/docs/concepts/services-networking/service/)
> - [Doc K8S - Conceitos - Serviços, balanceamento de carga e rede - DNS para Services](https://kubernetes.io/docs/concepts/services-networking/dns-pod-service/#services)
> - [Doc K8S - Conceitos - Serviços, balanceamento de carga e rede - Conectando aplicativos com Services](https://kubernetes.io/docs/concepts/services-networking/connect-applications-service/)
> - [Doc K8S - Tarefas - Administrar um cluster - Debugging DNS Resolution](https://kubernetes.io/docs/tasks/administer-cluster/dns-debugging-resolution/)

## Ingress

O `Ingress` expõe rotas `HTTP/HTTPS` de fora do cluster para `Services` dentro do cluster, pode atuar como balanceador de carga, aplicar SSL/TLS e oferecer hospedagem virtual baseada em nome.

É necessário ter um `Ingress Controller` para satisfazer um Ingress. Apenas a criação de um `Ingress resource` não tem efeito.

> Link's úteis:
>
> - [Doc K8S - Conceitos - Serviços, balanceamento de carga e rede - Ingress](https://kubernetes.io/docs/concepts/services-networking/ingress/)
> - [Doc K8S - Conceitos - Serviços, balanceamento de carga e rede - Ingress Controller](https://kubernetes.io/docs/concepts/services-networking/ingress-controllers/)
> - [Doc K8S - Tarefas - Acessando aplicações no Cluster - Configurando um Ingress no Minikube com o NGINX Ingress Controller](https://kubernetes.io/docs/tasks/access-application-cluster/ingress-minikube/)
