---
title: Template de VM's
date: 2022-04-20
weight: 10
categories:
    - fazendo e aprendendo
tags:
    - terraform
    - kvm
    - libvirt
    - qemu
    - template maquina virtual
autores:
    - Sandro Rogério
description: Vamos instalar o KVM e suas dependências em uma máquina Ubuntu, para gerar templates que serão utilizados na criação das máquinas virtuais.
keywords:
    - terraform
    - kvm
    - libvirt
    - qemu
    - template maquina virtual
slug: template-vm
resources:
- src: "**.{png,jpg}"
  title: "Image #:counter"
---

> ***Observações / informações iniciais:***
>
> - Os procedimentos abaixo foram aplicados em um computador com o `Ubuntu 20.04.4 LTS`.
> - Utilizamos como fonte de estudo os artigos listados na seção [Vivendo e Aprendendo -> Linux -> KVM / libvirt](/blog/linux/kvm-libvirt/).

## Entendendo os conceitos

A [virtualização de hardware](https://en.wikipedia.org/wiki/Hardware_virtualization) atualmente é uma tecnologia estável e madura, não sendo o objetivo deste material explorar todas as suas nuances e caracteristicas (quem sabe mais pra frente).

O objetivo aqui é demonstrar como criar os templates (modelos) de máquinas virtuais, que serão utilizados posteriormente.

Primeiro vamos conhecer os principais componentes que permitirão que isso aconteça.

{{< alert title="KVM" >}}
[***K**ernel-based **V**irtual **M**achine*](https://www.linux-kvm.org/page/Main_Page) é uma solução de virtualização completa para Linux em hardware x86 contendo extensões de virtualização ([Intel VT](https://en.wikipedia.org/wiki/X86_virtualization#Intel_virtualization_(VT-x)) ou [AMD-V](https://en.wikipedia.org/wiki/X86_virtualization#AMD_virtualization_(AMD-V))). Ele consiste em um módulo de kernel carregável, `kvm.ko`, que fornece a infraestrutura central de virtualização e um módulo específico do processador, `kvm-intel.ko` ou `kvm-amd.ko`.

Usando o KVM, é possível executar várias máquinas virtuais executando imagens não modificadas do Linux ou do Windows. Cada máquina virtual possui hardware virtualizado privado: uma placa de rede, disco, adaptador gráfico, etc.
{{< /alert >}}

{{< alert title="libvirt" >}}
O projeto [libvirt](https://libvirt.org/) é um kit de ferramentas para gerenciar [plataformas de virtualização](https://libvirt.org/platforms.html), é acessível a partir de C, Python, Perl, Go e muito mais, está licenciado sob licenças de código aberto, suporta [KVM](https://libvirt.org/drvqemu.html), [Hypervisor.framework](https://libvirt.org/drvqemu.html), [QEMU](https://libvirt.org/drvqemu.html), [Xen](https://libvirt.org/drvxen.html), [Virtuozzo](https://libvirt.org/drvvirtuozzo.html), [VMWare ESX](https://libvirt.org/drvesx.html), [LXC](https://libvirt.org/drvlxc.html), [BHyve](https://libvirt.org/drvbhyve.html) e [mais](https://libvirt.org/drivers.html), tem como alvo Linux, FreeBSD, [Windows](https://libvirt.org/windows.html) e [MacOS](https://libvirt.org/macos.html) e é usado por [muitas aplicações](https://libvirt.org/apps.html).
{{< /alert >}}

{{< alert title="QEMU" >}}
[QEMU](https://www.qemu.org/) é um emulador e virtualizador de máquina genérico e de código aberto.

O QEMU pode ser usado de várias maneiras diferentes. O mais comum é para “emulação de sistema”, onde fornece um modelo virtual de uma máquina inteira (CPU, memória e dispositivos emulados) para executar um sistema operacional convidado. Neste modo, a CPU pode ser totalmente emulada ou pode funcionar com um hypervisor como KVM, Xen, Hax ou Hypervisor.Framework para permitir que o convidado seja executado diretamente na CPU do host.

A segunda maneira suportada de usar o QEMU é a “emulação de modo de usuário”, onde o QEMU pode iniciar processos compilados para uma CPU em outra CPU. Neste modo a CPU é sempre emulada.
{{< /alert >}}

A imagem abaixo ilustra de forma simples a relação entre o hardware físico (CPU, memória, discos, placa de rede, etc.) o Hypervisor e as máquinas virtuais.

{{< imgproc exemplo-vm Fill "601x355" >}}
Fonte: www.revistaespacios.com/a16v37n27/16372721.html
{{< /imgproc >}}

## Instalando os componentes

Como todos os componentes (KVM, libvirt e QEMU) estam disponíveis nos repositórios upstream do Ubuntu, usaremos o gerenciador de pacotes `apt` para a instalação dos mesmos, para outras distribuições Linux ou sistemas operacionais verifique a documentação correspondente do [KVM](https://www.linux-kvm.org/page/Downloads), [libvirt](https://libvirt.org/docs.html) e [QEMU](https://www.qemu.org/download/).

```bash
# Atualiza a lista de pacotes disponíveis
sudo apt update

# Instalação dos pacotes principais do KVM / libvirt / QEMU
sudo apt -y install \
  qemu-kvm \
  libvirt-daemon \
  bridge-utils \
  virtinst \
  libvirt-daemon-system

# Instalação de pacotes adicionais
sudo apt -y install \
  virt-top \
  libguestfs-tools \
  libosinfo-bin \
  qemu-system \
  virt-manager
```

### Validando / concluindo a instalação

#### Testando o KVM

Para validar se o KVM está instalado corretamente e se a CPU está com a [virtualização x86](https://en.wikipedia.org/wiki/X86_virtualization) habilitada, utilize o utilitário [kvm-ok](http://manpages.ubuntu.com/manpages/jammy/en/man1/kvm-ok.1.html).

```bash
sudo kvm-ok

INFO: /dev/kvm exists
KVM acceleration can be used
```

{{< alert title="Extraído da documentação:" >}}
Em alguns casos de falha, o **kvm-ok** fornece dicas sobre como você pode habilitar o KVM em um sistema onde ele é arbitrariamente desabilitado.
{{< /alert >}}

#### Checando o modulo `vhost_net`

O módulo `vhost_net` fornecerá ferramentas semelhantes aos comandos `ls`, `cat`, `top` para uso com máquinas virtuais, para certificar que ele está carregado e habilitado, use o código abaixo:

```bash
# Verifica se o modulo está carregado, senão carrega o modulo do Kernel
# e já adiciona no arquivo '/etc/modules' para ser carregado na inicialização
if ! lsmod | grep vhost; then
  sudo modprobe vhost_net;
  echo vhost_net | sudo tee -a /etc/modules;
fi
```

### Consultando o `libvirt`

Para verificar se o serviço de virtualização está devidamente ativo execute o comando `sudo systemctl status libvirtd`, o resultado deve ser.

```bash
● libvirtd.service - Virtualization daemon
     Loaded: loaded (/lib/systemd/system/libvirtd.service; enabled; vendor preset: enabled)
     Active: active (running) since Tue 2022-04-19 08:15:26 -03; 3s ago
TriggeredBy: ● libvirtd.socket
             ● libvirtd-ro.socket
             ● libvirtd-admin.socket
       Docs: man:libvirtd(8)
             https://libvirt.org
   Main PID: 203390 (libvirtd)
      Tasks: 19 (limit: 32768)
     Memory: 35.6M
     CGroup: /system.slice/libvirtd.service
             ├─  1382 /usr/sbin/dnsmasq --conf-file=/var/lib/libvirt/dnsmasq/default.conf --leasefile-ro --dhcp-script=/usr/lib/libvirt/libvirt_leaseshelper
             ├─  1383 /usr/sbin/dnsmasq --conf-file=/var/lib/libvirt/dnsmasq/default.conf --leasefile-ro --dhcp-script=/usr/lib/libvirt/libvirt_leaseshelper
             └─203390 /usr/sbin/libvirtd
```

Se o status for diferente de `running` execute os comandos abaixo para iniciar o serviço agora e habilitar a inicialização durante o boot.

```bash
sudo systemctl enable libvirtd
sudo systemctl start libvirtd
```

Se tudo correu bem, também foi criada a interface de rede *virtual* `virbr0` vinculada a um switch *virtual* pré configurado no modo [NAT](https://en.wikipedia.org/wiki/Network_address_translation) e com o programa [dnsmasq](https://en.wikipedia.org/wiki/Dnsmasq) habilitado, conforme diagrama abaixo.

Maiores detalhes sobre a configuração padrão e dos outros modos disponíveis podem ser obtidos na [documentação sobre VirtualNetworking do libvirt](https://wiki.libvirt.org/page/VirtualNetworking).

{{< imgproc libvirt_virtual_network Fill "702x513" >}}
Fonte: wiki.libvirt.org/page/File:Virtual_network_default_network_overview.jpg
{{< /imgproc >}}

```bash
# Consultando as interfaces de rede
ip add | grep virbr

4: virbr0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN group default qlen 1000
    inet 192.168.122.1/24 brd 192.168.122.255 scope global virbr0
5: virbr0-nic: <BROADCAST,MULTICAST> mtu 1500 qdisc fq_codel master virbr0 state DOWN group default qlen 1000
```

## Criando templates de máquinas virtuais

**O que é um template de máquina virtual?** É uma máquina virtual pré-instalada e preparada para servir de base na criação 'em série' de outras máquinas virtuais, dessa forma os procedimentos normalmente demorados de baixa / instalação das aplicações é feito apenas uma vez, gerando um ganho de tempo e consequentemente de produtividade, além de garantir a padronização do que está pré instalado na máquina virtual.

Os templates são essenciais em processos automatizados, como parte de uma abordagem de [**IaC** (*Infrastructure as Code*)](https://en.wikipedia.org/wiki/Infrastructure_as_code) ou de [**VDI** (*Virtual Desktop Infrastructure*)](https://en.wikipedia.org/wiki/Desktop_virtualization).

### Escolhendo a imagem base

- Necessita de uma interface gráfica? ou a interface texto é o suficiente?
- Necessita das últimas versões das aplicações, drivers, etc.? ou...
- Precisa de versões mais antigas, e teoricamente, mais estáveis das aplicações, drivers, etc.?
- Existe alguma exigência técnica para o uso de um determinado sistema operacional / distribuição?
- Qual distribuição ou sistema operacional você tem mais familiaridade?

Essas são algumas perguntas que devem surgir no momento de decidir qual sistema operacional / distribuição (Linux) utilizar, independente das respostas tenha em mente estes 2 pontos, dê preferência a imagens `net-installer`, `minimal` ou `boot-iso` e ***instale apenas o necessário***, com isso o tamanho final do template será menor e reduzirá a cobertura de ataques cibernéticos e bugs (por ter menos aplicações instaladas).

Abaixo segue os link's para as páginas de download de algumas distribuições / sistemas operacionais (em ordem alfabética), fique a vontade para sugerir novas usando os link's ao lado na seção `Colabore`, obrigado!

[CentOS](https://www.centos.org/download/) | [Debian](https://www.debian.org/distrib/index.pt.html) | [OpenSuse](https://get.opensuse.org/leapmicro/5.2/) | [Ubuntu](https://ubuntu.com/download/server/) | [Windows 10](https://www.microsoft.com/pt-br/software-download/windows10ISO)

> Para os passos seguintes optamos por utilizar o [CentOS-8-Stream](http://mirror.ci.ifes.edu.br/centos/8-stream/isos/x86_64/CentOS-Stream-8-x86_64-20220414-boot.iso).

### Preparando para a instalação

O primeiro passo é criar uma imagem de disco que servirá para a instalação do sistema operacional, para tanto utilizaremos o utilitário [qemu-img create](https://qemu.readthedocs.io/en/latest/tools/qemu-img.html#), que tem as seguintes opções.

```bash
qemu-img create \
  [--object OBJECTDEF] \
  [-q] \
  [-f FMT] \
  [-b BACKING_FILE [-F BACKING_FMT]] \
  [-u] \
  [-o OPTIONS] \
  FILENAME \
  [SIZE]
```

Maiores detalhes sobre cada uma das opções acima consulte a [documentação do comando create](https://qemu.readthedocs.io/en/latest/tools/qemu-img.html#cmdoption-qemu-img-commands-arg-create).

Vamos executar o comando abaixo para criar a nossa imagem de disco.

```bash
sudo qemu-img create \
  -f qcow2 \
  -o preallocation=metadata \
  /var/lib/libvirt/images/centos8.qcow2 \
  15G
```

Para gerenciarmos nossas máquinas virtuais temos 2 opções, ambas criadas com base na biblioteca `libvirt`:

- [virsh](https://libvirt.org/manpages/virsh.html) (linha de comando);
- [virt-manager](https://virt-manager.org/) (interface gráfica);

Adicionalmente ainda temos à disposição as seguintes ferramentas de suporte, também criadas usando a `libvirt`.

- [virt-install](https://linux.die.net/man/1/virt-install) para provisionar novas máquinas virtuais.
- [virt-viewer](https://linux.die.net/man/1/virt-viewer) exibe o console gráfico para uma máquina virtual.
- [virt-clone](https://linux.die.net/man/1/virt-clone) para clonar imagens de máquinas virtuais existentes.
- [virt-xml](https://www.systutorials.com/docs/linux/man/1-virt-xml/) para editar o XML do domínio libvirt.
- [virt-bootstrap](https://github.com/virt-manager/virt-bootstrap) configura o sistema de arquivos raiz para contêineres baseados em libvirt.

{{< alert color="warning" title="Aguarde" >}}Página em construção...{{< /alert >}}

### Instalando o sistema operacional

<!-- ```bash
sudo virt-install \
  --virt-type kvm \
  --name centos8 \
  --memory 2048 \
  --disk /var/lib/libvirt/images/centos8.qcow2,format=qcow2 \
  --network network=default \
  --graphics vnc,listen=0.0.0.0 \
  --noautoconsole \
  --os-variant=centos-stream8 \
  --location=/home/sandro/Downloads/CentOS-Stream-8-x86_64-20220414-boot.iso
``` -->

### Limpando a imagem
