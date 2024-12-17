# **Como configurar o N8N com Postgres local usando Docker**

<img src="/docs/img/n8ndocker.png" width="600" alt="Logo do N8N">

---

### Links úteis

- [N8N docs - installation docker](https://docs.n8n.io/hosting/installation/docker/)
- [Github netroy - Arquivo compose base](https://github.com/n8n-io/n8n-hosting/blob/main/docker-compose/withPostgres/docker-compose.yml)
- [Community N8Nio - How to Install n8n with Portainer & Docker CLI](https://community.n8n.io/t/how-to-install-n8n-with-portainer-docker-cli/40053)
- [Docker hub N8Nio (Workflow Automation Tool)](https://hub.docker.com/layers/n8nio/n8n/latest/images/sha256-ce3fc01486fa386da944337303108edd760dae617ce2fa9097f75126a8b8bcd1)
- [Youtube - Instalação via Docker - Curso Completo de N8N: Do Básico à Inteligência Artificial](https://www.youtube.com/watch?v=8hQ1u0TAyAc)
- [Youtube - N8N - O que é e Como Instalar Gratuitamente](https://www.youtube.com/watch?v=S8U3T9yf4Do)
- [Youtube - N8N - Como fazer a instalação e configuração EFICIENTE com PostgreSQL](https://www.youtube.com/watch?v=ext5zhtzc34)

<br>

## **Tutorial para Instalação do N8N com PostgreSQL no Docker**

### Passo 1: Criar o Arquivo `docker-compose.yml`

#### 1.1 - Navegue até o Diretório do Projeto

No **PowerShell**, acesse o diretório onde deseja configurar o projeto do N8N:

```powershell
cd C:\caminho\para\seu\projeto
```

#### 1.2 - Crie o Arquivo com o Comando `New-Item`

No terminal, use o comando **`New-Item`** para criar o arquivo `docker-compose.yml`:

```powershell
New-Item -ItemType File -Name "docker-compose.yml"
```

#### 1.3 - Edite o Arquivo

Abra o arquivo `docker-compose.yml` em um editor de texto (como o **VS Code**):

```powershell
code docker-compose.yml
```

Adicione o seguinte conteúdo ao arquivo:

```yaml
version: '3.8'

volumes:
  n8n_storage:

services:
  n8n:
    image: docker.n8n.io/n8nio/n8n
    restart: always
    environment:
      - DB_TYPE=postgresdb
      - DB_POSTGRESDB_HOST=host.docker.internal  # Usando host.docker.internal para acessar o PostgreSQL no Windows
      - DB_POSTGRESDB_PORT=5432
      - DB_POSTGRESDB_DATABASE=n8n
      - DB_POSTGRESDB_USER=n8nuser
      - DB_POSTGRESDB_PASSWORD=rootn8n
    ports:
      - 5678:5678
    volumes:
      - n8n_storage:/home/node/.n8n

```

<br>

### Passo 2: Liberar a Porta 5678 no Firewall do Windows

Para garantir que o N8N seja acessível externamente na porta **5678**, você precisa liberar essa porta no firewall do Windows. Siga os passos abaixo:

1. **Abrir o Painel de Controle do Windows:**
    - Clique com o botão direito do mouse no menu Iniciar e selecione **Painel de Controle**.
2. **Acessar o Firewall do Windows:**
    - No painel de controle, clique em **Sistema e Segurança** e, em seguida, em **Firewall do Windows Defender**.
3. **Criar uma Nova Regra de Entrada para a Porta 5678:**
    - Clique em **Configurações Avançadas** no painel à esquerda.
    - Na janela do **Firewall do Windows com Segurança Avançada**, clique em **Regras de Entrada** no menu à esquerda.
    - Clique em **Nova Regra** no painel à direita.
    - Selecione **Porta** e clique em **Avançar**.
    - Escolha **TCP** e insira **5678** no campo para especificar o número da porta.
    - Selecione **Permitir a conexão** e clique em **Avançar**.
    - Selecione as opções **Domínio**, **Privado** e **Público**, dependendo de sua necessidade, e clique em **Avançar**.
    - Dê um nome para a regra (por exemplo, "Docker Porta 5678") e clique em **Concluir**.

Agora a porta **5678** está liberada no firewall e pode ser acessada pela aplicação N8N.

<br>

### Passo 3: Subir o Container Docker

#### 3.1 - Certifique-se de Estar no Diretório do Projeto

No **PowerShell**, navegue até o diretório do projeto onde o arquivo `docker-compose.yml` foi criado:

```powershell
cd C:\caminho\para\seu\projeto
```

#### 3.2 - Inicie o Container

Inicie o container do Docker com o comando:

```powershell
docker-compose up -d
```

- O Docker irá:
    - Fazer o download das imagens do N8N e do PostgreSQL, caso ainda não estejam no sistema.
    - Criar e executar os containers com base no arquivo `docker-compose.yml`.

#### 3.3 - Verifique se o Container Está Ativo/ Comandos para gerenciar o container

Execute o seguinte comando para listar os containers ativos:

```powershell
# Verifica quais containers estão ativos
docker ps

# Para parar o container do serviço 'n8n'
docker stop n8n #(n8n é o nome dele no arquivo compose) 

# Para iniciar o container novamente
docker start n8n

# Para remover o container (certifique-se de que ele está parado antes)
docker rm n8n 

# Para recriar e iniciar os containers conforme o docker-compose.yml
docker-compose up -d
```

Certifique-se de que o container **n8n** e o **postgres** estejam listados como ativos.

<br>

### Passo 4: Acessar o N8N

#### 4.1 - Abra o Navegador

No navegador, acesse o endereço:

```
http://localhost:5678
```

- Você será redirecionado para a interface do N8N, onde poderá criar e gerenciar workflows.

<br>

### Passo 5: Testar a Conexão com o Banco de Dados PostgreSQL

1. Na interface do N8N, crie um workflow simples que interaja com o banco de dados PostgreSQL.
2. Configure a conexão utilizando as credenciais fornecidas:
    - **Usuário:** `n8nuser`
    - **Senha:** `rootn8n`
3. Teste a inserção e recuperação de dados no banco por meio do N8N para garantir que a conexão esteja funcionando corretamente.

<br>

[Voltar ao inicio](/)