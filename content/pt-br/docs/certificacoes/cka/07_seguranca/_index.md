---
title: Segurança do Cluster
date: 2022-06-15
weight: 70
description: Entendendo como manter e gerenciar um cluster Kubernetes com segurança.
resources:
- src: "**.{png,jpg}"
  title: "Image #:counter"
---

Em linhas gerais o que veremos a seguir é a aplicação do modelo `4C's da segurança nativa da nuvem`, não necessariamente na ordem apresentada.

{{< imgproc 4c Fit "800x450" >}} {{< /imgproc >}}
Fonte.: [Doc K8S — Visão geral da segurança nativa da nuvem](https://kubernetes.io/docs/concepts/security/overview/#the-4c-s-of-cloud-native-security).

## A Segurança no Kubernetes

O [Kubernetes é formado por Nodes](../01_componentes/), portanto o primeiro passo é garantir a segurança dos mesmos, *desabilitando* a autenticação por senha e *habilitando* a autenticação por chave SSH;

Como a comunicação dentro de um cluster Kubernetes, os comandos, o gerenciamento, etc. passa pelo API Server, não podemos esquecer:

{{% pageinfo %}}
API Server Seguro == Kubernetes Seguro
{{% /pageinfo %}}

Para manter a segurança no acesso ao API Server, precisamos pensar em:

- Quem pode acessar (*Autenticação*) e;
- O que pode fazer (*Autorização*);

Para garantir a segurança nas comunicações entre os componentes do cluster é fundamental o *uso do TLS*.

E como a premissa do Kubernetes é que todos os Pods podem se comunicar entre si, temos as *Network Policies* para implementar um controle nestas comunicações.

> Link's úteis:
>
> - [Doc K8S - Conceitos - Segurança](https://kubernetes.io/docs/concepts/security/)

## Autenticação

> Link's úteis:
>
> - [Doc K8S - Conceitos - Segurança](https://kubernetes.io/docs/concepts/security/)

## Conceitos básicos sobre TLS

O [**TLS** (*Transport Layer Security*)](https://en.wikipedia.org/wiki/Transport_Layer_Security) é um protocolo de [criptografia](https://en.wikipedia.org/wiki/Cryptography) projetado para garantir a privacidade (confidencialidade), integridade e autenticidade na comunicação em uma rede de computadores através do uso de [certificados digitais](https://en.wikipedia.org/wiki/Public_key_certificate), ele é a base do [**HTTPS** (*Hypertext Transfer Protocol Secure*)](https://en.wikipedia.org/wiki/HTTPS).

> Link's úteis:
>
> - [Criptografia Simétrica e Assimétrica: Qual a diferença entre elas?](https://cryptoid.com.br/banco-de-noticias/29196criptografia-simetrica-e-assimetrica/)
> - [O que é SSL e TLS? E por que é hora de atualizar para TLS 1.3?](https://cryptoid.com.br/banco-de-noticias/o-que-e-ssl-tls-e-por-que-e-hora-de-atualizar-para-tls-1-3/)
> - [PKI - Public Key Infrastructure](https://en.wikipedia.org/wiki/Public_key_infrastructure)
> - [OpenSSL Project](https://www.openssl.org/docs/)

## TLS no Kubernetes

No Kubernetes para garantir a comunicação segura e confiável entre os componentes nos Worker Nodes, o [kubelet](http://localhost:1313/docs/certificacoes/cka/01_componentes/#kubelet) e o [kube-proxy](http://localhost:1313/docs/certificacoes/cka/01_componentes/#kube-proxy), e os componentes no [Control Plane](../01_componentes/#componentes-dos-master-nodes-ou-control-plane), principalmente o [kube-apiserver](http://localhost:1313/docs/certificacoes/cka/01_componentes/#api-server) é altamente recomendável usar certificados TLS.

{{< imgproc tls Fit "861x560" >}} {{< /imgproc >}}
Fonte.: [Curso - Certified Kubernetes Administrator (CKA) with Practice Tests](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/docs/07-Security/06-TLS-in-Kubernetes.md).

> Link's úteis:
>
> - [Doc K8S - Referência - Controle de acesso à API - Inicialização de TLS](https://kubernetes.io/docs/reference/access-authn-authz/kubelet-tls-bootstrapping/)
> - [Doc K8S - Começando - Melhores Práticas - Certificados e requisitos de PKI](https://kubernetes.io/docs/setup/best-practices/certificates/)
> - [Doc K8S - Tarefas - TLS - Gerenciar certificados TLS em um cluster](https://kubernetes.io/docs/tasks/tls/managing-tls-in-a-cluster/)
> - [Doc K8S - Tarefas - Administrar um cluster - Administração com kubeadm - Gerenciamento de certificados com kubeadm](https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-certs/)
> - [Curso - Certified Kubernetes Administrator (CKA) with Practice Tests - TLS in kubernetes - Certificate Creation](https://kubernetes.io/docs/setup/best-practices/certificates/)

## API de Certificados

https://kubernetes.io/docs/reference/access-authn-authz/certificate-signing-requests/#request-signing-process
https://kubernetes.io/docs/tasks/tls/managing-tls-in-a-cluster/

## KubeConfig

https://kubernetes.io/pt-br/docs/concepts/configuration/organize-cluster-access-kubeconfig/

## API de Grupos

https://kubernetes.io/docs/reference/using-api/
https://kubernetes.io/docs/concepts/overview/kubernetes-api/
https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.24/
https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/docs/07-Security/15-API-Groups.md

## Autorização

https://kubernetes.io/docs/reference/access-authn-authz/authorization/
https://github.com/kodekloudhub/certified-kubernetes-administrator-course/tree/master/docs/07-Security

## RBAC

https://kubernetes.io/docs/reference/access-authn-authz/rbac/
https://kubernetes.io/docs/reference/access-authn-authz/rbac/#command-line-utilities

## Cluster Roles e Role Bindings

https://kubernetes.io/docs/reference/access-authn-authz/rbac/#role-and-clusterrole
https://kubernetes.io/docs/reference/access-authn-authz/rbac/#command-line-utilities

## Services Accounts

https://kubernetes.io/docs/reference/access-authn-authz/service-accounts-admin/
https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/

## Segurança de Imagens

https://kubernetes.io/docs/concepts/containers/images/

## Segurança de Contexto

https://kubernetes.io/docs/tasks/configure-pod-container/security-context/

## Network Policy

https://kubernetes.io/docs/concepts/services-networking/network-policies/
https://kubernetes.io/docs/concepts/services-networking/network-policies/
