---
title: Log & Monitoração
date: 2022-05-25
weight: 40
description: Entendendo como consultar o log e monitorar as aplicações nos cluster Kubernetes.
---

## Monitorar os componentes do cluster

Para Kubernetes, a [API Metrics](https://kubernetes.io/docs/tasks/debug/debug-cluster/resource-metrics-pipeline/#metrics-api) oferece um conjunto básico de métricas para dar suporte ao dimensionamento automático e casos de uso semelhantes.

Essa API disponibiliza informações sobre o uso de recursos para nó e pod, incluindo métricas para CPU e memória.

Se você implantar a API de métricas em seu cluster, os clientes da API do Kubernetes poderão consultar essas informações e você poderá usar os mecanismos de controle de acesso do Kubernetes para gerenciar permissões para fazer isso.

> Link's úteis:
>
> - [Documentação do Kubernetes - Tarefas - Monitoramento, registro e depuração - Solução de problemas de clusters - Pipeline de métricas de recursos](https://kubernetes.io/docs/tasks/debug/debug-cluster/resource-metrics-pipeline/)
> - [Documentação do Kubernetes - Tarefas - Monitoramento, registro e depuração - Solução de problemas de clusters - Ferramentas para monitorar recursos](https://kubernetes.io/docs/tasks/debug/debug-cluster/resource-usage-monitoring/)
> - [Documentação do Kubernetes - Tarefas - Administrar um cluster - Gerenciar recursos de memória, CPU e API](https://kubernetes.io/docs/tasks/administer-cluster/manage-resources/)

## Acompanhar os logs das aplicações
