# Agendamento de Start/Stop de RDS com AWS Scheduler

Este guia ensina como agendar a **ligação e desligamento de uma instância RDS** usando o **AWS Scheduler**, que é uma evolução das regras do EventBridge.

## 📌 Pré-requisitos

- Ter uma instância RDS padrão (não Aurora Serverless).
- Ter permissões para criar roles IAM, instâncias de Scheduler e editar recursos RDS.

---

## 🔧 1. Criar a IAM Role que o AWS Scheduler usará

Essa role permite que o AWS Scheduler execute a ação `StartDBInstance` e `StopDBInstance` na sua instância RDS.

### ✅ 1.1 Trust Policy (confiança)

Salve como `trust-policy.json`:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "scheduler.amazonaws.com"
      },
      "Action": "sts:AssumeRole",
      "Condition": {
        "StringEquals": {
          "aws:SourceAccount": "AWS_ACCOUNT_ID_NUMBER"
        }
      }
    }
  ]
}
```json

### ✅ 1.2 Policy de Permissão 

Salve como `rds-policy.json`:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "rds:StartDBInstance",
        "rds:StopDBInstance"
      ],
      "Resource": "arn:aws:rds:AWS_REGION:AWS_ACCOUNT_ID:db/NOME_DO_BANCO"
    }
  ]
}


