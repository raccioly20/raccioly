---
title: "Carregando dados, Manipulando e Visualizando"
subtitle: ""
excerpt: "Lençóis"
date: 2022-01-11
author: "Ricardo Accioly"
draft: false
images:
series:
tags:
categories:
layout: single
output:
  html_document:
    toc: yes
    code_download: yes
---





## Entrada de dados no R

## Lendo dados de arquivos CSV


```r
library(tidyverse)
```

```
## -- Attaching packages --------------------------------------- tidyverse 1.3.1 --
```

```
## v ggplot2 3.3.5     v purrr   0.3.4
## v tibble  3.1.6     v dplyr   1.0.7
## v tidyr   1.1.4     v stringr 1.4.0
## v readr   2.1.0     v forcats 0.5.1
```

```
## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

```r
## Vamos definir a url de onde vamos fazer download dos dados
url <- "https://www.gov.br/anp/pt-br/centrais-de-conteudo/dados-abertos/arquivos/ie/derivados/importacoes-exportacoes-derivados-2000-2021.csv"
# O read.delim permite você definir qual o delimitador(;)
imp_exp_deriv <- url %>% read_delim(delim = ";", show_col_types = FALSE)
head(imp_exp_deriv)
```

```
## # A tibble: 6 x 6
##     ANO MÊS   PRODUTO `OPERAÇÃO COMERCIAL` `IMPORTADO / EXPOR~ `DISPÊNDIO / REC~
##   <dbl> <chr> <chr>   <chr>                <chr>                           <dbl>
## 1  2000 ABR   ASFALTO EXPORTAÇÃO           1362,035577                    309443
## 2  2000 AGO   ASFALTO EXPORTAÇÃO           2700,793269                    544917
## 3  2000 DEZ   ASFALTO EXPORTAÇÃO           1839,723077                    426760
## 4  2000 FEV   ASFALTO EXPORTAÇÃO           1676,491346                    270633
## 5  2000 JAN   ASFALTO EXPORTAÇÃO           1702,567308                    360648
## 6  2000 JUL   ASFALTO EXPORTAÇÃO           1566,573077                    406048
```

Observe que as colunas estão todas em maiúsculas e com nomes meio grandes

Vamos começar corrigindo os nomes das colunas


```r
imp_exp_deriv <- imp_exp_deriv %>%
  rename(
    ano = ANO, mes = MÊS, produto = PRODUTO,
    oper_comer = `OPERAÇÃO COMERCIAL`,
    imp_exp = `IMPORTADO / EXPORTADO`,
    disp_receita = `DISPÊNDIO / RECEITA`
  )
