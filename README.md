## Documentação Técnica: Projeto de Consultoria Estatística I (GES-120)
### Autor: Luíza Helena Pereira Lúcio

> **Nota de Reprodutibilidade:** Este documento foi atualizado ao longo de toda a execução do projeto visando servir como um documento de referência para possíveis reproduções. Fique à vontade para clonar este repositório e reproduzir as análises em sua máquina.

Este projeto foi desenvolvido para a disciplina **Consultoria em Estatística I (GES-120)**, do curso de Estatística da Universidade Federal de Lavras (UFLA), ministrada no 1º semestre de 2026 pelo professor Luiz Otávio Pala. 

## 1. Introdução e Contextualização

Como dito anteriormente, esse projeto foi desenvolvido como parte integrante da avaliação da disciplina GES-120, com peso de 40% na nota final, e foi estruturado para simular o ambiente real de uma consultoria estatística mercadológica ou acadêmica. O professor determinou que cada estudante selecionasse uma base de dados bruta específica, e o desafio era atuar como consultor estatístico, sendo responsável por todas as etapas de um projeto de análise de dados real. As etapas obrigatórias do projeto foram:

* Etapa 1: Organizar a base bruta, compreender suas variáveis, níveis e identificar valores atípicos ou erros.
* Etapa 2: Elaborar propostas de análises viáveis a serem discutidas e validadas individualmente com o orientador do projeto.
* Etapa 3: Documentar as decisões de limpeza de dados através de um relatório e um fluxograma de processamento.
* Etapa 4: Análise exploratória.
* Etapa 5: Justificar a escolha dos métodos utilizados e apresentar os procedimentos adotados na análise.
* Etapa 6: Apresentar os resultados de forma clara, com tabelas, gráficos e interpretações.

### 1.1. Sobre a minha base

Para cumprir esse projeto utilizei a base de dados do AutoSeg (Sistema de Estatísticas de Automóveis), mantida pela SUSEP (Superintendência de Seguros Privados), o recorte temporal e geográfico escolhido para a análise foi o 1º semestre de 2021, concentrando os dados exclusivamente no estado de São Paulo. Esse recorte foi uma sugestão do próprio professor, pois é uma base bem grande e só o estado de São Paulo já foi trabalhoso.

A questão central da minha consultoria, que também foi uma sugestão do professor, visou responder duas perguntas:

* Existem diferenças nos padrões de preços e custos de sinistros por sexo?
* Existe interação entre idade e sexo nas colisões? Homens jovens ou mulheres mais velhas se acidentam mais?

**Data de busca da base de dados:** 12/05/2026.

## 2. Organização da Base (Etapa 1)

Como essa era uma base bem grande, foi utilizado somente o ano de 2021 (primeiro semestre).

Tabelas Principais:

* arq_casco_comp - contém dados de exposição, prêmios, sinistros e importância segurada para a corbertura CASCO, classificados pela chave Categoria Tarifária/Região/Modelo/Ano/Sexo/Faixa Etária.
* arq_casco3_comp - contém dados de exposição, prêmios e sinistros para a corbertura CASCO, classificados pela chave Categoria Tarifária/CEP/Modelo/Ano.
* arq_casco4_comp - contém dados de exposição, prêmios e sinistros para a corbertura CASCO, classificados pela chave Categoria Tarifária/Cidade/Modelo/Ano.

Tabelas Auxiliares:

* auto2_vei - contém código e descrição de cada modelo de veículo, além do código do grupo a que pretence.
* auto2_grupo - código e descrição dos grupos de modelos
* auto_cat - código de descrição de categorias tarifárias
* auto_cau - código e descrição de causas de sinistros
* *auto_cep - correlaciona o CEP com cidades e regiões de circulação
* auto_cob - código e descrição de coberturas
* auto_idade - código e descrição de faixas etárias
* auto_reg - código e descrição de regiões de circulação
* auto_sexo - código e descrição de sexo (masculino, feminino, jurídico)

Decidi que irei usar a **arq_casco_comp** que contém dados de exposição, prêmios, sinistros e importância segurada para a corbertura CASCO, classificados pela chave Categoria. Essa base é do Brasil todo, então foi filtrado para SP:

