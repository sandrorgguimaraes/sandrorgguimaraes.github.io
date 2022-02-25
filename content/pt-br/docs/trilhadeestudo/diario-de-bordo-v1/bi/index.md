---
title: Business Intelligence
linkTitle: BI
date: 2022-02-23T00:00:00.000Z
categories:
    - data warehouse
tags:
    - bi
    - business intelligence
autores:
    - Sandro Rogério
description: O que é um BI (Business Intelligence)?
keywords:
    - bi
    - business intelligence
slug: bi-business-intelligence
weight: 15
resources:
- src: "**.{png,jpg}"
  title: "Image #:counter"
  params:
    byline: "Fonte: http://revistas.utfpr.edu.br/pb/index.php/CAP/article/viewFile/933/544"
---


{{% pageinfo %}}
O termo **Business Intelligence** ou Inteligência Empresarial foi utilizado pela primeira vez na década de 80 pelo Gartner Group, porém o conceito remonta desde a antiguidade onde, por exemplo, se observavam a posição dos astros, os períodos de sol / chuva e as marés para a tomada de decisões.
{{% /pageinfo %}}

Com o desenvolvimento dos sistemas computacionais ao longo das últimas décadas, observa-se também o desenvolvimento do BI, impulsionado pela permanente necessidade das organizações de manterem a competitividade.

Atualmente, pode-se sintetizar **BI** como um conjunto de ferramentas, conceitos e metodologias que se utiliza da tecnologia da informação (TI) para coletar ***dados***, analisá-los e transformá-los em ***informação***. Com isso, os sistemas de BI concedem às organizações ***conhecimento*** sobre seus negócios, contribuindo para que os gestores optem pela ***decisão*** mais acertada.

{{< imgproc bi Fill "400x306" >}} {{< /imgproc >}}

Os componentes de uma estrutura de BI são basicamente:

- Dados operacionais;
  - É a matéria prima do BI, são os dados originados das aplicações utilizadas no dia-a-dia da organização, por exemplo o ERP.
- ODS (Operacional Data Store);
  - Armazena os dados operacionais de forma consolidada, porém não possuem características dimensionais (como o DW e DM).
- Ferramentas de ETL (Extração, Transformação e Carga);
  - Veremos com mais detalhes posteriormente.
- Data Warehouse (DW) e Data Marts (DM);
  - Veja postagem.
- Data Mining (Mineração de Dados);
  - Veremos com mais detalhes posteriormente.
- Visualização dos resultados;
  - Ferramentas que permitem ao usuário final visualizar de forma amigável as informações para auxiliar na tomada das decisões.

Este é um breve resumo sobre BI, persistindo alguma dúvida ou necessitando de maiores esclarecimentos, registre seu comentário e vamos aprender juntos.

> Fonte:
> ANTONELLI, Ricardo A. Conhecendo o Business Intelligence (BI) - Uma Ferramenta de Auxílio à Tomada de Decisão. Revista TECAP. v. 3, n. 3, 2009. Disponível em: <http://revistas.utfpr.edu.br/pb/index.php/CAP/article/viewFile/933/544> acesso em: 14 de mar. 2012.
