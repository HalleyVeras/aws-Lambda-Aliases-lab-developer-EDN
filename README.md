# 🧪 Lab: AWS Lambda com Aliases + API Gateway com Stages

> 🌩️ Gerencie múltiplos ambientes serverless com elegância, controle e boas práticas de DevOps na AWS.

---

## 🎯 Objetivo

Este laboratório demonstra como criar uma arquitetura com **versionamento e ambientes isolados** usando:

- AWS Lambda + Aliases (`dev`, `prod`)
- API Gateway + Stages (`Desenvolvimento`, `Producao`)

---

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

📸 *[Adicione aqui o print da criação da função]*

---

### ✏️ Parte 2: Código da Função Lambda (Ambiente DEV)

1. Vá até a seção **Código**.
2. Substitua o conteúdo pelo código de desenvolvimento:

📥 [Download Código Desenvolvimento (.py)](https://bkt-lab-turmas.s3.us-east-1.amazonaws.com/C%C3%B3d.+py+do+ambiente+de+DESENVOLVIMENTO.txt)

3. Clique em **Deploy**.

---

### 🧪 Parte 3: Teste da Função `$LATEST`

1. Vá para a aba **Testar**.
2. Crie novo evento `teste-simulado`.
3. Modelo: `API Gateway AWS Proxy`.
4. Altere `requestContext.stage` para `"test-stage"`.
5. Clique em **Testar** e verifique a resposta.

📸 *[Adicione print da execução com sucesso]*

---

### 🧬 Parte 4: Versionamento e Aliases

#### 📌 Publicar Versão 1

1. Acesse aba **Versões** > `Publicar nova versão`.
2. Descrição: `Versão inicial`.

#### 🏷️ Criar Alias `dev`

1. Vá na aba **Aliases** > `Criar alias`.
2. Nome: `dev` | Versão: `1`
3. Copie o ARN e guarde.

📸 *[Adicione print do alias dev]*

---

### 🔁 Parte 5: Código para Produção

1. Substitua o código por:

📥 [Download Código Produção (.py)](https://bkt-lab-turmas.s3.us-east-1.amazonaws.com/C%C3%B3d.+py+do+ambiente+de+PRODU%C3%87%C3%83O.txt)

2. Clique em **Deploy**.
3. Teste novamente com `teste-simulado`.

---

#### 📌 Publicar Versão 2 e Alias `prod`

1. Publique nova versão → descrição: `Produção`.
2. Criar alias: Nome `prod`, Versão `2`.
3. Copie o ARN.

📸 *[Adicione print do alias prod]*

---

## 🌐 Parte 6: API Gateway + Integração

### 🛠️ Criar API REST

1. Acesse o serviço **API Gateway**.
2. Clique em **Criar API** → API REST (compilar).
3. Nome: `minha-api-proxy-lab-seunome`
4. Tipo: Regional → Criar API

---

### 📍 Criar Recurso `/hello`

1. Em **Recursos** → Criar Recurso.
2. Nome do recurso: `hello`

---

### 🔗 Integrar Método GET (ambiente dev)

1. No recurso `/hello`, clique em **Criar método** > `GET`.
2. Integração: **Função Lambda (proxy)**
3. ARN: cole o ARN do alias `dev`.
4. Clique em **Salvar e Testar**.

---

### 🚀 Implantar Estágio `Desenvolvimento`

1. Clique em **Implantar API** > Novo estágio: `Desenvolvimento`
2. Descrição: `API/Lambda - Desenvolvimento`

📸 *[Adicione print do estágio dev]*

---

### 🔁 Trocar para Alias `prod` e Implantar Produção

1. Edite o método GET > integração > troque para ARN do alias `prod`.
2. Implemente novo estágio: `Producao`.

📸 *[Adicione print do estágio prod]*

---

## 🔍 Parte 7: Testes Finais

1. Acesse o método `GET` em **Estágios** > `Desenvolvimento`.
2. Copie a **Invoke URL** e teste no navegador.

🔗 Exemplo: `https://xyz123.execute-api.us-east-1.amazonaws.com/Desenvolvimento/hello`

3. Repita o teste para o estágio `Producao`.

📸 *[Adicione prints de ambos os resultados]*

---

## 🧹 Parte 8: Limpeza

1. Exclua a API Gateway criada.
2. Exclua a Função Lambda.
3. Pronto! 😎

---

## 💬 Conclusão

> Agora você sabe criar pipelines simples e eficazes com **Lambda + API Gateway**, organizando ambientes isolados e aplicando versionamento na AWS como um verdadeiro profissional da nuvem!

---

## 💡 Citação para Inspirar

> “A melhor forma de prever o futuro é criá-lo.” – *Alan Kay*

---

📌 _Gostou do projeto? Deixe um ⭐ no repositório e compartilhe com sua rede!_