| Código | Região de SP |
| :--- | :--- |
| 10 | SP - Litoral Norte e Baixada Santista| 
| 11 | SP - Met. de São Paulo |
| 12 | SP - Grande Campinas |
| 13 | SP - Ribeirão Preto e Demais Mun. de Campinas |

## 2.1. Estudo da Base

A base de dados original continha inicialmente 3.390.758 observações (linhas) e 22 variáveis (colunas), com dados de todo o território nacional. Após a primeira filtragem para definir a análise ao estado de São Paulo (SP), como dito anteriormente, ficamos com 438.164 observações e 22 variáveis. Como algumas variáveis não seriam utilizadas, foram removidas 3 delas (EXPOSICAO2, PREMIO2 e IS_MEDIA), resultando em um conjunto final com 19 variáveis. 
A distribuição por sexo na base filtrada foi de 189.430 registros masculinos e 132.493 femininos. Os demais registros correspondem à categoria jurídica (pessoa jurídica), que foi excluída das análises comparativas por não ser o foco do estudo.

### 2.1.1. Classificação de Variáveis

A classificação correta de cada variável está apresentada na tabela abaixo:

| Variável | Classificação Estatística |
| :--- | :--- |
| `COD_TARIF` | Qualitativo Nominal |
| `REGIAO` | Qualitativo Nominal |
| `COD_MODELO` | Qualitativo Nominal |
| `ANO_MODELO` | Quantitativo Discreto |
| `SEXO` | Qualitativo Nominal |
| `IDADE` | Qualitativo Ordinal |
| `EXPOSICAO1` | Quantitativo Contínuo |
| `PREMIO1` | Quantitativo Contínuo |
| `FREQ_SIN1` | Quantitativo Discreto |
| `INDENIZ1` | Quantitativo Contínuo |
| `FREQ_SIN2` | Quantitativo Discreto |
| `INDENIZ2` | Quantitativo Contínuo |
| `FREQ_SIN3` | Quantitativo Discreto |
| `INDENIZ3` | Quantitativo Contínuo |
| `FREQ_SIN4` | Quantitativo Discreto |
| `INDENIZ4` | Quantitativo Contínuo |
| `FREQ_SIN9` | Quantitativo Discreto |
| `INDENIZ9` | Quantitativo Contínuo |
| `ENVIO` | Qualitativo Nominal |

Eu não coloquei um dicionário, pois junto ao trabalho tem um documento chamado `DEFINICOES_AUTOSEG.pdf` que tem um dicionário do próprio AutoSeg, o que for utilizado para as análises serão descritas junto nas interpretações no `analise.ipynb`. 

### 2.1.2. Sobre valores ausentes

Essa base não tem valores ausentes (NA), mas vale destacar que a variável `IDADE` vai apenas de 0 a 5 porque o próprio sistema AutoSeg dividiu as idades em grupos (onde 0 é "Não informada", 1 é entre 18 e 25 anos, 2 é entre 26 e 35 anos, 3 é entre 36 e 45 anos, 4 é entre 46 e 55 anos e 5 representa os maiores de 55 anos), o que a torna uma variável qualitativa ordinal. Não vou detalhar os grupos das outras variáveis aqui para o texto não ficar gigante, mas vou comentando sobre eles ao longo da análise sempre que for necessário para entender os gráficos e os resultados.

* **Primeiras Impressões:** Uma situação que chamou a atenção logo de início foi justamente a questão da idade. Como os valores estavam variando apenas de 0 a 5, primeiro imaginei que as informações reais da coluna tivessem sido perdidas, mas depois percebi que os dados estavam em classes. Isso também aconteceu com outras variáveis na base. Por escolha, já que não atrasava as análises, optei por não converter esses tipos de dados no Python, mas vale lembrar que as minhas interpretações estão sendo guiadas pelo dicionário de variáveis, aconselho você a fazer o mesmo.

## 3. Possíveis Abordagens (Etapa 2) 

Após a organização da base, foram propostas duas abordagens de análise, discutidas e validadas com o professor:

Abordagem 1: Padrões de preços e custos por sexo, o objetivo aqui foi investigar se existem diferenças estatísticas significativas no prêmio médio cobrado e nos custos de indenização entre segurados do sexo masculino e feminino. Esta análise foi conduzida por meio do Teste T para ver o tamanho do efeito.

