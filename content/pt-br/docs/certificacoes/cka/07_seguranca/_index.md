---
title: Segurança do Cluster
date: 2022-06-15
weight: 70
description: Entendendo como manter e gerenciar um cluster Kubernetes com segurança.
---

## A Segurança no Kubernetes

- Manter os nodes seguros
- Manter o API server seguro == Manter o Kubernetes seguro
  - 1ª linha de defesa

- Quem pode acessar (Autenticação)
- O que pode fazer (Autorização)

- Uso do TLS nas comunicações

- Todos os Pods se comunicam entre si;
  - Network Policy

## Autenticação

- Basicamente temos 2 tipos de acessos para gerenciar
  - Pessoas (usuários pessoais, com contas pessoais, sejam administradores ou desenvolvedores
  - Bots (usuários impessoais, com contas de serviço), outras aplicações que interagem com o k8s

- A autenticação é gerenciada pelo API Server

- Mecanismos de autenticação
  - Arquivo de senhas estatico
  - Arquivo de tokens estatico

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

## API de Grupos

## Autorização

## RBAC

## Cluster Roles e Role Bindings

## Services Accounts

## Segurança de Imagens

## Segurança de Contexto

## Network Policy
