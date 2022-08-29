---
title: Visão geral
date: 2022-02-23T00:00:00.000Z
categories:
    - data warehouse
tags:
    - data warehouse
    - sad
    - sistema apoio decisao
    - bi
    - business intelligence
    - data mart
    - oltp
    - olap
autores:
    - Sandro Rogério
description: Visão geral do que é um DW (Data Warehouse) e seus componentes?
keywords:
    - data warehouse
    - sad
    - sistema apoio decisao
    - bi
    - business intelligence
    - data mart
    - oltp
    - olap
slug: data-warehouse
weight: 10
resources:
- src: "**.{png,jpg}"
  title: "Image #:counter"
---

{{% pageinfo %}}
Em resumo um **DW** (*Data Warehouse*) pode ser definido como um banco de dados especializado, que integra e gerencia o fluxo de informações a partir de bancos de dados corporativos e fonte de dados externas à organização.
{{% /pageinfo %}}

Por trás de uma solução de DW  existe uma série de ferramentas / tecnologias /  conceitos (peças) que precisam ser combinadas para atingir um objetivo específico.

E para não sair do padrão temos a tradicional sopa de letrinhas: SAD, OLAP, OLTP, ETL, ODS além de alguns termos como Data Mart, Data Mining, Metadados, etc...

> Fontes:
>
> - MARTINS, C. E. W.; SILVA, E. L. da.; MUSSI, E. A. S. Implantação de um ODS no Banco do Brasil visando um DATA WAREHOUSE - Estudo de Caso. 1999. 51 f. Tese (MBA em Tecnologia) - Escola Politécnica, Universidade de São Paulo, Brasília. 1999.
> - HARRISON, Thomas H. Intranet Data Warehouse. São Paulo, Berkeley, 1998. 362 p.

## SAD (Sistema de Apoio à Decisão)

{{% pageinfo %}}
A definição do termo **SAD** (*Sistema de Apoio à Decisão*) ainda provoca divergência entre vários autores, mas sinteticamente pode-se definir como: "Conjunto de ferramentas que visa auxiliar os gestores na tomada de decisões".
{{% /pageinfo %}}

Um SAD se diferencia de outros sistemas existentes nas organizações basicamente por:

- Manipular grandes volumes de dados;
- Obter dados de fontes diferentes (internas e externas);
- Flexibilidade de relatórios gerenciais;
- Execução de rotinas de otimização e heurística;
- Execução de análises de simulação;
- Suporte para diversos níveis na tomada de decisão;

Com a utilização de um SAD as organizações tem melhores condições de, por exemplo:

- Decidir sobre fazer ou não uma promoção, de qual produto e quando;
- Projetar o risco ou o sucesso do investimento em determinado empreendimento ou do lançamento de um produto;
- Decidir se deve contratar ou não novos funcionários no próximo mês;

Ou seja, com este sistema os gestores têm condições de tomar decisões não apenas na intuição.

{{< imgproc sad Fill "400x286" >}}
{{< /imgproc >}}

> Fonte:
>
> - FALSARELLA, Orandi M.; CHAVES, Eduardo O. C. Sistemas de Informação e Sistemas de Apoio à Decisão. Disponivel em: <http://www.chaves.com.br/TEXTSELF/COMPUT/sad.htm> Acesso em: 13 de mar. 2012.
> - BORTOLIN, Sérgio A. M. Sistema de Apoio à Decisão. Disponível em: <http://www.al.urcamp.tche.br/infocamp/edicoes/nov05/Apoio%20a%20Decisao.pdf> Acesso em: 13 de mar. 2012.
> - PRIMAK, Fabio Vinicius. Decisões com B.I. Business Intelligence. Brasil. 1a edição. Ciência Moderna. 2008. 168 p. ISBN 8573937149.

## BI (Business Intelligence)

{{% pageinfo %}}
O termo **Business Intelligence** ou Inteligência Empresarial foi utilizado pela primeira vez na década de 80 pelo Gartner Group, porém o conceito remonta desde a antiguidade onde, por exemplo, se observavam a posição dos astros, os períodos de sol / chuva e as marés para a tomada de decisões.
{{% /pageinfo %}}

