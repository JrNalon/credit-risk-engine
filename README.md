# 🏦 Projeto: Credit Lifecycle Engine

> **Construindo um Motor de Decisão de Crédito "End-to-End" no Databricks**

**Autor:** Clovis R. Nalon Junior

**Tech Stack:** Databricks, PySpark, Delta Lake, Machine Learning (Random Forest & GBT), Git & GitHub, Business Analytics.

---

## 1. Estrutura do Projeto (Navegação)
O projeto foi desenvolvido simulando um pipeline de produção real, organizado em camadas (Medallion):

* **01_Ingestao_Bronze:** Conexão com fonte de dados, leitura bruta e salvamento em Delta Lake.
* **02_EDA_Analise_Exploratoria:** Análise estatística (ex: Saldo vs. Inadimplência).
* **03_Feature_Engineering_Silver:** Limpeza, tradução e criação de variáveis (`monthly_installment`).
* **04_Model_Training_Gold:** Treinamento do modelo produtivo (Random Forest) com balanceamento de classes.
* **05_Business_Strategy_NBO:** Motor de decisão (Aprovar/Negar), ofertas (NBO) e análise de viés.
* **06_Challenge_Models (Lab):** Ambiente de testes comparando algoritmos (Random Forest vs. GBT).

---

## 2. Arquitetura da Solução (Lakehouse)
Utilizei a arquitetura **Medallion** para governança de dados:
* **Bronze:** Dados crus ingeridos e salvos em Delta Lake (Transações ACID).
* **Silver:** Dados refinados. Tradução de códigos ("A11" -> "Conta Negativa") e indexação numérica.
* **Gold:** Dados enriquecidos com scores de risco e decisões de negócio prontos para consumo.

---

## 3. Destaques Técnicos

### 🛠️ Engenharia de Features (Silver)
* **Nova Variável:** Calculei a parcela mensal (`credit_amount / duration`) para medir a capacidade de pagamento real, indo além do valor total da dívida.
* **Encoding:** Uso de `StringIndexer` para tratar variáveis categóricas.

### 🤖 Modelagem: O Campeão (Gold)
* **Modelo:** Random Forest Classifier.
* **Estratégia:** Aplicação de *Class Weighting* (Peso ~2.3x para erros na classe de inadimplentes) para corrigir o desbalanceamento da base.
* **Resultado:** AUC de **~0.746**.
* **Insight:** A variável `checking_status` (Saldo) superou `credit_amount` em importância. O comportamento dita o risco.

### 🥊 Desafio Champion vs. Challenger (Lab)
No laboratório, o Random Forest (0.746) superou o Gradient Boosted Trees (0.737), provando ser mais robusto e estável para este volume de dados, evitando *overfitting*.

---

## 4. Impacto de Negócio & Estratégia (NBO/NBA)

### Régua de Decisão Automatizada
* **Score < 0.3 (VIP):** Aprovação Imediata + Oferta Platinum (**NBO**).
* **Score > 0.6 (Alto Risco):** Negar crédito ou exigir Avalista (**NBA**).

### 💰 Resultado Financeiro
> Em simulação com dados de teste, o modelo evitou **74%** das perdas potenciais com inadimplência comparado a um cenário sem crivo de risco.

---

## 5. Análise de Erros (Bias)
A análise de Falsos Negativos revelou uma concentração de erros em **Jovens (< 25 anos)**.
* **Ação Futura:** Implementar política de crédito híbrida com dados de Open Finance para este público, compensando a falta de histórico bancário.

---
## Destaques de Engenharia de Dados

Este projeto também demonstra conceitos importantes de Engenharia de Dados, incluindo:

- Arquitetura Lakehouse utilizando abordagem Medallion
- Camadas Bronze, Silver e Gold
- Ingestão e transformação de dados com PySpark
- Tabelas Delta Lake
- Separação entre dados brutos, refinados e analíticos
- Camada analítica preparada para Machine Learning e insights de negócio
- Organização modular dos notebooks
- Versionamento com GitHub

---

## ⚙️ Engenharia & Reprodução
Este projeto foi desenvolvido utilizando boas práticas de **CI/CD** e versionamento:
* **Ambiente:** Databricks Free Edition (Runtime 12.2 LTS ou superior).
* **Versionamento:** Integrado via Databricks Git Folders (Repos).
* **Como rodar:** Clone este repositório no seu workspace do Databricks e execute os notebooks na ordem numérica (01 a 06).

---

## 📁 Fonte de Dados
Este projeto utilizou o dataset público **German Credit Data**, disponibilizado pelo *UCI Machine Learning Repository* e acessado via Kaggle.
* **Dataset:** German Credit Risk (Benchmark Acadêmico).
* **Conteúdo:** Dados anonimizados de perfil demográfico, financeiro e histórico de pagamentos.
