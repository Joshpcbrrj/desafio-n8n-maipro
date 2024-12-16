# **Documentação nível 2 - Qualificando Leads no Agente Imobiliário Automatizado**

## **Explicação**

Após a coleta e armazenamento das informações básicas dos leads, o próximo passo é qualificá-los com base nos dados fornecidos. A qualificação determina o nível de interesse de cada lead e ajuda a identificar os leads mais promissores para o contato com o corretor.

<br>

## **Fluxo de Trabalho**

### **1. Adicionando o Nó de Função (Function Node)**

Antes de salvar os dados no banco de dados, é necessário calcular o nível de interesse do lead com base no campo `property_type`. Esse cálculo será feito por meio de um nó de função que avalia as informações do lead.

### **Passos:**

1. **Adicione o Nó de Função:**
    - No fluxo existente, adicione um nó **Function** logo após o nó que coleta os dados do lead (ex.: Webhook).
2. **Implemente a Lógica de Qualificação:**
    - No nó **Function**, insira o código para qualificar o lead conforme o tipo de imóvel:
    
    ```jsx
    const lead = $json; // Dados do lead capturados do nó anterior
    
    // Desestruturação para remover o campo 'id' sem deletá-lo
    const { id, ...leadWithoutId } = lead;
    
    // Definindo regras para o nível de interesse
    let interestLevel = "Baixo Interesse"; // O valor padrão é Baixo Interesse
    
    if (lead.property_type === "Apartamento") {
        interestLevel = "Médio Interesse";
    } else if (lead.property_type === "Casa" || lead.property_type === "Cobertura") {
        interestLevel = "Alto Interesse";
    }
    
    return {
        json: {
            ...leadWithoutId, // Dados sem o campo 'id'
            interestLevel,    // Adiciona o nível de interesse
        },
    };
    ```
    
3. **Salve o Nó de Função.**

<br>

### **2. Armazenando Leads Qualificados**

Após calcular o nível de interesse, o próximo passo é armazenar os dados dos leads qualificados em uma tabela no banco de dados.

### **2.1. Criando a Tabela `qualified_leads` no PostgreSQL**

Execute a seguinte consulta SQL para criar a tabela `qualified_leads`:

```sql
CREATE TABLE qualified_leads (
    id SERIAL PRIMARY KEY,
    name VARCHAR(255),
    email VARCHAR(255),
    property_type VARCHAR(255),
    interest_level VARCHAR(50)
);
```

### **2.2. Adicionando o Nó PostgreSQL**

Agora que a tabela foi criada, adicione um nó **PostgreSQL** após o nó **Function** para salvar os dados dos leads qualificados.

1. **Selecione a operação `Insert`:** No nó **PostgreSQL**, escolha a operação **Insert**.
2. **Mapeie os Campos:** Mapeie os campos do JSON gerado no nó **Function** para os campos da tabela `qualified_leads`:
    - `name` → Nome do lead
    - `email` → E-mail do lead
    - `property_type` → Tipo de imóvel procurado
    - `interest_level` → Nível de interesse calculado
3. **Salve a Configuração do Nó.**

<br>

### **3.1. Adicionando um Nó Condicional (If Node)**

Após o nó **PostgreSQL**, adicione um nó **If** para verificar o nível de interesse do lead e agir conforme o valor de `interestLevel`. Vamos usar três condições: **Alto Interesse**, **Médio Interesse** e, por padrão, **Baixo Interesse**.

### **Passos:**

1. **Adicione o Nó Condicional:**
    - Após o nó **PostgreSQL**, adicione um nó **If** para avaliar o campo `interestLevel` e verificar se ele corresponde a algum dos três níveis de interesse.
2. **Configuração do Nó If:**
    - O nó **If** terá três condições separadas para tratar cada nível de interesse de forma distinta.
    
    **Exemplo de Condição para "Alto Interesse":**
    
    ```swift
    IF {{ $json.interest_level }} is equal to "Alto Interesse"
        - Ação para "Alto Interesse":
            - Salvar na planilha de clientes com alto interesse
            - Enviar e-mail para o cliente de alto interesse
    ```
    
    **Exemplo de Condição para "Médio Interesse":**
    
    ```swift
    IF {{ $json.interest_level }} is equal to "Médio Interesse"
        - Ação para "Médio Interesse":
            - Salvar na planilha de clientes com médio interesse
    ```
    
    **Caso Não Atenda Nenhuma das Condições Anteriores (Baixo Interesse)**
    
    Caso o lead não atenda a nenhuma das duas primeiras condições, ele será automaticamente classificado como de **Baixo Interesse**.
    

<br>

### **5. Armazenando os Leads nas Planilhas Corretas**

Agora vamos configurar os nós para armazenar os dados dos leads nas planilhas de acordo com seu nível de interesse.

### **5.1. Adicionando Nó do Google Sheets para "Alto Interesse"**

1. **Adicione o Nó Google Sheets:**
    - Após o nó **If** que trata o "Alto Interesse", adicione um nó **Google Sheets**.
    - Selecione a operação **"Append Row"**.
    - Insira o **Planilha ID** e **GID** da aba **"Alto Interesse"**.
    - Mapie os campos do JSON para as colunas da planilha.

### **5.2. Adicionando Nó do Google Sheets para "Médio Interesse"**

Repita o processo para o nó de **Médio Interesse**, mapeando os campos da mesma forma.

### **5.3. Adicionando Nó do Google Sheets para "Baixo Interesse"**

Repita o processo para o nó de **Baixo Interesse**.

<br>

### **6. Enviando E-mails para Leads de Alto Interesse**

Agora, para leads classificados como **Alto Interesse**, configuraremos o envio de e-mails personalizados.

### **6.1. Configurando o Envio de E-mail no n8n**

1. **Adicione o Nó "Send Email":**
    - Após o nó do Google Sheets para "Alto Interesse", adicione o nó **Send Email**.
2. **Configuração de Envio de E-mail:**
    - **Sender Name**: Insira o nome do remetente, como "Imóveis Premium".
    - **Sender Email**: Coloque o endereço de e-mail do remetente.
    - **Recipient Email**: Use `{{$json["email"]}}` para o endereço do lead.
    - **Subject**: Exemplo: "Oferta Especial: Imóveis de Alto Padrão".
    - **Body**:
    
    ```html
    <h3>Olá {{$json["name"]}},</h3>
    <p>Temos ofertas especiais de imóveis de alto padrão que podem ser do seu interesse!</p>
    <p>Entre em contato para mais informações ou agendar uma visita.</p>
    <p>Atenciosamente, <br>Equipe Imóveis Premium</p>
    
    ```
    
3. **Testar o Envio de E-mail:**
    - Após a configuração, clique em **Execute Node** para testar.

### **Passo complementar para criar serviço no Google Cloud**

Antes de poder usar o Google Sheets no n8n, você precisa configurar o serviço no Google Cloud e obter a chave de autenticação JSON. Aqui está um passo a passo simples para isso:

1. **Criação do Projeto no Google Cloud:**
    - Acesse o [Google Cloud Console](https://console.cloud.google.com/), crie um novo projeto e ative a API do Google Sheets e Google Drive.
2. **Criação das Credenciais de Autenticação:**
    - Crie uma conta de serviço e baixe a chave JSON.
3. **Compartilhar a Planilha com a Conta de Serviço:**
    - Abra a planilha no Google Sheets e compartilhe com o e-mail da conta de serviço.

<br>

<br>