# Como criar o Banco de Dados `n8n` e um Usuário para Gerenciá-lo no Postegres via DBeaver

![111.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/b0b7c39d-7a1c-487f-a458-46f2d389b76c/759492f1-34e9-481a-bcfb-b87074e35f62/111.png)

## **Tutorial passo a passo**

### **Passo 1: Conectar ao PostgreSQL no DBeaver**

1. Abra o **DBeaver**.
2. Clique no ícone de **Nova Conexão** (ou vá para `Arquivo > Nova Conexão`).
3. Escolha **PostgreSQL** na lista de bancos de dados e clique em **Avançar**.
4. Insira as credenciais do seu PostgreSQL:
    - **Host**: `localhost`
    - **Porta**: `5432` (ou a porta que o PostgreSQL está configurado)
    - **Usuário**: `postgres`
    - **Senha**: `root`
5. Clique em **Testar Conexão**. Certifique-se de que a conexão foi bem-sucedida.
6. Clique em **Finalizar**.

<br>

### **Passo 2: Criar o Banco de Dados `n8n`**

1. Na aba de conexões, clique com o botão direito na conexão do PostgreSQL (`localhost`).
2. Escolha **SQL Editor > Nova Janela SQL**.
3. Execute o seguinte comando para criar o banco de dados:
    
    ```sql
    CREATE DATABASE n8n;
    ```
    
4. Clique no ícone de **Executar** (ou pressione `Ctrl+Enter` no Windows). Você verá uma mensagem confirmando a criação.
5. Para confirmar a criação do banco de dados, execute o comando abaixo:
    
    ```sql
    SELECT datname FROM pg_database;
    ```
    
    - Verifique na lista retornada se o banco `n8n` está presente.

<br>

### **Passo 3: Criar um Usuário para Gerenciar o Banco**

1. Na janela SQL, execute o seguinte comando para criar o usuário `n8nuser` com a senha `rootn8n` (o usuário será criado somente se ainda não existir):
    
    ```sql
    DO
    $$
    BEGIN
        IF NOT EXISTS (SELECT 1 FROM pg_roles WHERE rolname = 'n8nuser') THEN
            CREATE USER n8nuser WITH PASSWORD 'rootn8n';
        END IF;
    END
    $$;
    ```
    
2. Conceda permissões ao usuário `n8nuser` para acessar e gerenciar o banco de dados `n8n`:
    
    ```sql
    GRANT ALL PRIVILEGES ON DATABASE n8n TO n8nuser;
    ```
    
3. Para verificar se o usuário foi criado corretamente, execute o seguinte comando:
    
    ```sql
    SELECT usename FROM pg_catalog.pg_user;
    ```
    
    - A lista retornada deve conter o usuário `n8nuser`.
4. Para verificar as permissões concedidas ao usuário no esquema `public` do banco de dados `n8n`, execute:
    
    ```sql
    SELECT grantee, table_schema, table_name, privilege_type
    FROM information_schema.role_table_grants
    WHERE grantee = 'n8nuser' AND table_schema = 'public';
    ```
    

<br>

### **Passo 4: Alterar a Conexão para o Banco de Dados `n8n`**

1. Na aba **Database Navigator** (geralmente à esquerda), localize a conexão do PostgreSQL já criada.
2. Clique com o botão direito sobre a conexão e escolha **Editar Conexão**.
3. Na aba **Main**, altere o valor do campo **Database** de `postgres` para `n8n`.
4. Clique em **OK** para salvar as alterações.
5. Reconecte-se à base de dados para confirmar que está utilizando o banco `n8n`.

<br>

### **Passo 5: Testar a Conexão do Novo Usuário no DBeaver**

1. Clique novamente no ícone de **Nova Conexão** no DBeaver.
2. Escolha **PostgreSQL** e clique em **Avançar**.
3. Insira as credenciais do novo usuário:
    - **Usuário**: `n8nuser`
    - **Senha**: `rootn8n`
    - **Database**: `n8n`
4. Clique em **Testar Conexão** para garantir que o novo usuário tem acesso ao banco `n8n`.
5. Clique em **Finalizar**.