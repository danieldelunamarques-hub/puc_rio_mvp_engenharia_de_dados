# Índice de Desenvolvimento Social (IDS) - Belo Horizonte (Censo 2022)

## Visão Geral do Projeto
Este repositório contém o pipeline de Engenharia de Dados desenvolvido para o cálculo e a análise do Índice de Desenvolvimento Social (IDS) no município de Belo Horizonte (MG) e em seus recortes intramunicipais (setores censitários, bairros e favelas/comunidades urbanas). O projeto usa da metodologia concebida pelo Instituto Pereira Passos (IPP) para servir como um instrumento de diagnóstico municipal voltado ao planejamento governamental e à alocação prioritária e equitativa de recursos públicos em territórios de maior vulnerabilidade.

## Fonte de Dados
Os dados de entrada provêm dos microdados e agregados oficiais do Censo Demográfico de 2022, produzidos e disponibilizados publicamente pelo Instituto Brasileiro de Geografia e Estatística (IBGE) sob licença de dados abertos governamentais. Foram integrados arquivos temáticos referentes a cadastros básicos, infraestrutura de domicílios, índices de alfabetização e rendimento nominal médio mensal dos responsáveis.

## Arquitetura e Tecnologias
O pipeline foi desenvolvido de ponta a ponta em ambiente de nuvem na plataforma Databricks, utilizando PySpark para o processamento distribuído de extração e transformação e SQL para consultas analíticas. A organização dos dados segue a Arquitetura Medalhão, estruturada em tabelas Delta gerenciadas via Unity Catalog:

* **Camada Bronze (Ingestão):** Os arquivos originais em formato CSV foram armazenados no Unity Catalog Volumes e persistidos como tabelas Delta brutas, preservando o histórico integral dos dados de origem.
* **Camada Silver (Limpeza e Qualidade):** Execução da filtragem territorial para Belo Horizonte e padronização da chave primária geográfica (CD_SETOR). Contempla o tratamento de anomalias referentes ao sigilo estatístico do IBGE (presença do caractere "X" em colunas quantitativas), convertendo-os em valores nulos e aplicando tipagem estrita (*cast* para inteiros e *floats*).
* **Camada Gold (Modelagem Analítica):** Consolidação dos dados em formato desnormalizado e agregado em quatro granulometrias territoriais (setores censitários, bairros, favelas e totalidade do município). Esta camada implementa as regras de negócio para as Cestas de Variáveis, o tratamento de nulos para zero, o cálculo dos seis indicadores temáticos, a normalização contínua com limites globais de 0 a 1 (com inversão metodológica do indicador de analfabetismo) e a apuração da pontuação final do IDS.

## Catalogação de Dados
A catalogação e a rastreabilidade do ecossistema foram implementadas nativamente no Unity Catalog. O catálogo registra os metadados com descrições semânticas em nível de tabela e de coluna, além de tags de classificação ("Censo Demográfico 2022" e "IDS").
