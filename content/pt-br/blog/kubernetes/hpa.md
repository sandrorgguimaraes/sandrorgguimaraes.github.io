---
title: Horizontal Pod Autoscaler
date: 2022-05-05
weight: 70
categories:
    - kubernetes
tags:
    - hpa
    - horizontal pod autoscaler
autores:
    - Kubernetes SIGs
description: Vamos ver alguns materiais sobre Horizontal Pod Autoscaler.
keywords:
    - hpa
    - horizontal pod autoscaler
slug: hpa
---

---

## [Prometheus Adapter for Kubernetes Metrics APIs](https://github.com/kubernetes-sigs/prometheus-adapter)

por [**Kubernetes SIGs**](/autores/kubernetes-sigs/) 04/2022.

Este é um projeto do grupo `Kubernetes Special Interest Groups - Prometheus Adapter` e implementa  métricas de recursos do Kubernetes, métricas personalizadas e APIs de métricas externas .

Este adaptador é, portanto, adequado para uso com o autoscaling/v2 Horizontal Pod Autoscaler no Kubernetes 1.6+.

Ele também pode substituir o [servidor de métricas](https://github.com/kubernetes-incubator/metrics-server) em clusters que já executam o Prometheus e coletar as métricas apropriadas.

---