Abordagem 2: Interação entre idade e sexo nas colisões, aqui quis investigar se a frequência e o custo de colisões (parciais e perda total) variam de acordo com a faixa etária e o sexo, para identificar padrões como "homens jovens batem mais" ou "mulheres mais velhas tem sinistros mais caros".

Ambas as abordagens foram validadas pelo professor e desenvolvidas de forma integral nesse projeto.

## 4. Processamento e Registro Metodológico (Etapa 3)

O processamento da base de dados começou com a seleção da tabela arq_casco_comp, a única que continha as variáveis de sexo e faixa etária de forma simultânea. Em seguida, foi aplicada uma filtragem para manter apenas os registros do estado de São Paulo, correspondentes aos códigos 10, 11, 12 e 13 da variável REGIAO. Para melhorar o banco de dados, as colunas EXPOSICAO2, PREMIO2 e IS_MEDIA foram excluídas por não serem necessárias para as análises. Além disso, os casos com IDADE == 0 foram descartados das análises etárias por representarem informações não preenchidas.
Após essas etapas de filtragem, percebi que a base não apresentava valores ausentes (NAs) em nenhuma das 19 variáveis restantes. Por fim, optei por não excluir os outliers das variáveis de prêmio e indenização, visto que valores muito altos em seguros representam eventos reais e importantes, como a perda total de veículos caros. Para evitar que esses extremos distorcessem a interpretação dos dados, as análises descritivas passaram a apresentar a mediana sempre acompanhada da média.

### 4.1. Fluxograma de Processamento de Dados

![Fluxograma das Etapas](etapas.jpg)

## 5. Análise Exploratória dos Dados (Etapa 4)

## 5.1. Resumo Geral por Sexo (Tabela 1)

A Tabela 1 apresenta as principais métricas calculadas para cada sexo: prêmio médio, frequência sinistral total, custo médio por exposto e severidades específicas por tipo de sinistro (roubo, colisão parcial e perda total), acompanhadas das medianas de prêmio e indenização. Os resultados mostram que homens pagam um prêmio médio levemente superior ao das mulheres. A frequência sinistral total também difere marginalmente entre os grupos, sem que um padrão claro de maior risco seja atribuído exclusivamente a um dos sexos. As medianas tendem a ser inferiores as médias em todas as variáveis, o que quer dizer que poucos sinistros de alto valor elevam a média sem representar o comportamento da maioria dos segurados.

## 5.2. Resumo de Riscos por Idade e Sexo (Tabela 2)

A Tabela 2 detalha as métricas de colisão (parcial e perda total) segmentadas por faixa etária e sexo. Os registros com IDADE == 0 (não informada) foram excluídos desta análise, porque estavam "ganhando" na maioria dos casos. Aqui percebemos que as faixas etárias mais jovens (faixa 1: 18 a 25 anos) tendem a apresentar frequências de colisão mais elevadas em ambos os sexos, o que faz sentido. Nas faixas de 26 a 45 anos, as frequências svão reduzindo. Nas faixas mais velhas (acima de 55 anos), vemos uma leve elevação na severidade, que pode ser talvez associada ao maior valor dos veículos segurados por esse perfil, mas eu não cheguei a analisar por isso.

## 5.3. Prêmio Médio por Sexo (Gráfico 1 e Teste T)

O Gráfico 3 compara o prêmio médio entre homens e mulheres por meio de um gráfico de barras simples. Visualmente percebemso que a diferença entre os grupos é pequena. O Teste T foi aplicado para verificar se essa diferença é estatisticamente significativa. 

## 5.4. Frequência de Colisão Parcial por Idade e Sexo (Gráfico 1)

O Gráfico 1 apresenta a frequência de colisão parcial (número de colisões por veículo exposto) ao longo das faixas etárias, separada por sexo. Nesse gráfico podemos ver que as mulheres apresentam frequência de colisão parcial levemente superior aos homens na faixa jovem (18-25 anos), contrariando o que eu imaginava. Nas faixas intermediárias, as curvas se aproximam e se cruzam em diferentes pontos. Essa sobreposição das curvas reforça que a diferença entre sexos na frequência de colisão é pequena ao longo de todas as faixas etárias, sem que se possa afirmar que um sexo é consistentemente mais propenso a colisões do que o outro.