Com o desenvolvimento dos sistemas computacionais ao longo das últimas décadas, observa-se também o desenvolvimento do BI, impulsionado pela permanente necessidade das organizações de manterem a competitividade.

Atualmente, pode-se sintetizar **BI** como um conjunto de ferramentas, conceitos e metodologias que se utiliza da Tecnologia da Informação (TI) para coletar ***dados***, analisá-los e transformá-los em ***informação***.

Com isso, os sistemas de BI concedem às organizações ***conhecimento*** sobre seus negócios, contribuindo para que os gestores optem pela ***decisão*** mais acertada.

{{< imgproc bi Fill "400x306" >}}
{{< /imgproc >}}

Os componentes de uma estrutura de BI são basicamente:

- Dados operacionais;
  - É a matéria prima do BI, são os dados originados das aplicações utilizadas no dia-a-dia da organização, por exemplo o ERP.
- ODS (*Operacional Data Store*);
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

## DataWarehouse (DW) & Data Mart (DM)

Um DW (*Data Warehouse* ou Armazém de Dados) é uma coleção de dados orientada por assunto, integrada, variante no tempo e não volátil que tem por objetivo dar suporte aos processos de tomada de decisão.

É um repositório central que armazena dados de várias fontes, transformando-os em um modelo comum, multidimensional, para a realização de consultas e análises mais eficientes.

A diferença entre um DW e um DM basicamente consiste no volume de dados, abrangência e foco. Enquanto o DW foca na organização como um todo os DM´s focam em um determinado departamento ou conjunto especifíco de usuário, por exemplo.

A construção deste armazém pode acontecer de duas formas, cada abordagem têm seus prós e contras. As circunstâncias e particularidades de cada projeto é que determinarão qual utilizar.

Na abordagem *Top-Down* primeiro se monta o DW (corporativo) para num segundo momento criar os DM (departamentais).

{{< imgproc top-down Fill "400x212" >}}
Fonte: http://www.dataprix.net/pt-pt/24-data-mart
{{< /imgproc >}}

Ou utilizar a abordagem *Bottom-UP* onde primeiro é criado os DM´s para em seguida montar o DW da organização.

{{< imgproc bottom-up Fill "400x213" >}}
Fonte: http://www.dataprix.net/pt-pt/24-data-mart
{{< /imgproc >}}

## OLTP & OLAP

**OLTP** (*Online Transaction Processing* ou Processamento de Transações em Tempo Real) caracteriza-se por um grande número de transações (*INSERT, UPDATE e DELETE*) envolvendo uma pequena quantidade de dados em um ambiente multi-acesso, mantendo a integridade referencial.

