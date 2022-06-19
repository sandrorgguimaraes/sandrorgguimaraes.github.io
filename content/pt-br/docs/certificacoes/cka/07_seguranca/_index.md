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

- Diferença entre criptografia simetrica e assimetrica
- O que é TLS
- Certificados
  - Processo de assinatura dos certificados (KEY -> CSR -> CRT)
  - Autoridade Certificadora (CA)
- PKI

## TLS no Kubernetes

https://github.com/kodekloudhub/certified-kubernetes-administrator-course/tree/master/docs/07-Security

- Toda comunicação interna do cluster, entre os nodes (master e/ou workers).
- Certificados dos Servers e Clientes
- Sequencia de geração dos certificados
  - CA do Kubernetes
    - Certificados Clientes
      - Admin User
      - Scheduler
      - Controller Manager
      - Kube Proxy
      - ApiServer-Kubelet-cliente
      - ApiServer-ETCD-cliente
      - Kubelet-cliente
    - Certificados Servidoes
      - ETCD
      - ApiServer
        - Vários CN
      - Kubelet
        - Um para cada Node

https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-certs/
https://kubernetes.io/docs/setup/best-practices/certificates/

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
