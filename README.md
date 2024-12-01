# Microsoft-AI---Doc-Anti-Fraude
### Análise de Documentos Anti-Fraude com Azure AI
**Projeto de Solução de Análise Automatizada de Documentos Utilizando Azure AI**

#### Visão Geral do Projeto
Este projeto visa implementar uma solução de análise automatizada de documentos utilizando **Azure AI**, com o objetivo de **identificar fraudes, validar a autenticidade** e **aumentar a segurança** de transações empresariais. Dessa forma, busca-se garantir maior **confiabilidade** no processamento de documentos sensíveis e reduzir riscos.

#### Objetivos do Projeto
- **Identificação de Padrões de Fraude**: Utilizar IA para detectar anomalias e inconsistências em documentos.
- **Validação de Autenticidade**: Garantir que documentos submetidos sejam autênticos e não adulterados.
- **Aumento da Segurança**: Implementar um sistema robusto que mitigue fraudes e falsificações.

### Arquitetura Proposta
A solução segue etapas principais para integrar a análise e automação dos processos documentais:

1. **Aquisição de Documentos**: 
   - Documentos são enviados via APIs ou upload para um armazenamento seguro no **Azure Blob Storage**.

2. **Processamento e Extração de Dados**:
   - Uso do **Azure Form Recognizer** para extrair dados estruturados de PDFs, imagens e outros formatos.

3. **Identificação de Padrões de Fraude**:
   - Treinamento de modelos com **Azure Machine Learning** para detectar fraudes e validar autenticidade dos documentos.

4. **Armazenamento de Dados**:
   - Armazenamento dos dados extraídos em **Azure SQL Database** ou **Azure Cosmos DB** para garantir integridade.

5. **Análise e Monitoramento**:
   - Uso de **Power BI** ou **Azure Synapse Analytics** para gerar relatórios e análises sobre documentos processados.

6. **Integração e Automação**:
   - Orquestração com **Azure Logic Apps** ou **Power Automate** para automatizar processos de validação e ações em casos de suspeita.

### Etapas do Projeto
#### 1. Configuração do Azure Form Recognizer
- **Criação do Recurso**: Configure o **Azure Form Recognizer** no portal Azure e escolha o modelo a ser usado (predefinido ou personalizado).
- **Extração de Dados**: Use APIs específicas (Layout, Receipts, Invoices) para extrair dados como datas, valores e nomes.

**Exemplo de Integração com API em Python**:
```python
import requests

endpoint = "<seu-endpoint>"
api_key = "<sua-chave-api>"
document_path = "documento.pdf"
headers = {"Ocp-Apim-Subscription-Key": api_key}

with open(document_path, "rb") as f:
    document_data = f.read()

response = requests.post(f"{endpoint}/formrecognizer/v2.1/layout/analyze", headers=headers, data=document_data)

if response.status_code == 202:
    print("Análise iniciada com sucesso")
else:
    print(f"Erro: {response.text}")
```

#### 2. Identificação de Fraude com Azure Machine Learning
- **Coleta e Organização dos Dados**: Os dados extraídos são armazenados em bancos como **Azure SQL** ou **Cosmos DB**.
- **Treinamento de Modelo de Fraude**: Modelos são treinados para detectar anomalias, utilizando algoritmos como **Random Forest** ou **Redes Neurais** para classificar documentos suspeitos.
- **Implantação**: Após o treinamento, o modelo é implantado em produção usando o **Azure ML Web Service**.

**Exemplo Básico de Treinamento**:
```python
from azureml.core import Workspace, Dataset
from sklearn.ensemble import RandomForestClassifier
from sklearn.model_selection import train_test_split
from sklearn.metrics import classification_report

ws = Workspace.from_config()
dataset = Dataset.get_by_name(ws, 'fraud_dataset')

X = dataset.drop('fraud', axis=1)
y = dataset['fraud']
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3)

model = RandomForestClassifier()
model.fit(X_train, y_train)
predictions = model.predict(X_test)
print(classification_report(y_test, predictions))
```

#### 3. Armazenamento dos Dados
- **Banco de Dados Relacional**: Dados extraídos são armazenados no **Azure SQL Database** para manter estrutura e integridade.

**Estrutura de Tabela SQL**:
```sql
CREATE TABLE Faturas (
    id INT PRIMARY KEY,
    data_emissao DATE,
    valor DECIMAL(10, 2),
    fornecedor VARCHAR(100),
    cliente VARCHAR(100),
    status VARCHAR(50) -- 'Fraudulento' ou 'Autêntico'
);
```

#### 4. Integração com Azure Logic Apps
- **Automação do Fluxo de Trabalho**: **Azure Logic Apps** automatiza o fluxo desde o envio até a resposta a documentos suspeitos, notificando equipes ou bloqueando transações.

#### 5. Análise e Relatórios
- **Dashboards em Power BI**: Relatórios interativos são criados para visualizar a quantidade de documentos analisados e tendências de fraudes.

#### 6. Monitoramento Contínuo e Melhorias
- **Acompanhamento do Desempenho**: Uso do **Azure Monitor** para monitorar a precisão do modelo e fazer ajustes contínuos.
- **Treinamento Contínuo**: Re-treinamento periódico do modelo com novos dados para melhorar a acuracidade.

### Conclusão
Este projeto integra múltiplas ferramentas do Azure para criar uma solução robusta que melhora a segurança no processamento de documentos empresariais. Com o uso de **Azure Form Recognizer** para extração de dados, **Azure Machine Learning** para identificação de fraudes e **Azure Logic Apps** para automação, a solução visa garantir **eficiência**, **escalabilidade** e **segurança**.
