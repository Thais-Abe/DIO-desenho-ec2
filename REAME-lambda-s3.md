# ☁️ Tarefas Automatizadas com S3 e Lambda

Projeto: upload de arquivos com processamento e registro no DynamoDB

## 📋 Descrição do Projeto
Este projeto demonstra uma arquitetura **serverless** utilizando **AWS Lambda**, **Amazon S3**, **DynamoDB** e **API Gateway**, com o ambiente **LocalStack** para simular os serviços localmente.  

O objetivo é automatizar o processamento de arquivos enviados a um bucket S3, armazenar os dados no DynamoDB e disponibilizá-los por meio de uma API REST.

---

## ⚙️ Arquitetura da Solução

### 🔁 Fluxo do Processo
1. **Upload no S3**  
   O usuário faz o upload de um arquivo (por exemplo, um `.csv` ou `.json`) para um bucket do **Amazon S3**.

2. **Disparo de Evento (S3 → Lambda)**  
   O evento de criação do objeto (`s3:ObjectCreated:*`) aciona uma **função Lambda** escrita em **Python**.

3. **Processamento e Armazenamento**  
   A função Lambda lê o conteúdo do arquivo, realiza o processamento necessário e grava os dados estruturados em uma **tabela DynamoDB**.

4. **Exposição via API Gateway**  
   Outra função **Lambda** é acionada pelo **API Gateway**, consulta o **DynamoDB** e retorna os dados via endpoint HTTP.

---

## 🧩 Componentes AWS Utilizados

| Serviço | Função |
|----------|--------|
| **Amazon S3** | Armazenamento dos arquivos enviados pelo usuário |
| **AWS Lambda** | Processamento automático e integração entre serviços |
| **Amazon DynamoDB** | Banco de dados NoSQL para persistência dos dados processados |
| **Amazon API Gateway** | Exposição dos dados por meio de uma API REST |
| **LocalStack** | Simulação local dos serviços AWS |

---

## 🧱 Arquitetura Visual

```plaintext
[Usuário] 
   ↓ Upload
[S3 Bucket] 
   ↓ Evento (ObjectCreated)
[Lambda Processadora] 
   ↓
[DynamoDB Table]
   ↑
[Lambda Leitura + API Gateway]
   ↑
[Cliente HTTP / Browser / Postman]
