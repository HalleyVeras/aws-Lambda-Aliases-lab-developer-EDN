# 🧪 Lab: AWS Lambda com Aliases + API Gateway com Stages

![1](https://raw.githubusercontent.com/HalleyVeras/aws-Lambda-Aliases-lab-developer-EDN/refs/heads/main/arquivos/Edn_Lambda_API.jpg)

> 🌩️ Gerencie múltiplos ambientes serverless com elegância, controle e boas práticas de DevOps na AWS.

---

**Autor:** Halley Veras  
**Curso: Developer – Escola da Nuvem** 

## 🎯 Objetivo

Este laboratório demonstra como criar uma arquitetura com **versionamento e ambientes isolados** usando:

- AWS Lambda + Aliases (`dev`, `prod`)
- API Gateway + Stages (`Desenvolvimento`, `Producao`)

---

## 🧰 Pré-requisitos

- Conta ativa na AWS.
- Permissões para Lambda e API Gateway (IAM).
- Acesso ao [AWS Console](https://console.aws.amazon.com/).

---

## 🛠️ Etapas do Laboratório

### ✅ Parte 1: Criar a Função Lambda

1. Acesse o serviço **Lambda**.
2. Clique em **Criar função** → Criar do zero.
3. Nome da função: `minha-funcao-proxy-lab-seunome`
4. Runtime: `Python 3.9`
5. Arquitetura: `x86_64`
6. Permissões: Criar nova função com permissões básicas.
7. Clique em **Criar função**.

![1](https://raw.githubusercontent.com/HalleyVeras/aws-Lambda-Aliases-lab-developer-EDN/refs/heads/main/arquivos/2025-06-25_17-24.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-Lambda-Aliases-lab-developer-EDN/refs/heads/main/arquivos/2025-06-25_17-24_1.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-Lambda-Aliases-lab-developer-EDN/refs/heads/main/arquivos/2025-06-25_17-29.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-Lambda-Aliases-lab-developer-EDN/refs/heads/main/arquivos/2025-06-25_17-30.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-Lambda-Aliases-lab-developer-EDN/refs/heads/main/arquivos/2025-06-25_17-31.png)


---

### ✏️ Parte 2: Código da Função Lambda (Ambiente DEV)

1. Vá até a seção **Código**.
2. Substitua o conteúdo pelo código de desenvolvimento:

```python
    import json
    import os
    
    def lambda_handler(event, context):
        try:
            stage = event['requestContext']['stage']
        except KeyError:
            stage = 'desconhecido'
    
        response_body = {
            'message': f'Ola do ambiente {stage}!',
            'functionVersion': context.function_version,
            'functionAlias': (
                context.invoked_function_arn.split(':')[-1]
                if ':' in context.invoked_function_arn else '$LATEST'
            )
        }
    
        return {
            'statusCode': 200,
            'headers': {
                'Content-Type': 'application/json',
                'Access-Control-Allow-Origin': '*'
            },
            'body': json.dumps(response_body, ensure_ascii=False)
        }### Features
```

3. Clique em **Deploy**.

![1](https://raw.githubusercontent.com/HalleyVeras/aws-Lambda-Aliases-lab-developer-EDN/refs/heads/main/arquivos/2025-06-25_17-34.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-Lambda-Aliases-lab-developer-EDN/refs/heads/main/arquivos/2025-06-25_17-35.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-Lambda-Aliases-lab-developer-EDN/refs/heads/main/arquivos/2025-06-25_17-36.png)

---

### 🧪 Parte 3: Teste da Função `$LATEST`

1. Vá para a aba **Testar**.
2. Crie novo evento `teste-simulado`.
3. Modelo: `API Gateway AWS Proxy`.
4. Altere `requestContext.stage` para `"test-stage"`.
5. Clique em **Testar** e verifique a resposta.

![1](https://raw.githubusercontent.com/HalleyVeras/aws-Lambda-Aliases-lab-developer-EDN/refs/heads/main/arquivos/2025-06-25_17-39.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-Lambda-Aliases-lab-developer-EDN/refs/heads/main/arquivos/2025-06-25_17-40.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-Lambda-Aliases-lab-developer-EDN/refs/heads/main/arquivos/2025-06-25_17-41.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-Lambda-Aliases-lab-developer-EDN/refs/heads/main/arquivos/2025-06-25_17-42.png)

---

### 🧬 Parte 4: Versionamento e Aliases

#### 📌 Publicar Versão 1

1. Acesse aba **Versões** > `Publicar nova versão`.
2. Descrição: `Versão inicial`.

#### 🏷️ Criar Alias `dev`

1. Vá na aba **Aliases** > `Criar alias`.
2. Nome: `dev` | Versão: `1`
3. Copie o ARN e guarde.

![1](https://raw.githubusercontent.com/HalleyVeras/aws-Lambda-Aliases-lab-developer-EDN/refs/heads/main/arquivos/2025-06-25_17-43.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-Lambda-Aliases-lab-developer-EDN/refs/heads/main/arquivos/2025-06-25_17-47.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-Lambda-Aliases-lab-developer-EDN/refs/heads/main/arquivos/2025-06-25_17-48.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-Lambda-Aliases-lab-developer-EDN/refs/heads/main/arquivos/2025-06-25_17-49.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-Lambda-Aliases-lab-developer-EDN/refs/heads/main/arquivos/2025-06-25_17-50.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-Lambda-Aliases-lab-developer-EDN/refs/heads/main/arquivos/2025-06-25_17-50_1.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-Lambda-Aliases-lab-developer-EDN/refs/heads/main/arquivos/2025-06-25_17-52.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-Lambda-Aliases-lab-developer-EDN/refs/heads/main/arquivos/2025-06-25_17-53.png)

---

### 🔁 Parte 5: Código para Produção

1. Substitua o código por:

```python
import json
import os

def lambda_handler(event, context):
    try:
        stage = event['requestContext']['stage']
    except KeyError:
        stage = 'desconhecido'

    response_body = {
        'message': f'Ola do ambiente de Producao!',
        'functionVersion': context.function_version,
        'functionAlias': (
            context.invoked_function_arn.split(':')[-1]
            if ':' in context.invoked_function_arn else '$LATEST'
        )
    }

    return {
        'statusCode': 200,
        'headers': {
            'Content-Type': 'application/json',
            'Access-Control-Allow-Origin': '*'
        },
        'body': json.dumps(response_body, ensure_ascii=False)
    }

```

2. Clique em **Deploy**.
3. Teste novamente com `teste-simulado`.

![1](https://raw.githubusercontent.com/HalleyVeras/aws-Lambda-Aliases-lab-developer-EDN/refs/heads/main/arquivos/2025-06-25_17-59.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-Lambda-Aliases-lab-developer-EDN/refs/heads/main/arquivos/2025-06-25_18-00.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-Lambda-Aliases-lab-developer-EDN/refs/heads/main/arquivos/2025-06-25_18-01.png)

---

#### 📌 Publicar Versão 2 e Alias `prod`

1. Publique nova versão → descrição: `Produção`.
2. Criar alias: Nome `prod`, Versão `2`.
3. Copie o ARN.

![1](https://raw.githubusercontent.com/HalleyVeras/aws-Lambda-Aliases-lab-developer-EDN/refs/heads/main/arquivos/2025-06-25_18-01_1.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-Lambda-Aliases-lab-developer-EDN/refs/heads/main/arquivos/2025-06-25_18-02.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-Lambda-Aliases-lab-developer-EDN/refs/heads/main/arquivos/2025-06-25_18-02_1.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-Lambda-Aliases-lab-developer-EDN/refs/heads/main/arquivos/2025-06-25_18-03.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-Lambda-Aliases-lab-developer-EDN/refs/heads/main/arquivos/2025-06-25_18-04.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-Lambda-Aliases-lab-developer-EDN/refs/heads/main/arquivos/2025-06-25_18-05.png)

---

## 🌐 Parte 6: API Gateway + Integração

### 🛠️ Criar API REST

1. Acesse o serviço **API Gateway**.
2. Clique em **Criar API** → API REST (compilar).
3. Nome: `minha-api-proxy-lab-seunome`
4. Tipo: Regional → Criar API

![1](https://raw.githubusercontent.com/HalleyVeras/aws-Lambda-Aliases-lab-developer-EDN/refs/heads/main/arquivos/2025-06-25_18-07.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-Lambda-Aliases-lab-developer-EDN/refs/heads/main/arquivos/2025-06-25_18-18.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-Lambda-Aliases-lab-developer-EDN/refs/heads/main/arquivos/2025-06-25_18-20.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-Lambda-Aliases-lab-developer-EDN/refs/heads/main/arquivos/2025-06-25_18-23.png)


---

### 📍 Criar Recurso `/hello`

1. Em **Recursos** → Criar Recurso.
2. Nome do recurso: `hello`

![1](https://raw.githubusercontent.com/HalleyVeras/aws-Lambda-Aliases-lab-developer-EDN/refs/heads/main/arquivos/2025-06-25_18-23_1.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-Lambda-Aliases-lab-developer-EDN/refs/heads/main/arquivos/2025-06-25_18-25.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-Lambda-Aliases-lab-developer-EDN/refs/heads/main/arquivos/2025-06-25_18-25_1.png)

---

### 🔗 Integrar Método GET (ambiente dev)

1. No recurso `/hello`, clique em **Criar método** > `GET`.
2. Integração: **Função Lambda (proxy)**
3. ARN: cole o ARN do alias `dev`.
4. Clique em **Salvar e Testar**.

![1](https://raw.githubusercontent.com/HalleyVeras/aws-Lambda-Aliases-lab-developer-EDN/refs/heads/main/arquivos/2025-06-25_18-27.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-Lambda-Aliases-lab-developer-EDN/refs/heads/main/arquivos/2025-06-25_18-29.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-Lambda-Aliases-lab-developer-EDN/refs/heads/main/arquivos/2025-06-25_18-30.png)

---

### 🚀 Implantar Estágio `Desenvolvimento`

1. Clique em **Implantar API** > Novo estágio: `Desenvolvimento`
2. Descrição: `API/Lambda - Desenvolvimento`

![1](https://raw.githubusercontent.com/HalleyVeras/aws-Lambda-Aliases-lab-developer-EDN/refs/heads/main/arquivos/2025-06-25_18-32.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-Lambda-Aliases-lab-developer-EDN/refs/heads/main/arquivos/2025-06-25_18-33.png)

---

### 🔁 Trocar para Alias `prod` e Implantar Produção

1. Edite o método GET > integração > troque para ARN do alias `prod`.
2. Implemente novo estágio: `Producao`.

![1](https://raw.githubusercontent.com/HalleyVeras/aws-Lambda-Aliases-lab-developer-EDN/refs/heads/main/arquivos/2025-06-25_18-35.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-Lambda-Aliases-lab-developer-EDN/refs/heads/main/arquivos/2025-06-25_18-36.png)

---

## 🔍 Parte 7: Testes Finais

1. Acesse o método `GET` em **Estágios** > `Desenvolvimento`.
2. Copie a **Invoke URL** e teste no navegador.

🔗 Exemplo: `https://xyz123.execute-api.us-east-1.amazonaws.com/Desenvolvimento/hello`

3. Repita o teste para o estágio `Producao`.

![1](https://raw.githubusercontent.com/HalleyVeras/aws-Lambda-Aliases-lab-developer-EDN/refs/heads/main/arquivos/2025-06-25_18-36_1.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-Lambda-Aliases-lab-developer-EDN/refs/heads/main/arquivos/2025-06-25_18-37.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-Lambda-Aliases-lab-developer-EDN/refs/heads/main/arquivos/2025-06-25_18-38.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-Lambda-Aliases-lab-developer-EDN/refs/heads/main/arquivos/2025-06-25_18-38_1.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-Lambda-Aliases-lab-developer-EDN/refs/heads/main/arquivos/2025-06-25_18-39.png)
![1](https://raw.githubusercontent.com/HalleyVeras/aws-Lambda-Aliases-lab-developer-EDN/refs/heads/main/arquivos/2025-06-25_18-39_1.png)


---



## 💬 Conclusão

> Agora você sabe criar pipelines simples e eficazes com **Lambda + API Gateway**, organizando ambientes isolados e aplicando versionamento na AWS como um verdadeiro profissional da nuvem!

---

## 💡 Citação para Inspirar

> “A melhor forma de prever o futuro é criá-lo.” – *Alan Kay*

---

📌 _Gostou do projeto? Deixe um ⭐ no repositório e compartilhe com sua rede!_

