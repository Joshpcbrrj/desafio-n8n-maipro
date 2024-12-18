# Como instalar o Docker desktop (Aplicações em containers)

<img src="/docs/img/docker.png" width="800" alt="Logo do N8N">

---

## **Links Úteis**

- **Documentação Oficial:** Para mais informações, consulte a documentação oficial: https://docs.docker.com/.

### **Requisitos**

- Windows 10 (64 bits) ou superior.
- Processador de 64 bits com suporte à virtualização (ativada na BIOS).
- Pelo menos 4 GB de RAM.
- Conta no Docker Hub (pode ser criada gratuitamente em: https://hub.docker.com/).

<br>

### Passo a passo da instalação:

#### Passo 1 - Baixar o Docker Desktop

1. Acesse o site oficial do Docker Desktop: https://www.docker.com/products/docker-desktop.
2. Clique em **Download for Windows** para baixar o instalador.
3. Aguarde o download do arquivo (ex.: `Docker Desktop Installer.exe`).

<br>

#### Passo 2 - Instalar o Docker Desktop

1. Localize o arquivo baixado e clique duas vezes para iniciá-lo.
2. No assistente de instalação:
    - Certifique-se de marcar a opção para instalar o **WSL 2** (Windows Subsystem for Linux), caso solicitado.
    - Clique em **OK** ou **Next** para continuar.
3. Aguarde a conclusão da instalação.
4. Clique em **Close** para finalizar.

<br>

#### Passo 3 - Configurar o Docker Desktop

1. **Iniciar o Docker Desktop:**
    - Após a instalação, localize o ícone do Docker Desktop no menu Iniciar ou na área de trabalho e abra o aplicativo.
2. **Fazer Login no Docker Hub:**
    - Insira suas credenciais do Docker Hub (e-mail e senha).
3. **Configurar o WSL 2:**
    - Caso solicitado, habilite o **WSL2** como backend do Docker.
    - Reinicie o computador, se necessário.

<br>

#### Passo 4 - Testar a Instalação

1. Abra o **Prompt de Comando**, **PowerShell**, ou o terminal de sua preferência.
2. Verifique a versão instalada do Docker com o comando:
    
    ```powershell
    docker --version
    ```
    
    - O comando deve retornar a versão do Docker instalada.
3. Execute o comando abaixo para testar o funcionamento:
    
    ```powershell
    docker run hello-world
    ```
    
    - Esse comando baixará uma imagem de teste e executará um container.
    - Se o Docker estiver configurado corretamente, você verá uma mensagem confirmando que o teste foi bem-sucedido.

<br>

[Voltar ao inicio](/)