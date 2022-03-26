---
title: Segurança
date: 2022-03-25
categories:
    - kubernetes
tags:
    - security
    - mtls
    - tls
    - linkerd
    - service mesh
autores:
    - Piotr
    - William Morgan
description: Entenda como funciona as segurança no Kubernetes.
keywords:
    - security
    - mtls
    - tls
    - linkerd
    - service mesh
slug: security
---

{{% pageinfo %}}
Autenticação mútua ou autenticação bidirecional (não confundir com autenticação de dois fatores - [2FA](https://pt.wikipedia.org/wiki/Identifica%C3%A7%C3%A3o_por_dois_fatores)) refere-se a duas partes que se autenticam ao mesmo tempo em um protocolo de autenticação. É um modo de autenticação padrão em alguns protocolos (IKE, SSH) e opcional em outros (TLS).

Fonte.: <https://en.wikipedia.org/wiki/Mutual_authentication/>
{{% /pageinfo %}}

---

## [Kubernetes Security & Hardening Guidance](https://itnext.io/kubernetes-security-hardening-guidance-cf5fc48a9b3e)

por [**Piotr**](/autores/piotr/) em 04/08/2021.

Neste artigo, veremos padrões na segurança nativa da nuvem e do Kubernetes. Esses padrões são baseados em duas publicações interessantes relacionadas à segurança, uma da *National Security Agency* (NSA) e outra da *Cloud Native Computing Foundation* (CNCF).

---

## [A Kubernetes engineer’s guide to mTLS](https://buoyant.io/mtls-guide/)

por [**William Morgan**](/autores/william-morgan/) da [Buoyant](https://buoyant.io/).

Segue alguns tópicos que serão abordados neste guia:

- O que é mTLS?
- Que tipo de segurança o TLS oferece?
- Quando o mTLS é útil?
- Usando mTLS para proteger microsserviços;
- A parte difícil do TLS: gerenciamento de certificados
- Kubernetes, mTLS e a malha de serviço

Além de um tutorial, claro! senão não teria graça.

---

## [cert-manager](https://github.com/cert-manager/cert-manager)

`cert-manager` é um complemento do Kubernetes para automatizar o gerenciamento e a emissão de certificados TLS de várias fontes de emissão.

Ele garantirá que os certificados sejam válidos e atualizados periodicamente e tentará renová-los em um momento apropriado antes da expiração.

---
