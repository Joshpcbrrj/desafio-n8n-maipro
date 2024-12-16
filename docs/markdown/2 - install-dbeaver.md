# Como instalar o DBeaver (Interface simples para gerenciar bancos locais)

## **Links Úteis**

- Documentação oficial: https://dbeaver.io/docs/

<br>

## **Requisitos**

Antes de começar, verifique se você atende aos seguintes requisitos:

- **Sistema Operacional:** Windows 7 ou superior.
- **Espaço em Disco:** Aproximadamente 200 MB.

<br>

## **Passo a Passo**

### 1. Baixar e Instalar o DBeaver

1. Acesse o site oficial do DBeaver: https://dbeaver.io/.
2. Clique no botão **Download** no topo da página.
3. Escolha a versão **Community** (gratuita) e selecione a opção para **Windows (Installer)**.
4. Localize o arquivo baixado (ex.: `dbeaver-ce-x.x.x-installer.exe`) e execute-o.
5. Siga os passos do instalador clicando em **Next** e depois em **Install**.
6. Ao finalizar, clique em **Finish** para concluir a instalação.

<br>

### 2. Iniciar o DBeaver

1. Localize o ícone do DBeaver na área de trabalho ou no menu Iniciar.
2. Clique no ícone para abrir o aplicativo.
3. Pronto! Agora você está pronto para usar o DBeaver.

<br>
### 3. Configuração Inicial

### Adicionar uma Conexão ao PostgreSQL

1. Clique no ícone de **Nova Conexão** no canto superior esquerdo.
2. Na lista exibida, selecione **PostgreSQL** e clique em **Next**.
3. Preencha as informações da conexão:
    - **Host:** `localhost` (ou o IP do servidor PostgreSQL).
    - **Port:** `5432` (porta padrão do PostgreSQL).
    - **Database:** `postgres` (ou o nome do banco que deseja conectar).
    - **Username:** `postgres` (usuário padrão do PostgreSQL).
    - **Password:** `root` (ou a senha configurada durante a instalação do PostgreSQL).
4. Clique em **Test Connection**.

### Testar a Conexão (Exemplo)

1. Após clicar em **Test Connection**, o DBeaver tentará se conectar ao banco de dados.
2. **Resultado esperado:**
    - Se a conexão for bem-sucedida, você verá uma mensagem como:
        
        ```
        Connected successfully (ping: XX ms).
        ```
        
    - Isso confirma que o DBeaver está configurado corretamente para se conectar ao PostgreSQL.
3. Clique em **Finish** para salvar a conexão.

<br>

### 4. Conceder Permissões ao Usuário no PostgreSQL

Se você estiver utilizando o **n8n** ou outro aplicativo que precise de permissões adequadas no banco de dados PostgreSQL, siga os passos abaixo para conceder permissões ao usuário.

1. **Abrir o terminal do PostgreSQL:**
    
    No DBeaver, clique com o botão direito na conexão que você acabou de configurar e selecione **SQL Editor** para abrir uma nova aba de consulta SQL.
    
2. **Conceder permissões ao usuário `n8nuser`:**
    
    Execute o seguinte comando para garantir que o usuário tenha permissões para acessar e modificar o banco de dados:
    
    ```sql
    GRANT ALL PRIVILEGES ON SCHEMA public TO n8nuser;
    GRANT ALL PRIVILEGES ON DATABASE n8n TO n8nuser;
    ```
    
    Esses comandos garantem que o usuário `n8nuser` tenha permissão para trabalhar com o esquema `public` e a base de dados `n8n`.
    
3. **Verificar a execução:**
    
    Caso não apareça nenhuma mensagem de erro, as permissões foram concedidas corretamente. Caso contrário, revise os comandos e garanta que o usuário `n8nuser` esteja corretamente configurado no banco de dados.
    

### 5. Conectar-se ao Banco de Dados e Testar

Agora que você concedeu as permissões, volte à conexão no DBeaver e clique em **Test Connection** novamente para verificar se tudo está funcionando corretamente.