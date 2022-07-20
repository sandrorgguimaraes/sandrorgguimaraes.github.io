---
title: Dicas
date: 2022-07-20
weight: 100
description: Dicas para a preparação e para o dia da prova.
resources:
- src: "**.{png,jpg}"
  title: "Image #:counter"
---

Segue abaixo um compilado de várias dicas que pegamos durante a preparação para a prova que acredito vá ajudar a todos.

1. Pratique, pratique e pratique... e depois volte e pratique mais um pouco.
   - No nosso [guia de estudos](../../cka/) você encontrará em cada tópico `Link's úteis` e sempre que possível teremos um link direto para `Doc K8S - Tarefas` específica, vá lá e faça o exercício.
   - Na [documentação oficial do Kubernetes](https://kubernetes.io/docs/tasks/) tem disponível muitos exercícios, veja os que estão relacionados ao [curriculo da certificação](https://github.com/cncf/curriculum) e faça-os também.
   - [KillerCoda - CKA](https://killercoda.com/killer-shell-cka) é um ambiente interativo com vários exercícios.
   - Repositório [K8s Practice Training](https://github.com/StenlyTU/K8s-training-official) cotém alguns exercícios.
   - Site [Kube by Example](https://kubebyexample.com/).
2. Estude sobre a estrutura dos arquivos `.yaml` / `.json` e aprenda a fazer consultas utilizando o `JSON PATH`.
   - Curso gratuito:
      - [JSON Path Test – Free Course](https://kodekloud.com/courses/json-path-quiz/?no_frame=1) da [KodeKloud](https://kodekloud.com/).
   - Sites:
      - [JSONPath Online Evaluator](https://jsonpath.com/).
      - [JSONPath expressions](https://goessner.net/articles/JsonPath/index.html#e2).
3. Otimize o seu ambiente, criando `alias` e `variáveis de ambiente`, para poupar preciosos segundos durante a avaliação. Comece a fazer isso já durante os simulados até decorar (lembre-se que no dia da prova não terá acesso a recursos externos).
   - No arquivo `~/.bashrc`:

      ```bash
      #
      # Estes 3 (três) primeiros comandos não precisa decorar.
      #
      
      # Criação do alias `k` e configuração do autocomplete já devem estar pré configurados no ambiente
      # mas vale a pena conferir, na dúvida consulte a documentação oficial procurando por
      # `kubectl cheat` e ir para a página https://kubernetes.io/docs/reference/kubectl/cheatsheet/
      alias k="kubectl"
      source <(kubectl completion bash)
      complete -o default -F __start_kubectl k
      ```

      ```bash
      #
      # Já os 4 (quatro) comandos abaixo, será necessário decorar.
      #

      # O comando para alterar de contexto / cluster normalmente é dado na questão,
      # mas para alterar entre os namespaces não.
      # Exemplo de uso:
      #   kns <nome-do-namespace>
      alias kns="kubectl config set-context --current --namespace" 

      # Para agilizar a criação de objetos via manifestos (arquivos .yaml)
      # Exemplo de uso:
      #   ka <nome-do-arquivo>.yaml
      alias ka="kubectl apply -f"

      # Para facilitar a validação dos comandos e geração dos manifestos (arquivos .yaml).
      # Exemplos de uso:
      #   k run <nome-pod> --image=<nome-da-image> $do
      #   k run <nome-pod> --image=<nome-da-image> $do > <nome-do-arquivo>.yaml
      export do="--dry-run=client -o yaml"

      # Para não esperar pela exclusão de um objeto.
      # Exemplo de uso:
      #   k delete pod <nome-do-pod> $now
      export now="--force --grace-period 0"
      ```

   - No arquivo `~/.vimrc`:

      ```bash
      set autoindent expandtab tabstop=2 shiftwidth=2
      ```

      ou

      ```bash
      set autoindent
      set expandtab
      set tabstop=2
      set shiftwidth=2
      ```

      Neste artigo [Set Indentation Width to 2 or 4 Spaces (or Tab) in Vim](https://linuxhandbook.com/vim-indentation-tab-spaces/) tem uma explicação direta e objetiva sobre essas configurações.