```


Veja que a coluna Importado / Exportado veio como texto

Como no R a casa  decimal é dada por "." temos que substituir nossa "," por ponto e depois transformar em número.


```r
imp_exp_deriv$imp_exp <- as.numeric(gsub(",", ".", gsub("\\.", "", imp_exp_deriv$imp_exp)))
```


Agora vamos ver como ficou a importação de dados

```r
knitr::kable(head(imp_exp_deriv, 10),
  booktabs = TRUE,
  caption = "Importação e Exportação de Derivados"
)
```



Table: Table 1: Importação e Exportação de Derivados

|  ano|mes |produto |oper_comer | imp_exp| disp_receita|
|----:|:---|:-------|:----------|-------:|------------:|
| 2000|ABR |ASFALTO |EXPORTAÇÃO |    1362|       309443|
| 2000|AGO |ASFALTO |EXPORTAÇÃO |    2701|       544917|
| 2000|DEZ |ASFALTO |EXPORTAÇÃO |    1840|       426760|
| 2000|FEV |ASFALTO |EXPORTAÇÃO |    1676|       270633|
| 2000|JAN |ASFALTO |EXPORTAÇÃO |    1703|       360648|
| 2000|JUL |ASFALTO |EXPORTAÇÃO |    1567|       406048|
| 2000|JUN |ASFALTO |EXPORTAÇÃO |    2171|       488515|
| 2000|MAI |ASFALTO |EXPORTAÇÃO |     696|       132424|
| 2000|MAR |ASFALTO |EXPORTAÇÃO |    2086|       406910|
| 2000|NOV |ASFALTO |EXPORTAÇÃO |    1988|       506260|


## Lendo dados de arquivos excel no diretório


```r
library(readxl)
frota_veiculos <- read_excel("d_frota_por_uf_municipio_combustivel_outubro_2021.xlsx")
head(frota_veiculos)
```

```
## # A tibble: 6 x 4
##   UF    Município  `Combustível Veículo`       `Qtd. Veículos`
##   <chr> <chr>      <chr>                                 <dbl>
## 1 ACRE  ACRELANDIA ALCOOL                                   60
## 2 ACRE  ACRELANDIA ALCOOL/GASOLINA                        2252
## 3 ACRE  ACRELANDIA DIESEL                                 1064
## 4 ACRE  ACRELANDIA GASOLINA                               3592
## 5 ACRE  ACRELANDIA GASOLINA/ALCOOL/GAS NATURAL               1
## 6 ACRE  ACRELANDIA Sem Informação                          167
```



## Bibliotecas

As bibliotecas que vamos usar aqui são a `dplyr` e a `ggplot2` que serão usadas na manipulação e visualização de dados. 


Vamos primeiro conhecer o que tem no arquivo de frota nacional de veículos de outubro de 2021. 

A base de dados possui 43812 linhas


```r
class(frota_veiculos) # Tipo de base de dados
```

```
## [1] "tbl_df"     "tbl"        "data.frame"
```

```r
summary(frota_veiculos) # Sumario da base de dados
```

```
##       UF             Município         Combustível Veículo Qtd. Veículos    
##  Length:43812       Length:43812       Length:43812        Min.   :      1  
##  Class :character   Class :character   Class :character    1st Qu.:      5  
##  Mode  :character   Mode  :character   Mode  :character    Median :    123  
##                                                            Mean   :   2584  
##                                                            3rd Qu.:    925  
##                                                            Max.   :4079631
```

```r
head(frota_veiculos) # Primeiras linhas da base de dados
```

```
## # A tibble: 6 x 4
##   UF    Município  `Combustível Veículo`       `Qtd. Veículos`
##   <chr> <chr>      <chr>                                 <dbl>
## 1 ACRE  ACRELANDIA ALCOOL                                   60
## 2 ACRE  ACRELANDIA ALCOOL/GASOLINA                        2252
## 3 ACRE  ACRELANDIA DIESEL                                 1064
## 4 ACRE  ACRELANDIA GASOLINA                               3592
## 5 ACRE  ACRELANDIA GASOLINA/ALCOOL/GAS NATURAL               1
## 6 ACRE  ACRELANDIA Sem Informação                          167
```

```r
str(frota_veiculos) # tipo de dados
```

```
## tibble [43,812 x 4] (S3: tbl_df/tbl/data.frame)
##  $ UF                 : chr [1:43812] "ACRE" "ACRE" "ACRE" "ACRE" ...
##  $ Município          : chr [1:43812] "ACRELANDIA" "ACRELANDIA" "ACRELANDIA" "ACRELANDIA" ...
##  $ Combustível Veículo: chr [1:43812] "ALCOOL" "ALCOOL/GASOLINA" "DIESEL" "GASOLINA" ...
##  $ Qtd. Veículos      : num [1:43812] 60 2252 1064 3592 1 ...
```



## Dados da frota de veículos

1) UF = Unidade Federativa
2) Município
3) Combustível Veículo
4) Qtd. Veículos

Vamos ver quais informações de Estados nós temos


```r
uf <- frota_veiculos %>%
  select(UF) %>%
  unique() %>%
  arrange(UF) # crescente
