# Pipeline de Dados de Transporte de Cargas com Enriquecimento de IA (LogiData-Pipeline)

## 📌 Contexto do Projeto
Em grandes operações logísticas e de transporte, motoristas geram milhares de registros de ocorrências diariamente por meio de texto livre (chat, aplicativos ou sistemas de monitoramento). Centralizar, categorizar e extrair inteligência desses textos de forma manual é ineficiente e propício a erros.

Este projeto simula uma solução de ponta a ponta (**End-to-End**) para o setor de transportes, automatizando a ingestão de dados de viagens, aplicando a **Arquitetura Medalhão** para refino dos dados com **PySpark** no **Databricks**, e utilizando **Inteligência Artificial (Structured Outputs)** para ler os textos de observações e transformá-los em dados altamente estruturados para tomada de decisão.

---

## 🏗️ Arquitetura do Pipeline

O fluxo de dados foi desenhado seguindo as melhores práticas de Engenharia de Dados em Nuvem:

1. **Origem / Ingestão:** Script Python simulando a coleta de dados JSON brutos de uma API de sistema TMS (Transportation Management System).
2. **Camada Bronze (Dados Brutos):** Armazenamento do JSON original exatamente como veio da origem.
3. **Camada Silver (Tratamento e PySpark):** Limpeza de dados, padronização de formatos (timestamps, valores decimais), tratamento de nulos e enriquecimento de colunas utilizando PySpark no Databricks.
4. **Enriquecimento com IA:** Integração (simulada via contrato estrito) com modelos de linguagem (LLMs) aplicando *Structured Outputs* para classificar automaticamente a gravidade, categoria e ação imediata de cada ocorrência.
5. **Camada Gold (Analítica & SQL):** Criação de views agregadas utilizando SQL avançado (Window Functions) para calcular o impacto financeiro de cargas em risco por região de destino.

---

## 🛠️ Tecnologias Utilizadas

* **Python 3:** Ingestão de dados e simulação da API.
* **Apache Spark / PySpark:** Processamento distribuído e transformação dos dados.
* **Databricks:** Ambiente unificado de análise e execução dos notebooks.
* **SQL (Spark SQL):** Consultas analíticas avançadas e criação da camada Gold.
* **Conceitos de IA Aplicada:** Engenharia de prompts e estruturação de dados não estruturados via LLM (*Structured Outputs*).

---

## 📂 Estrutura do Repositório

* `ingestao_bronze.py`: Script responsável por gerar/simular a coleta de dados brutos.
* `Transformacao_Silver_Gold.ipynb`: Notebook do Databricks contendo todo o processo de tratamento PySpark, enriquecimento de IA e consultas SQL.

---

## 📊 Exemplo de Consulta Analítica (Camada Gold)

Para validar a entrega do projeto, foi desenvolvida uma query SQL utilizando **Window Functions (`RANK()`)** para identificar quais regiões demandam atenção imediata da gerência operacional devido ao valor financeiro das cargas retidas em trânsito ou problemas mecânicos:

```sql
SELECT 
    regiao_destino AS Regiao_Foco,
    categoria_ocorrencia_ia AS Categoria_Problema,
    COUNT(id_viagem) AS Total_Ocorrencias,
    SUM(valor_carga) AS Valor_Total_Em_Risco,
    RANK() OVER (ORDER BY SUM(valor_carga) DESC) AS Ranking_Criticidade_Financeira
FROM 
    vw_ocorrencias_enriquecidas
WHERE 
    gravidade_ocorrencia_ia IN ('Alta', 'Media')
GROUP BY 
    regiao_destino, categoria_ocorrencia_ia;


🚀 Como Executar

    Clone este repositório:
    Bash

    git clone https://github.com/kito88/pipeline-dados-tms-ia.git

    Execute o arquivo ingestao_bronze.py localmente para entender a geração do dado bruto.

    Importe o arquivo .ipynb para dentro do seu ambiente Databricks (pode ser na versão Community).

    Execute as células sequencialmente para visualizar a transformação dos dados em tempo real.

✉️ Contato: Francisco Gonçalves - f.goncalves.sp@gmail.com / Linkedin: www.linkedin.com/in/francisco-gonçalves-cloud-aws-devops
