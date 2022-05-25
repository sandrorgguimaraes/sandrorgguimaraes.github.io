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
description: Vamos instalar o KVM e suas dependências em uma máquina Ubuntu para gerar templates que serão utilizados na criação das máquinas virtuais.
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
> - Utilizamos como fonte de estudo os artigos listados na seção [Vivendo & Aprendendo -> Linux -> KVM / libvirt](/blog/linux/kvm-libvirt/).

## Entendendo os conceitos

A [virtualização de hardware](https://en.wikipedia.org/wiki/Hardware_virtualization) atualmente é uma tecnologia estável e madura, não sendo o objetivo deste material explorar todas as suas nuances e caracteristicas (quem sabe mais pra frente).

O objetivo aqui é demonstrar como criar os templates (modelos) de máquinas virtuais, que serão utilizados posteriormente.

Primeiro vamos conhecer os principais componentes que permitirão que isso aconteça.

{{< alert title="KVM" >}}
[***K**ernel-based **V**irtual **M**achine*](https://www.linux-kvm.org/page/Main_Page) é uma solução de virtualização completa para Linux em hardware x86 contendo extensões de virtualização ([Intel VT](https://en.wikipedia.org/wiki/X86_virtualization#Intel_virtualization_(VT-x)) ou [AMD-V](https://en.wikipedia.org/wiki/X86_virtualization#AMD_virtualization_(AMD-V))). Ele consiste em um módulo de kernel carregável o `kvm.ko` que fornece a infraestrutura central de virtualização e um módulo específico do processador `kvm-intel.ko` ou `kvm-amd.ko`.

Usando o KVM, é possível executar várias máquinas virtuais executando imagens não modificadas do Linux ou do Windows. Cada máquina virtual possui hardware virtualizado privado: uma placa de rede, disco, adaptador gráfico, etc.
{{< /alert >}}

{{< alert title="libvirt" >}}
O projeto [libvirt](https://libvirt.org/) é um kit de ferramentas para gerenciar [plataformas de virtualização](https://libvirt.org/platforms.html) é acessível a partir de C, Python, Perl, Go e muito mais, está licenciado sob licenças de código aberto, suporta [KVM](https://libvirt.org/drvqemu.html), [Hypervisor.framework](https://libvirt.org/drvqemu.html), [QEMU](https://libvirt.org/drvqemu.html), [Xen](https://libvirt.org/drvxen.html), [Virtuozzo](https://libvirt.org/drvvirtuozzo.html), [VMWare ESX](https://libvirt.org/drvesx.html), [LXC](https://libvirt.org/drvlxc.html), [BHyve](https://libvirt.org/drvbhyve.html) e [mais](https://libvirt.org/drivers.html), tem como alvo Linux, FreeBSD, [Windows](https://libvirt.org/windows.html) e [MacOS](https://libvirt.org/macos.html) e é usado por [muitas aplicações](https://libvirt.org/apps.html).
{{< /alert >}}

{{< alert title="QEMU" >}}
[QEMU](https://www.qemu.org/) é um emulador e virtualizador de máquina genérico e de código aberto.

O QEMU pode ser usado de várias maneiras diferentes. O mais comum é para “emulação de sistema”, onde fornece um modelo virtual de uma máquina inteira (CPU, memória e dispositivos emulados) para executar um sistema operacional convidado. Neste modo, a CPU pode ser totalmente emulada ou pode funcionar com um hypervisor como [KVM]((https://www.linux-kvm.org/page/Main_Page)), [Xen](https://xenproject.org/), Hax ou Hypervisor.Framework para permitir que o convidado seja executado diretamente na CPU do host.

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

Para verificar se o serviço de virtualização está devidamente ativo execute o comando `sudo systemctl status libvirtd`, o resultado deve parecer com a saída abaixo.

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

Se o status for diferente de `running` execute os comandos abaixo para habilitar a inicialização durante o boot e iniciar o serviço agora, depois repita o teste acima.

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

> Para os passos seguintes optamos por utilizar a versão mais recente do [CentOS-Stream-8-x86_64](http://mirror.ci.ifes.edu.br/centos/8-stream/isos/x86_64/CentOS-Stream-8-x86_64-latest-boot.iso).

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

Para maiores detalhes sobre cada uma das opções acima consulte a [documentação da opção create](https://qemu.readthedocs.io/en/latest/tools/qemu-img.html#cmdoption-qemu-img-commands-arg-create).

Vamos executar o comando abaixo para criar a nossa imagem de disco no formato [qcow2](https://qemu.readthedocs.io/en/latest/system/images.html#cmdoption-image-formats-arg-qcow2) usando a [pré-alocação dos metadados](https://qemu.readthedocs.io/en/latest/system/images.html#cmdoption-qcow2-arg-preallocation), gravando-a na pasta `/var/lib/libvirt/images/` (altere o nome da imagem para melhor identifica-la depois) e por fim especifique um tamanho para a mesma.

```bash
sudo qemu-img create \
  -f qcow2 \
  -o preallocation=metadata \
  /var/lib/libvirt/images/centos8-slim.qcow2 \
  15G
```

A saída desse comando será algo parecido com o trecho abaixo.

```bash
Formatting '/var/lib/libvirt/images/centos8-slim.qcow2', fmt=qcow2 size=16106127360 cluster_size=65536
preallocation=metadata lazy_refcounts=off refcount_bits=16
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

### Instalando o sistema operacional

#### Revisando / conhecendo as opções do comando `virt-install`

Se já tem algum tempo da publicação deste material ou se você gosta de saber com o que esta lidando, é recomendável consultar a documentação com o comando `man virt-install`, para verificar se as opções permanecem as mesmas, se não foram substituídas, se existem novas opções, etc.

Uma vez carregada a documentação algumas dicas podem nos poupar tempo para localizarmos o que precisamos em conteúdos extensos como este do `virt-install`.

- Para localizar o texto '--virt-type' digite `/--virt-type<<ENTER>>`.
- Para ir para a próxima ocorrência do texto use `n` e para voltar para as ocorrências anteriores `N`.

#### Iniciando a instalação

Uma vez terminada a consulta / validação das opções abaixo, vamos executar o comando e iniciar a instalação.

```bash
sudo virt-install \
  --virt-type kvm \
  --name centos8 \
  --memory 2048 \
  --disk /var/lib/libvirt/images/centos8-slim.qcow2,format=qcow2 \
  --network network=default \
  --graphics vnc,listen=0.0.0.0 \
  --noautoconsole \
  --os-variant=centos-stream8 \
  --cdrom=/home/sandro/Downloads/CentOS-Stream-8-x86_64-latest-boot.iso
```

Após o comando acima apareceu uma mensagem informando que a instalação ainda está em progresso, vamos confirmar se temos alguma máquina virtual em execução no momento?

```bash
virsh list

 Id   Name      State
-------------------------
 2    centos8   running
```

Show! Então agora precisamos acessar a console dessa máquina virtual e concluir a instalação do nosso sistema operacional, para tanto execute o comando abaixo.

```bash
virt-viewer centos8
```

Deverá aparecer a tela abaixo, de boas vindas à instalação do CentOS Stream 8, onde a primeira coisa é escolher o idioma a ser utilizado no processo de instalação.

{{< imgproc welcome-centos-stream-8 Fill "702x513" >}} {{< /imgproc >}}

A partir deste ponto, **cada caso é um caso** e, dependendo da função das máquinas virtuais a serem criadas com este template, iremos instalar um determinado conjunto de pacotes / programas. Fora alguns casos especificos, o mais indicado é **instalar apenas o necessário**, por exemplo:

- Se a função da máquina virtual for ser um servidor de aplicação, onde iremos acessar basicamente via terminal, realmente é necessário ter uma interface gráfica?

Lembre-se! Quanto menos programas tivermos instalados em nosso sistema, menor será a cobertura de ataques ciberneticos e menor o espaço ocupado em disco.

Bom! Uma vez concluída a instalação inicial do sistema operacional para para as etapas finais.

### Acessando a máquina virtual

Uma vez concluída a instalação a máquina virtual é desligada, podemos verificar este estado com o comando abaixo:

```bash
virsh list --all

 Id   Name      State
--------------------------
 -    centos8   shut off
```

Para 'ligar' a máquina virtual execute o comando:

```bash
virsh start centos8
```

E para acessar a console da máquina virtual utilize o mesmo comando de antes:

```bash
virt-viewer centos8
```

Se preferir pode usar o `virt-manager` para acessar estas e outras funcionalidades via interface gráfica.

### Ajustes pós install

Para completar a instalação do sistema operacional vamos instalar alguns pacotes básicos e necessários para a manutenção futura de nossa máquina virtual.

> Se quiser acessar 

```bash
sudo yum update -y;
sudo yum install -y \
  epel-release \
  curl \
  wget \
  telnet \
  net-tools \
  unzip;
```

Para concluirmos esta etapa, precisamos desabilitar a funcionalidade [ZeroConf](https://en.wikipedia.org/wiki/Zero-configuration_networking) para que a configuração de rede seja feita manualmente quando da instalação das futuras máquinas virtuais baseadas neste template, para tanto execute o comando abaixo.

```bash
sudo echo "NOZEROCONF=yes" >> /etc/sysconfig/network
```

Pronto, agora podemos ir para a etapa final, então vamos desligar a nossa máquina virtual.

```bash
sudo poweroff
```

### Limpando a imagem

Antes de seguir certifique-se que a máquina virtual está desligada.

```bash
virsh list --all

 Id   Name      State
--------------------------
 -    centos8   shut off
```

Agora vamos utilizar o útilitário [virt-sysprep](https://libguestfs.org/virt-sysprep.1.html) que tem a função de redefinir, desconfigurar ou personalizar uma máquina virtual para que possam ser feitos os clones dessa máquina virtual.

```bash
sudo virt-sysprep -d centos8

[sudo] senha para sandro: 
[   0.0] Examining the guest ...
[   5.0] Performing "abrt-data" ...
[   5.0] Performing "backup-files" ...
[   5.2] Performing "bash-history" ...
[   5.2] Performing "blkid-tab" ...
[   5.2] Performing "crash-data" ...
[   5.2] Performing "cron-spool" ...
[   5.3] Performing "dhcp-client-state" ...
[   5.3] Performing "dhcp-server-state" ...
[   5.3] Performing "dovecot-data" ...
[   5.3] Performing "logfiles" ...
[   5.4] Performing "machine-id" ...
[   5.4] Performing "mail-spool" ...
[   5.4] Performing "net-hostname" ...
[   5.4] Performing "net-hwaddr" ...
[   5.5] Performing "pacct-log" ...
[   5.5] Performing "package-manager-cache" ...
[   5.6] Performing "pam-data" ...
[   5.6] Performing "passwd-backups" ...
[   5.6] Performing "puppet-data-log" ...
[   5.6] Performing "rh-subscription-manager" ...
[   5.7] Performing "rhn-systemid" ...
[   5.7] Performing "rpm-db" ...
[   5.7] Performing "samba-db-log" ...
[   5.7] Performing "script" ...
[   5.7] Performing "smolt-uuid" ...
[   5.8] Performing "ssh-hostkeys" ...
[   5.8] Performing "ssh-userdir" ...
[   5.8] Performing "sssd-db-log" ...
[   5.8] Performing "tmp-files" ...
[   5.8] Performing "udev-persistent-net" ...
[   5.8] Performing "utmp" ...
[   5.9] Performing "yum-uuid" ...
[   5.9] Performing "customize" ...
[   5.9] Setting a random seed
[   5.9] Setting the machine ID in /etc/machine-id
[   6.0] Performing "lvm-uuids" ...
```

E agora vamos remover a configuração da máquina virtual com o comando abaixo:

```bash
sudo virsh undefine centos8
```
Pronto! Procedimento concluído, agora podemos clonar a nossa máquina virtual nos próximos artigos.

Espero que tenha gostado, deixe seu Feedback abaixo e sinta-se à vontade para colaborar usando o menu da direita.

Até breve!