No planejamento do banco de dados busca-se reduzir o tamanho e a redundância dos dados e normalmente se aplica a [3FN (terceira Forma Normal)](https://www.google.com/search?q=3FN+(terceira+Forma+Normal)) na modelagem, são os dados primitivos ou dados operacionais segundo [W. H. Inmon](https://en.wikipedia.org/wiki/Bill_Inmon).

**OLAP** (*Online Analytical Processing* ou Processamento Analítico em Tempo Real) caracteriza-se por poucas transações (*INSERT e SELECT*) envolvendo um volume muito grande de dados.

Na busca por eficiência das consultas, os dados armazenados no DW ou DM´s, normalmente são desnormalizados e utilizam esquemas multi dimensionais, são os dados derivados ou dados **SAD** segundo [W. H. Inmon](https://en.wikipedia.org/wiki/Bill_Inmon).

{{< imgproc olap-oltp Fill "400x213" >}}
Fonte: http://datawarehouse4u.info/OLTP-vs-OLAP.html
{{< /imgproc >}}

Juntamente com o OLAP surgiram novos termos como *DOLAP*, *ROLAP*, *MOLAP*, *HOLAP*, *slice and dice*, *pivot table*, *drill down/up* que ampliaremos posteriormente.

A tabela abaixo extraída do livro "Como construir o Data Warehouse" de [W. H. Inmon](https://en.wikipedia.org/wiki/Bill_Inmon) apresenta algumas diferenças conceituais entre os dados primitivos e os dados derivados, vejamos:

| DADOS PRIMITIVOS ou DADOS OPERACIONAIS | DADOS DERIVADOS ou DADOS SAD |
| --- | --- |
| Baseados em aplicações | Baseados em assuntos ou negócios |
| Detalhados | Resumidos ou refinados |
| Exatos em relação ao momento do acesso | Representam valores de momentos já decorridos ou instantâneos |
| Atendem à comunidade funcional | Atendem à comunidade gerencial |
| Podem ser atualizados | Não são atualizados |
| São processados repetitivamente | Processados de forma heurística |
| Requisitos de processamento conhecidos com antecedência | Requisitos de processamento não são conhecidos com antecedência |
| Compatíveis com o SDLC | Ciclo de vida completamente diferente |
| Performance é fundamental | Performance atenuada |
| Acessados uma unidade por vez | Acessados um conjunto por vez |
| Voltados para transações | Voltados para análises |
| O controle de atualizações é atribuição de quem tem a posse | O controle de atualizações não é problema |
| Alta disponibilidade | Disponibilidade atenuada |
| Gerenciados em sua totalidade | Gerenciados por subconjuntos |
| Não contemplam a redundância | A redundância não pode ser ignorada |
| Estrutura fixa; conteúdos variáveis | Estrutura flexível |
| Pequena quantidade de dados usada em um processo | Grande quantidade de dados usada em um processo |
| Atendem às necessidades cotidianas | Atendem às necessidades gerenciais |
| Alta probabilidade de acesso | Baixa ou modesta probabilidade de acesso |

## O ambiente do DW

Devido às diferenças existentes entre os dados primitivos e os dados derivados como vimos em [*OLTP & OLAP*](#oltp--olap), outros aspectos por consequencia necessitam de nova abordagem segundo Inmon.

### Os níveis dos dados

Estas diferenças acabam por gerar 4 (quatro) níveis de dados na organização, como segue:

- **Operacional** ==> Contém os dados primitivos que atende às transações **OLTP**, refletindo o valor atual dos registros;
- **Atômico / Data Warehouse** ==> Contém dados primitivos que não são atualizados e alguns dados derivados, não existe a sobreposição de valores, mantendo um histórico dos registros através da utilização de um elemento tempo associado a chave de cada registro;
- **Departamental** ==> Contém apenas dados derivados e agrupados por departamento, tem-se uma base de dados para o Marketing, outro para o RH, outro para o Financeiro e assim por diante. Também existe neste nível o elemento tempo associado a chave de cada registro;
- **Individual** ==> Contém dados que serão utilizados nas análises heurísticas, normalmente são dados temporários de pequenas proporçoes e utilizados pelos Sitemas de Informações Executivas (EIS).

### Ciclo de vida do desenvolvimento de sistemas

As diferenças entre os sistemas tradicionais e um **DW** não termina na forma de armazenar / modelar os dados, o desenvolvimento de sistemas para um ambiente de **DW** é praticamente o oposto ao tradicional **SDLC** (*Systems Development Life Cycle*), conforme abaixo:

| SDLC Clássico | SDLC de um DW |
| --- | --- |
| Ciclo de vida baseado em requisitos | Ciclo de vida baseado em dados |
| Levantamento de necessidades | Implementar o warehouse |
| Análise | Integrar os dados |
| Projeto | Procurar distorções |
| Programação | Programas para os dados |
| Teste | Projetar sistemas SAD |
| Integração | Analisar os resultados |
| Implementação | Entender necessidades |

### Utilização de Hardware

Em um sistema **OLTP** o consumo do hardware mantém um certo padrão / média de utilização durante o tempo. É possível planejar o crescimento / upgrade do sistema.

É possível prever os picos de utilização por conta da sazonalidade, por exemplo, no natal o sistema das operadoras de cartão de crédito têm um pico de utilização.

Em contrapartida um sistema **DW / SAD** terá picos de utilização do hardware apenas quando houver solicitações de **ETL** e / ou consultas **OLAP** dos usuários e logo em seguida o hardware voltará a ficar ocioso.

Neste cenário consegue-se prever o ritmo de crescimento da base de dados, porém o consumo de CPU / memória RAM e espaço para dados temporários já fica mais complicado.

Ou seja, a configuração / ajuste fino do hardware que vai atender às demandas dos sistemas legados (**OLTP**) é totalmente diferente das necessidades / especificações dos sistemas que irão atender às demandas dos sistemas **DW / OLAP**.
