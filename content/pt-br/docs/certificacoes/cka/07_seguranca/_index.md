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

A API de certificados permite a automação do provisionamento de credenciais X.509 fornecendo uma interface programática para clientes da API Kubernetes solicitarem e obterem certificados X.509 de uma Autoridade de Certificação (CA).

Um recurso `CertificateSigningRequest` (CSR) é usado para solicitar que um certificado seja assinado por um assinante indicado, após o qual a solicitação pode ser aprovada ou negada antes de ser finalmente assinada.

> Link's úteis:
>
> - [Doc K8S - Referência - Controle de acesso à API - Solicitações de assinatura de certificado](https://kubernetes.io/docs/reference/access-authn-authz/certificate-signing-requests/)
> - [Doc K8S - Tarefas - TLS - Gerenciar certificados TLS em um cluster](https://kubernetes.io/docs/tasks/tls/managing-tls-in-a-cluster/)

## KubeConfig

Utilize arquivos `kubeconfig` para organizar informações sobre clusters, usuários, namespaces e mecanismos de autenticação. A ferramenta de linha de comando `kubectl` faz uso dos arquivos `kubeconfig` para encontrar as informações necessárias para escolher e se comunicar com o serviço de API de um cluster.

Por padrão, o `kubectl` procura por um arquivo de nome `config` no diretório `$HOME/.kube`. Você pode especificar outros arquivos kubeconfig através da variável de ambiente `KUBECONFIG` ou adicionando a opção `--kubeconfig`.

> Link's úteis:
>
> - [Doc K8S - Conceitos - Configuração - Organizando o acesso ao cluster usando arquivos kubeconfig](https://kubernetes.io/pt-br/docs/concepts/configuration/organize-cluster-access-kubeconfig/)

## Grupos de API

Os grupos de API facilitam a extensão da API do Kubernetes. O grupo de APIs é especificado em um caminho `REST` e no campo `apiVersion` de um objeto serializado.

Existem vários grupos de API no Kubernetes:

- O grupo principal ou `core` (também chamado de legado) é encontrado no caminho REST `/api/v1`. O grupo principal não é especificado como parte do campo `apiVersion`, por exemplo, `apiVersion: v1`.

{{< imgproc api5 Fit "861x560" >}} {{< /imgproc >}}
Fonte.: [Curso - Certified Kubernetes Administrator (CKA) with Practice Tests](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/docs/07-Security/15-API-Groups.md).

- Os grupos nomeados estão no caminho REST `/apis/$GROUP_NAME/$VERSION` e usam `apiVersion: $GROUP_NAME/$VERSION` (por exemplo, `apiVersion: batch/v1`). Podemos encontrar a lista completa de grupos de API compatíveis na [referência da API do Kubernetes](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.24/#-strong-api-groups-strong-).

{{< imgproc api6 Fit "861x560" >}} {{< /imgproc >}}
Fonte.: [Curso - Certified Kubernetes Administrator (CKA) with Practice Tests](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/docs/07-Security/15-API-Groups.md).

> Link's úteis:
>
> - [Doc K8S - Conceitos - Visão geral - A API do Kubernetes](https://kubernetes.io/docs/concepts/overview/kubernetes-api/)
> - [Doc K8S - Referência - Configuração - Visão geral da API](https://kubernetes.io/docs/reference/using-api/)
> - [Doc K8S - Referência - API do Kubernetes](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.24/)

## Autorização

O Kubernetes autoriza solicitações de API usando o API Server. Ele avalia todos os atributos de solicitação em relação a todas as políticas e permite ou nega a solicitação. Todas as partes de uma solicitação de API devem ser permitidas por alguma política para prosseguir. *Isso significa que as permissões são negadas por padrão*.

Quando vários módulos de autorização são configurados, cada um é verificado em sequência. Se algum autorizador aprovar ou negar um pedido, *essa decisão é imediatamente devolvida* e *nenhum outro autorizador é consultado*. Se todos os módulos não tiverem opinião sobre a solicitação, *a solicitação será negada*. Uma negação retorna um código de status HTTP 403.

> Link's úteis:
>
> - [Doc K8S - Referência - Controle de acesso à API - Visão geral da autorização](https://kubernetes.io/docs/reference/access-authn-authz/authorization/)

## RBAC (Role-Based Access Control)

O **RBAC** (*Role-Based Access Control* ou Controle de Acesso Baseado em Função) é um método de regular o acesso a recursos de computador ou rede com base nas funções de usuários individuais em sua organização.

A autorização **RBAC** usa o `rbac.authorization.k8s.io` [Grupo de API](#grupos-de-api) para orientar as decisões de autorização, permitindo configurar políticas dinamicamente por meio da API do Kubernetes.

Para habilitar o **RBAC**, inicie o API Server com o sinalizador `--authorization-modes` definido para uma lista separada por vírgulas que inclua `RBAC`, por exemplo:

```bash
kube-apiserver --authorization-mode=Example,RBAC --other-options --more-options
```

> Link's úteis:
>
> - [Doc K8S - Referência - Controle de acesso à API - Usando autorização RBAC](https://kubernetes.io/docs/reference/access-authn-authz/rbac/)
> - [Doc K8S - Referência - Controle de acesso à API - Usando autorização RBAC # Utilitários de linha de comando](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#command-line-utilities)

## Cluster Roles e Role Bindings

Uma `Role` ou `ClusterRole` [RBAC](#rbac-role-based-access-control) contém regras que representam um conjunto de permissões. *As permissões são puramente aditivas* (não há regras de "negação").

- Uma `Role` sempre define permissões em um determinado `namespace`; ao criar uma `Role`, precisa ser especificado o `namespace` ao qual ela pertence.
- `ClusterRole`, por outro lado, é um recurso *sem namespace*, se aplica ao cluster inteiro.

Os recursos têm nomes diferentes (`Role` e `ClusterRole`) porque um objeto Kubernetes sempre precisa ter namespace ou não; não pode ser os dois.

> Link's úteis:
>
> - [Doc K8S - Referência - Controle de acesso à API - Usando autorização RBAC # Role e ClusterRole](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#role-and-clusterrole)
> - [Doc K8S - Referência - Controle de acesso à API - Usando autorização RBAC # Utilitários de linha de comando](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#command-line-utilities)

## Services Accounts

Segue abaixo algumas caracteristicas das *Services Accounts* ou Contas de Serviço:

- São para processos, que são executados em pods.
- São vinculadas a um namespace.
- Processo de criação mais leve, permitindo que os usuários do cluster criem contas de serviço para tarefas específicas seguindo o princípio de privilégio mínimo.

> Link's úteis:
>
> - [Doc K8S - Referência - Controle de acesso à API - Como gerenciar Services Accounts](https://kubernetes.io/docs/reference/access-authn-authz/service-accounts-admin/)
> - [Doc K8S - Tarefas - Configurar pods e contêineres - Configurar contas de serviço para pods](https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/)

## Segurança de Imagens

https://kubernetes.io/docs/concepts/containers/images/

## Segurança de Contexto

https://kubernetes.io/docs/tasks/configure-pod-container/security-context/

## Network Policy

https://kubernetes.io/docs/concepts/services-networking/network-policies/
https://kubernetes.io/docs/concepts/services-networking/network-policies/

<!-- 
curl --cacert ${CACERT} --header "Authorization: Bearer ${TOKEN}" -X GET ${APISERVER}/api/v1/nodes/

curl http://localhost:8080/api/v1/namespaces/default/pods -->
