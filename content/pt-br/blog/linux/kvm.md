---
title: KVM / libvirt
date: 2022-04-06
categories:
    - sistema operacional
    - linux
tags:
    - kvm
    - libvirt
autores:
    - Fabian Lee
    - Josphat Mutai
description: Veja algumas ferramentas para monitorar um sistema Linux.
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

## [KVM: Bare metal virtualization on Ubuntu with KVM](https://fabianlee.org/2018/08/27/kvm-bare-metal-virtualization-on-ubuntu-with-kvm/)

por [**Fabian Lee**](/autores/fabian-lee/) em 27/08/2018.

**KVM** é um hipervisor tipo 1 implementado como um módulo de kernel Linux que utiliza extensões de virtualização de um processador moderno, tornando-o capaz de execução direta da CPU sem tradução.

Cada máquina virtual é um processo Linux regular, agendado pelo agendador Linux padrão.

Um exemplo de algo que o **KVM** pode fazer que o *VirtualBox não pode*, o **KVM** consegue passar a capacidade de virtualização para seu sistema operacional convidado, o que permitiria a *virtualização aninhada*.

---

## [Install KVM Hypervisor on Ubuntu 22.04|20.04](https://computingforgeeks.com/install-kvm-hypervisor-on-ubuntu-linux/)

por [**Josphat Mutai**](/autores/josphat-mutai/) em 16/11/2021.

---
