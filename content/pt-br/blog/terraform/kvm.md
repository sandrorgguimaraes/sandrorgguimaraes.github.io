---
title: KVM / libvirt
date: 2022-04-06
categories:
    - terraform
tags:
    - kvm
    - libvirt
autores:
    - Fabian Lee
    - Nitesh
    - Josphat Mutai
description: Como utilizar o provedor para libvirt / KVM do Terraform para gerenciar recursos (VM's, redes e discos) locais.
keywords:
    - kvm
    - libvirt
slug: kvm-libvirt
---

{{% pageinfo %}}
O **KVM** (***K**ernel-based **V**irtual **M**achine*) é um módulo de virtualização no kernel do Linux que permite que o kernel funcione como um [hypervisor](https://en.wikipedia.org/wiki/Hypervisor). Ele foi incorporado ao kernel Linux principal na versão 2.6.20, que foi lançada em 5 de fevereiro de 2007.

O **KVM** requer um processador com extensões de virtualização de hardware, como `Intel VT` ou `AMD-V`.

Fonte.: <https://en.wikipedia.org/wiki/Kernel-based_Virtual_Machine>
{{% /pageinfo %}}

{{% pageinfo %}}
`libvirt` é uma API de código aberto, daemon e ferramenta de gerenciamento para gerenciar a virtualização de plataforma.

Ele pode ser usado para gerenciar KVM, Xen, VMware ESXi, QEMU e outras tecnologias de virtualização. Essas APIs são amplamente utilizadas na camada de orquestração de hipervisores no desenvolvimento de uma solução baseada em nuvem.

Fonte.: <https://en.wikipedia.org/wiki/Libvirt>
{{% /pageinfo %}}

---

## [KVM: Terraform and cloud-init to create local KVM resources](https://fabianlee.org/2020/02/22/kvm-terraform-and-cloud-init-to-create-local-kvm-resources/)

por [**Fabian Lee**](/autores/fabian-lee/) em 22/02/2020.

O [Terraform](https://www.terraform.io/) é uma ferramenta popular para provisionar não apenas infraestrutura em provedores de nuvem, mas também localmente com o provedor para [libvirt](https://libvirt.org/) / KVM.

Usando o provedor libvirt, podemos usar construções padrão do Terraform para criar VMs, redes e discos locais.

---

## [How To Provision VMs on KVM with Terraform](https://computingforgeeks.com/how-to-provision-vms-on-kvm-with-terraform/)

por [**Josphat Mutai**](/autores/josphat-mutai/) em 16/11/2021.

Se você é fã do Terraform e do KVM, tenho certeza de que está procurando uma maneira de provisionar máquinas virtuais no KVM de maneira automatizada com o Terraform.

Neste artigo, e mostrado como instalar o provedor Terraform KVM e usá-lo para gerenciar instâncias em execução no hipervisor KVM.

---

## [How to use Terraform to create a small-scale Cloud Infrastructure](https://itnext.io/how-to-use-terraform-to-create-a-small-scale-cloud-infrastructure-abf54fabc9dd)

por [**Nitesh**](/autores/nitesh/) em 16/04/2021.

Este tutorial dá e descreve as instruções sobre como usar o software Terraform para criar vários tipos de infraestruturas Cloud, nomeadamente Infraestrutura como Serviço (IaaS), Plataforma como Serviço (PaaS) e Software como Serviço (SaaS) e utilizando-as como método para ajudar o leitor a aprender mais sobre o uso do Terraform e da tecnologia de nuvem em geral.

Este documento-tutorial fornecerá uma orientação passo a passo para qualquer pessoa que não tenha conhecimento sobre o Terraform e alguma tecnologia de nuvem, mas tenha interesse em aprender mais sobre isso em um ambiente de teste de pequena escala.

---