knitr::kable(uf,
  booktabs = TRUE,
  caption = "Uma tabela mais elegante"
)
```



Table: Table 2: Uma tabela mais elegante

|UF                  |
|:-------------------|
|ACRE                |
|ALAGOAS             |
|AMAPA               |
|AMAZONAS            |
|BAHIA               |
|CEARA               |
|DISTRITO FEDERAL    |
|ESPIRITO SANTO      |
|GOIAS               |
|MARANHAO            |
|MATO GROSSO         |
|MATO GROSSO DO SUL  |
|MINAS GERAIS        |
|Não Identificado    |
|Não se Aplica       |
|PARA                |
|PARAIBA             |
|PARANA              |
|PERNAMBUCO          |
|PIAUI               |
|RIO DE JANEIRO      |
|RIO GRANDE DO NORTE |
|RIO GRANDE DO SUL   |
|RONDONIA            |
|RORAIMA             |
|SANTA CATARINA      |
|SAO PAULO           |
|Sem Informação      |
|SERGIPE             |
|TOCANTINS           |

Vamos eliminar nomes de Estados errados/ não informados


```r
frota_veiculos <- frota_veiculos %>% filter(UF != "Sem Informação" & UF != "Não Identificado" & UF != "Não se Aplica")
uf <- frota_veiculos %>%
  select(UF) %>%
  unique() %>%
  arrange(UF) # crescente
knitr::kable(uf,
  booktabs = TRUE,
  caption = "Lista de Estados"
)
```



Table: Table 3: Lista de Estados

|UF                  |
|:-------------------|
|ACRE                |
|ALAGOAS             |
|AMAPA               |
|AMAZONAS            |
|BAHIA               |
|CEARA               |
|DISTRITO FEDERAL    |
|ESPIRITO SANTO      |
|GOIAS               |
|MARANHAO            |
|MATO GROSSO         |
|MATO GROSSO DO SUL  |
|MINAS GERAIS        |
|PARA                |
|PARAIBA             |
|PARANA              |
|PERNAMBUCO          |
|PIAUI               |
|RIO DE JANEIRO      |
|RIO GRANDE DO NORTE |
|RIO GRANDE DO SUL   |
|RONDONIA            |
|RORAIMA             |
|SANTA CATARINA      |
|SAO PAULO           |
|SERGIPE             |
|TOCANTINS           |




```r
## Cria um extrato para o Estado do Rio de Janeiro
frota_RJ <- filter(frota_veiculos, UF == "RIO DE JANEIRO")
head(frota_RJ)
```

```
## # A tibble: 6 x 4
##   UF             Município      `Combustível Veículo`       `Qtd. Veículos`
##   <chr>          <chr>          <chr>                                 <dbl>
## 1 RIO DE JANEIRO ANGRA DOS REIS ALCOOL                                 1968
## 2 RIO DE JANEIRO ANGRA DOS REIS ALCOOL/GAS NATURAL VEICULAR              65
## 3 RIO DE JANEIRO ANGRA DOS REIS ALCOOL/GASOLINA                       26076
## 4 RIO DE JANEIRO ANGRA DOS REIS DIESEL                                 3862
## 5 RIO DE JANEIRO ANGRA DOS REIS ELETRICO/FONTE EXTERNA                    7
## 6 RIO DE JANEIRO ANGRA DOS REIS ELETRICO/FONTE INTERNA                    2
```


## Manipulação de dados
Vamos renomear alguns nomes de colunas


```r
frota_RJ2 <- frota_RJ %>%
  rename(municipio = Município, combustivel = "Combustível Veículo", quantidade = "Qtd. Veículos")