## 5.5. Frequência de Perda Total por Idade e Sexo (Gráfico 2)

O Gráfico 2 apresenta a frequência de perda total (veículos com dano irreparável ou roubados sem recuperação) por faixa etária e sexo. A frequência de perda total é visivelmente mais alta na faixa jovem e se reduz progressivamente com o aumento da idade. Homens jovens tendem a apresentar frequência de perda total superior a das mulheres na mesma faixa, o que pode estar associado tanto ao comportamento de risco quanto ao perfil de veículo segurado nessa faixa. Nas faixas mais velhas, as diferenças entre sexos se tornam praticamente inexistentes.

> Por favor, verifique os resultados na tabela no documento `relatorio.qmd` para ver a tabela.

## 6. Resultados e Discussão (Etapa 6)

As análises realizadas responderam às duas perguntas de pesquisa propostas:

1) **Existem diferenças nos padrões de preços e custos de sinistros por sexo?** Existem diferenças estatisticamente significativas no prêmio médio e na frequência de colisão entre homens e mulheres. 

2) **Existe interação entre idade e sexo nas colisões? Homens jovens ou mulheres mais velhas se acidentam mais?** A faixa etária tem influência mais evidente do que o sexo nos padrões de colisão. Segurados mais jovens (18-25 anos) apresentam maior frequência de colisão e de perda total em ambos os sexos. As diferenças entre homens e mulheres dentro de cada faixa são pequenas e não sustentam uma conclusão clara sobre qual grupo apresenta maior risco associado ao sexo.

## 7. Justificativas e Decisões Metodológicas

Para garantir a confiabilidade nos resultados estatísticos, foram adotadas as seguintes decisões metodológicas:

* Foco em SP: Essa decisão foi tomada porque a base era muito grande, só São Paulo já daria bastante trabalho. 
* Exclusão de Dados _Não Informados_: Os registros cujo código de idade ou sexo constavam como 0 (Não informada) foram desconsiderados das análises de perfil e dos gráficos. Como o objetivo principal era responder sobre idades e sexo colocar dados assim não faria sentido. O mesmo foi feito com o perfil J (jurídico).
* Escolha do Teste T: Para validar se as diferenças observadas nos preços e nos custos de colisão entre homens e mulheres não eram meras coincidências, meu professor aconselhou.

No geral não houve tantas decisões metodológicas, porque a análise foi bem básica.

## 8. Uso de Inteligência Artificial

Durante o desenvolvimento deste projeto, foi utilizada a ferramenta de Inteligência Artificial Gemini (Google) e Claude (Anthropic) como apoio ao desenvolvimento de códigos e rotinas computacionais, conforme permitido pelas instruções do trabalho.

Etapas em que houve utilização:

Etapa 3: Auxílio na correção do código Python para filtragem da base, cálculo de métricas (prêmio médio, frequência sinistral, severidade).
Etapa 4: Auxílio na implementação do Teste T.

## 8.1 Avaliação crítica

Acredito que a utilização da IA foi positiva para acelerar a escrita de código, uma vez que o tempo para fazer esse trabalho foi curto, e para identificar erros pontuais, como a chamada de função com nome errado e outros erros de sintaxe que são os mais comuns pra mim. A escolha do Teste T foi um conselho do professor para avaliar se era significativo em termos estatísticos, a parte da IA serviu só para auxílio no código, pois eu não sabia reproduzir em python, somente no R. Acredito que a IA não substitui o meu raciocínio, apesar de um trabalho bem estruturado do que deveria ser entregue, a maior parte do tempo eu levei para organizar tudo que eu deveria fazer e depois organizar essa base, a IA serviu como ferramenta de apoio na implementação. Talvez uma das maiores limitações observadas no uso de IA foi que o Claude é meio "acelerado" muitas vezes eu pedia uma correção em um código e ele me respondia com mais coisas do que eu pedia, isso tomava meu tempo, porque eu precisava conferir linha por linha no código, se tivesse mais tempo eu não teria usado IA para essa parte, acredito que teria plena capacidade de fazer tudo sozinha. Acredito que o Gemini também alucina demais, se não prestar atenção ele te dá respostas sem pé nem cabeça.




