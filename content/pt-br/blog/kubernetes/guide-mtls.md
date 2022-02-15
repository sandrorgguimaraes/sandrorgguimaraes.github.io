---
title: Um guia para mTLS
date: 2022-02-14T00:00:00.000Z
categories:
    - kubernetes
tags:
    - mtls
    - tls
    - linkerd
    - service mesh
autores:
    - William Morgan
description: Guia sobre mTLS (ou TLS mútuo) apresentando seus prós e contras e como implantar em um cluster kubernetes.
keywords:
    - mtls
    - linkerd
slug: kubernetes-engineer’s-guide-mtls
---

{{% pageinfo %}}
Autenticação mútua ou autenticação bidirecional (não confundir com autenticação de dois fatores - 2FA) refere-se a duas partes que se autenticam ao mesmo tempo em um protocolo de autenticação. É um modo de autenticação padrão em alguns protocolos (IKE, SSH) e opcional em outros (TLS).

Fonte.: <https://en.wikipedia.org/wiki/Mutual_authentication/>
{{% /pageinfo %}}

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