# melhorando a visualização dos dados
knitr::kable(
  head(frota_RJ2, 10),
  booktabs = TRUE
)
```



|UF             |municipio      |combustivel                 | quantidade|
|:--------------|:--------------|:---------------------------|----------:|
|RIO DE JANEIRO |ANGRA DOS REIS |ALCOOL                      |       1968|
|RIO DE JANEIRO |ANGRA DOS REIS |ALCOOL/GAS NATURAL VEICULAR |         65|
|RIO DE JANEIRO |ANGRA DOS REIS |ALCOOL/GASOLINA             |      26076|
|RIO DE JANEIRO |ANGRA DOS REIS |DIESEL                      |       3862|
|RIO DE JANEIRO |ANGRA DOS REIS |ELETRICO/FONTE EXTERNA      |          7|
|RIO DE JANEIRO |ANGRA DOS REIS |ELETRICO/FONTE INTERNA      |          2|
|RIO DE JANEIRO |ANGRA DOS REIS |GASOLINA                    |      27648|
|RIO DE JANEIRO |ANGRA DOS REIS |GASOLINA/ALCOOL/ELETRICO    |         15|
|RIO DE JANEIRO |ANGRA DOS REIS |GASOLINA/ALCOOL/GAS NATURAL |       2543|
|RIO DE JANEIRO |ANGRA DOS REIS |GASOLINA/ELETRICO           |         10|


## Ordenando pela Quantidade de Veículos


```r
frota_RJ2 %>% arrange(quantidade) # crescente
```

```
## # A tibble: 961 x 4
##    UF             municipio               combustivel              quantidade
##    <chr>          <chr>                   <chr>                         <dbl>
##  1 RIO DE JANEIRO APERIBE                 GASOLINA/ALCOOL/ELETRICO          1
##  2 RIO DE JANEIRO ARARUAMA                ELETRICO/FONTE EXTERNA            1
##  3 RIO DE JANEIRO ARARUAMA                GAS METANO                        1
##  4 RIO DE JANEIRO AREAL                   GASOLINA/ELETRICO                 1
##  5 RIO DE JANEIRO ARRAIAL DO CABO         GASOLINA/ALCOOL/ELETRICO          1
##  6 RIO DE JANEIRO BARRA MANSA             ELETRICO/FONTE EXTERNA            1
##  7 RIO DE JANEIRO BOM JARDIM              ELETRICO/FONTE INTERNA            1
##  8 RIO DE JANEIRO BOM JESUS DO ITABAPOANA GASOLINA/ALCOOL/ELETRICO          1
##  9 RIO DE JANEIRO CABO FRIO               GASOGENIO                         1
## 10 RIO DE JANEIRO CACHOEIRAS DE MACACU    ELETRICO/FONTE EXTERNA            1
## # ... with 951 more rows
```

## Ordenando decrescente


```r
frota_RJ2 %>% arrange(desc(quantidade)) # decrescente
```

```
## # A tibble: 961 x 4
##    UF             municipio       combustivel                   quantidade
##    <chr>          <chr>           <chr>                              <dbl>
##  1 RIO DE JANEIRO RIO DE JANEIRO  GASOLINA                         1019127
##  2 RIO DE JANEIRO RIO DE JANEIRO  ALCOOL/GASOLINA                   968402
##  3 RIO DE JANEIRO RIO DE JANEIRO  GASOLINA/ALCOOL/GAS NATURAL       385076
##  4 RIO DE JANEIRO RIO DE JANEIRO  GASOLINA/GAS NATURAL VEICULAR     291559
##  5 RIO DE JANEIRO RIO DE JANEIRO  ALCOOL                            138788
##  6 RIO DE JANEIRO RIO DE JANEIRO  DIESEL                            135660
##  7 RIO DE JANEIRO SAO GONCALO     GASOLINA                          121573
##  8 RIO DE JANEIRO NITEROI         GASOLINA                          108888
##  9 RIO DE JANEIRO NITEROI         ALCOOL/GASOLINA                   101127
## 10 RIO DE JANEIRO DUQUE DE CAXIAS GASOLINA                           98413
## # ... with 951 more rows
```

## Selecionando colunas


```r
frota_RJ2 %>% select(municipio, combustivel, quantidade) # selecionando 3 colunas
```

```
## # A tibble: 961 x 3
##    municipio      combustivel                 quantidade
##    <chr>          <chr>                            <dbl>
##  1 ANGRA DOS REIS ALCOOL                            1968
##  2 ANGRA DOS REIS ALCOOL/GAS NATURAL VEICULAR         65
##  3 ANGRA DOS REIS ALCOOL/GASOLINA                  26076
##  4 ANGRA DOS REIS DIESEL                            3862
##  5 ANGRA DOS REIS ELETRICO/FONTE EXTERNA               7
##  6 ANGRA DOS REIS ELETRICO/FONTE INTERNA               2
##  7 ANGRA DOS REIS GASOLINA                         27648
##  8 ANGRA DOS REIS GASOLINA/ALCOOL/ELETRICO            15
##  9 ANGRA DOS REIS GASOLINA/ALCOOL/GAS NATURAL       2543
## 10 ANGRA DOS REIS GASOLINA/ELETRICO                   10
## # ... with 951 more rows
```

## Criando colunas com mutate


```r
frota_RJ2 <- frota_RJ2 %>% mutate(quantidade_mil = quantidade / 1000)
frota_RJ2 %>%
  head() %>%
  knitr::kable(booktabs = TRUE)
