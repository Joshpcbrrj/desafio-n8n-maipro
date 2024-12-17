# Como instalar o PostegreSQL (Banco de dados local)

<img src="/docs/img/pgadmin.png" width="800" alt="Logo do N8N">

___

## **Pré-requisitos**

- Sistema operacional **Windows 11**.
- Acesso à internet.


### **Passos**

#### 1. **Baixar o Instalador do PostgreSQL**

1. Acesse a página oficial do PostgreSQL: https://www.postgresql.org/download/.
2. Clique em **Download** e selecione a versão adequada para **Windows**.
3. Faça o download do instalador correspondente ao seu sistema operacional.

<br>

#### 2. **Executar o Instalador**

1. Localize o arquivo `.exe` baixado e clique para executá-lo.
2. Siga as instruções do assistente de instalação. Para a maioria das opções, você pode manter as configurações padrão.
3. **Durante a instalação**, será solicitado que você:
    - **Escolha a senha para o usuário `postgres` (administrador do banco de dados)**:
        - Digite uma senha de sua escolha. Neste tutorial, usaremos **`root`** como exemplo.
        - **Atenção:** Guarde essa senha, pois será necessária para acessar o banco de dados posteriormente.
4. Conclua a instalação clicando em **Finish**.

<br>

#### 3. **Abrir o pgAdmin**

1. Após a instalação, abra o **pgAdmin** (gerenciador gráfico do PostgreSQL) através do menu Iniciar ou da área de trabalho.
2. O pgAdmin abrirá em seu navegador padrão. Na primeira execução, será solicitado que você insira a senha configurada para ele durante a instalação.

<br>

#### 4. **Testar a Conexão com o Banco de Dados**

1. Após fazer login no pgAdmin, adicione uma conexão com o banco de dados PostgreSQL:
    - Clique em **Add New Server** no painel à esquerda.
    - Preencha as informações necessárias:
        - **Name:** Nome de sua escolha (ex.: `PostgreSQL Local`).
        - **Host:** `localhost` ou o endereço IP do servidor.
        - **Port:** `5432` (porta padrão do PostgreSQL).
        - **Username:** `postgres` (usuário padrão do PostgreSQL).
        - **Password:** **`root`** (ou a senha definida durante a instalação).
2. Clique em **Save** para testar e salvar a conexão.
3. **Resultado esperado:**
    - Se a conexão for bem-sucedida, o servidor será listado no painel à esquerda e estará disponível para gerenciamento.

<br>

#### 5. **Liberar Acesso Remoto ao PostgreSQL**

### **Alterar as Configurações do PostgreSQL para Aceitar Conexões Remotas**

1. **Abrir o arquivo `postgresql.conf`:**
    - Navegue até o diretório onde o PostgreSQL foi instalado (geralmente em `C:\Program Files\PostgreSQL\17\data\`).
    - Encontre o arquivo chamado `postgresql.conf` e abra-o com um editor de texto (como o Notepad++).
2. **Habilitar a Escuta de Conexões Remotas:**
    - Procure pela linha `#listen_addresses = 'localhost'` no arquivo.
    - Descomente essa linha (removendo o `#`) e altere o valor para:
        
        ```
        listen_addresses = '*'
        
        ```
        
    - Isso permite que o PostgreSQL aceite conexões de qualquer IP.
3. **Salvar e Fechar o arquivo**.

<br>

##### **Alterar o Arquivo `pg_hba.conf` para Permitir Conexões Externas**

1. **Abrir o arquivo `pg_hba.conf`:**
    - No mesmo diretório onde você encontrou o `postgresql.conf`, localize o arquivo `pg_hba.conf` e abra-o com um editor de texto.
2. **Adicionar Permissão para Conexões Remotas:**
    - No final do arquivo, adicione a seguinte linha:
        
        ```
        host    all             all             0.0.0.0/0               md5
        ```
        
    - Isso permite que qualquer endereço IP se conecte ao banco de dados usando a autenticação `md5` (senha).
3. **Salvar e Fechar o arquivo**.

<br>

##### **Reiniciar o Serviço do PostgreSQL**

1. Abra o **Prompt de Comando** ou **PowerShell** como **Administrador**.
2. Execute o seguinte comando para reiniciar o serviço do PostgreSQL:
    
    ```powershell
    net stop postgresql-x64-17
    net start postgresql-x64-17
    ```
    
<br>

#### 6. **Verificar a Conexão Remota (usando o próprio PC)**

1. **Obter o Endereço IP do Computador:**
    - Abra o **Prompt de Comando** (cmd) e digite o seguinte comando para encontrar o seu endereço IP:
        
        ```powershell
        ipconfig
        ```
        
    - Procure pelo **endereço IPv4** da sua conexão de rede (normalmente, será algo como `192.168.x.x`).
2. **Testar a Conexão Usando o IP:**
    - Abra o **pgAdmin** ou qualquer cliente de banco de dados, e na hora de configurar a conexão, use o **endereço IP** obtido no passo anterior em vez de "localhost" ou "127.0.0.1".
    - Complete os campos de conexão como segue:
        - **Host:** O endereço IP que você obteve (exemplo: `192.168.x.x`).
        - **Port:** `5432` (porta padrão do PostgreSQL).
        - **Username:** `postgres` (usuário padrão do PostgreSQL).
        - **Password:** **`root`** (ou a senha que você definiu).
    - Clique em **Save** para tentar a conexão.
3. **Resultado Esperado:**
    - Se a configuração estiver correta e o PostgreSQL estiver aceitando conexões remotas, a conexão será bem-sucedida e você poderá acessar o banco de dados.
  

<br>

[Voltar para inicio](/)