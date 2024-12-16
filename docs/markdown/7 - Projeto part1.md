# Documentação nível 1 - Coletando Informações Básicas de um Lead no Agente Imobiliário Automatizado

## **Explicação**

O primeiro passo no fluxo automatizado do Agente Imobiliário Automatizado é coletar e armazenar as informações básicas de leads. Esse processo é crucial para organizar os dados iniciais, que servirão como base para os próximos passos de qualificação e interação com os corretores.

## **Fluxo de Trabalho**

### **1. Criação do Fluxo no N8n**

O primeiro passo é criar um fluxo automatizado no N8n para coletar os dados do lead via um Webhook.

### **1.1. Criando um Novo Fluxo**

1. Acesse o N8n no seu navegador (`http://localhost:5678`).
2. Clique em **"New Workflow"** para criar um novo fluxo.

### **1.2. Adicionando o Nó de Webhook**

1. Clique no botão **"+"** (Adicionar Nó).
2. Busque e selecione o nó **Webhook**.
3. Na configuração do Webhook:
    - Em **HTTP Method**, selecione **POST**.
    - Em **Path**, insira um caminho para o Webhook, por exemplo, `/lead-form`.
4. Salve o fluxo.

O Webhook agora estará disponível para receber dados via requisições HTTP POST.

### **2. Testando o Webhook**

### **2.1. Obter o URL do Webhook**

Após salvar o fluxo, o N8n gera um URL único para o Webhook. Este URL será usado para enviar os dados do lead.

Como ficou a minha URL

```bash
http://localhost:5678/webhook-test/lead-form
```

### **2.2. Enviando Dados para o Webhook**

Para testar o Webhook, você pode usar ferramentas como o **Postman** para enviar uma requisição HTTP POST com os dados do lead.

Exemplo de corpo (body) da requisição POST em formato JSON:

```json
{
  "name": "João Silva",
  "email": "joao.silva@email.com",
  "property_type": "Apartamento"
}
```

Envie esses dados para o URL do Webhook que foi gerado no N8n.

### **3. Armazenamento das Informações no Banco de Dados**

### **3.1. Adicionando o Nó de Banco de Dados (PostgreSQL)**

1. Após o Webhook, adicione um nó **PostgreSQL**.
2. Clique no botão **"+"** e busque por **PostgreSQL**.
3. Selecione o nó e configure a conexão com o banco de dados PostgreSQL.
    - **Host**: Para conexões locais, use `host.docker.internal` (caso o banco de dados esteja rodando na sua máquina local e o N8n esteja no Docker ou similar). Caso contrário, utilize `localhost` ou `127.0.0.1`.
    - **Porta**: A porta padrão do PostgreSQL é **5432**. Caso tenha configurado outra porta, insira-a aqui.
    - **Usuário**: Informe o nome de usuário utilizado para acessar o banco de dados (ex: `n8nuser`).
    - **Senha**: No meu caso `rootn8n`
    - **Banco de dados**: Especifique o nome do banco de dados onde os dados serão armazenados, como `n8n` (ou outro banco que você tenha configurado).
4. Após preencher as informações, clique em **Testar Conexão** para verificar se tudo está configurado corretamente.
5. Se a conexão for bem-sucedida, você poderá continuar configurando o fluxo para inserir os dados na tabela do banco de dados.

### **3.2. Criando a Tabela no PostgreSQL**

Antes de inserir os dados, certifique-se de que a tabela para armazenar os leads já está criada no banco de dados. Se não estiver, crie a tabela `leads` com a seguinte estrutura SQL:

```sql
CREATE TABLE leads (
  id SERIAL PRIMARY KEY,
  name VARCHAR(255),
  email VARCHAR(255),
  property_type VARCHAR(255)
);
```

### **Passo extra: Confirmação se a Tabela Foi Criada:**

### **1. Conectando ao PostgreSQL no PowerShell (Windows)**

Para se conectar ao PostgreSQL usando o PowerShell no Windows com o usuário `n8nuser`, utilize o seguinte comando:

```bash
psql -h localhost -U n8nuser -d n8n
```

Este comando conecta ao banco de dados `n8n` utilizando o usuário `n8nuser`. Quando solicitado, insira a senha para autenticação.

### **2. Comandos para Listar Tabelas e Verificar a Estrutura da Tabela no PostgreSQL**

1. **Listar Todas as Tabelas**:
    
    Para listar todas as tabelas no banco de dados, use o comando:
    
    ```sql
    \dt
    ```
    
    Isso exibirá todas as tabelas disponíveis no banco de dados atual.
    
2. **Verificar a Estrutura de uma Tabela**:
    
    Para verificar as colunas e tipos de dados de uma tabela específica (como a tabela `leads`), use o comando:
    
    ```sql
    \d leads
    ```
    
    Esse comando mostrará as colunas da tabela `leads`, incluindo seus tipos de dados, se são `NOT NULL`, e outras informações de estrutura.
    

### **3.3. Inserindo Dados no Banco de Dados**

1. **Após criar a tabela**, configure o nó **PostgreSQL** para inserir os dados no banco de dados.
2. **Escolha o tipo de operação** **Insert**.
3. **Selecione a tabela** onde os dados serão armazenados (exemplo: `leads`).
4. **Mapeie os campos recebidos pelo Webhook para os campos da tabela no banco de dados**, **exceto o campo `id`**, que será gerado automaticamente pelo PostgreSQL:
    - `name` → Nome do lead
    - `email` → E-mail do lead
    - `property_type` → Tipo de imóvel procurado
5. O campo `id` será automaticamente preenchido pelo PostgreSQL, pois está configurado como `SERIAL` e não precisa ser enviado no corpo da requisição.

### **4. Testando o Fluxo Completo**

### **4.1. Executando o Fluxo no N8n**

1. Após configurar todos os nós, clique em **Execute Workflow** para rodar o fluxo.
2. Verifique se o fluxo está funcionando corretamente, monitorando a execução do fluxo no N8n.

### **4.2. Verificando os Dados no Banco de Dados**

1. Após enviar os dados do lead para o Webhook, execute a consulta SQL para verificar se os dados foram armazenados corretamente no banco de dados:

```sql
SELECT * FROM leads;
```

Se tudo estiver correto, você verá os dados do lead armazenados na tabela `leads`.