```



|UF             |municipio      |combustivel                 | quantidade| quantidade_mil|
|:--------------|:--------------|:---------------------------|----------:|--------------:|
|RIO DE JANEIRO |ANGRA DOS REIS |ALCOOL                      |       1968|          1.968|
|RIO DE JANEIRO |ANGRA DOS REIS |ALCOOL/GAS NATURAL VEICULAR |         65|          0.065|
|RIO DE JANEIRO |ANGRA DOS REIS |ALCOOL/GASOLINA             |      26076|         26.076|
|RIO DE JANEIRO |ANGRA DOS REIS |DIESEL                      |       3862|          3.862|
|RIO DE JANEIRO |ANGRA DOS REIS |ELETRICO/FONTE EXTERNA      |          7|          0.007|
|RIO DE JANEIRO |ANGRA DOS REIS |ELETRICO/FONTE INTERNA      |          2|          0.002|

## Selecionando alguns tipos de combustivel / sumarizando


```r
## Criar um gráfico de barras de número de veículos por combústivel no RJ
S_frota_RJ2 <- frota_RJ2 %>%
  filter(combustivel == "GASOLINA" |
    combustivel == "DIESEL" |
    combustivel == "ALCOOL") %>%
  group_by(combustivel) %>%
  summarize(t_veiculos = sum(quantidade_mil))
```


## Gráfico de Barras usando o ggplot2


```r
ggplot(S_frota_RJ2, aes(x = combustivel, y = t_veiculos)) +
  geom_bar(stat = "identity")
```

<img src="imagens/Aula01/barra0-1.png" width="576" />


## Sumarizando por Região


```r
frota_SE <- frota_veiculos %>%
  rename(
    municipio = Município,
    combustivel = "Combustível Veículo",
    quantidade = "Qtd. Veículos"
  ) %>%
  mutate(quantidade_mil = quantidade / 1000) %>%
  filter(UF == "RIO DE JANEIRO" | UF == "MINAS GERAIS" |
    UF == "SAO PAULO" | UF == "ESPIRITO SANTO") %>%
  group_by(UF) %>%
  summarize(t_veiculos = sum(quantidade_mil))
```


## Gráficos de barras usando ggplot2


```r
## Criar um gráfico de barras de número de veículos por Estado do SE
ggplot(frota_SE, aes(x = UF, y = t_veiculos)) +
  geom_bar(stat = "identity")
```

<img src="imagens/Aula01/grafico_barra1-1.png" width="576" />

Veja que para informar o número total de carros nos estados tivemos de usar `stat="identity"`. Caso não tivessemos usado isso o que teríamos era a contagem de registros. 

## Melhorando a apresentação


```r
ggplot(frota_SE, aes(x = UF, y = t_veiculos)) +
  geom_bar(stat = "identity") +
  theme_light() +
  labs(
    x = "Estados do Sudeste",
    y = "Quant. Veículos (Mil)",
    title = "Total de Veículos no SE"
  )
```

<img src="imagens/Aula01/grafico_barra2-1.png" width="576" />

