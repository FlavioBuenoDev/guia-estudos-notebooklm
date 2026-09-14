# guia-estudos-notebooklm
Esse será um repositório para o treinamento de IA - DIO

# 🎯 Objetivos:
  -  Comporte-se como um desenvolvedor de software e ajude a compreender a arquitetura de containers do n8n, dominar o uso de webhooks e criar um guia prático de referência rápida para implantação em servidores Debian e Ubuntu

# 📚 Curadoria de Fontes
  - https://www.youtube.com/watch?v=wMachQ0Ph-k
  - https://www.youtube.com/watch?v=bz5e6u3OfpU
  - https://aws.amazon.com/pt/free/containers/?trk=ae9f5ec7-1e7a-4148-b014-2392144368e1&sc_channel=ps&ef_id=CjwKCAjwtp7VBhBjEiwAJfpV-6OcUQ7mDJINzJw3FCD0BQvPpsDQ4YqF7Of--COMwWnoMqeYLsHiehoC8tgQAvD_BwE:G:s&gads_camp=23533256329&gads_ag=197930742883&gads_ad=816175851817&gads_kw=aplicativos%20em%20cont%C3%AAineres&gads_matchtype=p&gads_network=g&gads_device=c&gads_geo=9196892&gad_campaignid=23533256329&gbraid=0AAAAADjHtp_zWd3CaBi5--IX6oK7p5i23&gclid=CjwKCAjwtp7VBhBjEiwAJfpV-6OcUQ7mDJINzJw3FCD0BQvPpsDQ4YqF7Of--COMwWnoMqeYLsHiehoC8tgQAvD_BwE
  - https://www.youtube.com/watch?v=bre2yEUFBnc
  - https://www.youtube.com/watch?v=DoDuN-BARwA

# 🛠️ Engenharia de Prompts e "Cicatrizes"
 ## Prompt: Explique como configurar a virtualização na BIOS para usar Docker
 ## Resposta: Para utilizar o Docker Desktop sem interrupções no seu computador, é necessário garantir que a **virtualização de hardware esteja ativada na BIOS**. 

O passo a passo para realizar essa configuração envolve os seguintes tópicos:

1. **Acessar a BIOS**: Reinicie o computador e acesse a BIOS do seu dispositivo durante a inicialização.
2. **Navegar até as Configurações Avançadas**: Procure pela seção de configurações do sistema ou do processador, cujos nomes variam de acordo com a fabricante do seu dispositivo (geralmente em menus como **Avançado**, **Configuração da CPU**, **Configuração do Processador** ou **Segurança**).
3. **Habilitar o Recurso de Virtualização**:
   - **Processadores Intel**: Localize a opção referente à **virtualização** presente nos menus avançados, de configuração da CPU/processador, segurança ou sistema, e altere o status para ativado.
   - **Processadores AMD**: Navegue até o menu de **configurações avançadas** da CPU para localizar e ativar a opção de virtualização.
4. **Salvar e Iniciar o Computador**: Salve a alteração feita na BIOS e continue com o carregamento normal do sistema.

Com a virtualização habilitada na BIOS, o Docker poderá ser aberto e executado normalmente no sistema em segundo plano. Em ambientes Windows, também é importante certificar-se de habilitar o recurso **WSL 2 (Windows Subsystem for Linux)** ou o **Hyper-V** para garantir o correto funcionamento dos contêineres.

### Prompt: O que é o Docker Compose?
### Resposta: O **Docker Compose** é uma ferramenta do ecossistema Docker utilizada para **definir, orquestrar e gerenciar aplicações multi-container** a partir de um único arquivo de configuração, geralmente chamado `docker-compose.yml`

Em vez de executar comandos longos e manuais no terminal (`docker run`) para cada container individualmente, o Docker Compose permite descrever todos os serviços, redes e volumes em formato YAML.

### Principais recursos e vantagens:

* Orquestração de múltiplos serviços**: Permite rodar e conectar vários containers na mesma execução — por exemplo, a ferramenta n8n operando em conjunto com um banco de dados como PostgreSQL ou Redis.
* Gerenciamento simplificado**: É possível iniciar todo o ambiente de uma só vez em segundo plano (`docker-compose up -d`) ou pará-lo completamente (`docker-compose down`).
* Organização de configurações**: Centraliza em um só arquivo a definição de portas mapeadas, variáveis de ambiente e **volumes** para a persistência de dados.
* Portabilidade e padronização**: Garante que o ambiente de execução permaneça idêntico em qualquer máquina, facilitando a migração do ambiente de desenvolvimento para produção

### Prompt:  Quais diferenças na instalação do docker no ubuntu ou no debian ?
### Resposta: Nos materiais do seu caderno, a instalação do Docker no Linux é exemplificada utilizando o **Ubuntu**, sendo realizada pelo terminal através do gerenciador de pacotes `apt` com os comandos `sudo apt update` e `sudo apt install docker.io`[1].

No entanto, **os fontes não detalham as diferenças específicas de instalação entre o Ubuntu e o Debian**. Os materiais cobrem a instalação no Linux de forma geral utilizando o Ubuntu como referência, mas não abordam comparativos de repositórios ou particularidades entre essas duas distribuições.

### Prompt: Me mostre um docker-compose.yml para o n8n
### Resposta: Aqui está um modelo padrão de arquivo **docker-compose.yml** para rodar o n8n de forma prática com suporte a persistência de dados[1]:

```
version: '3.8'

services:
  n8n:
    image: n8nio/n8n
    container_name: n8n
    restart: unless-stopped
    ports:
      - "5678:5678"
    environment:
      - GENERIC_TIMEZONE=America/Sao_Paulo
      - TZ=America/Sao_Paulo
      - N8N_ENFORCE_SETTINGS_FILE_PERMISSIONS=true
      - N8N_RUNNERS_ENABLED=true
    volumes:
      - n8n_data:/home/node/.n8n

volumes:
  n8n_data:

```

### Explicação dos principais campos:

* **image: n8nio/n8n**: Define a imagem oficial do n8n que o Docker baixa do Docker Hub[3][4].
* **ports: - "5678:5678"**: Mapeia a porta 5678 do container para a porta 5678 da sua máquina local, permitindo o acesso via navegador[3][4].
* **volumes: - n8n\_data:/home/node/.n8n**: Garante a persistência de workflows, credenciais e configurações no diretório do container[3].
* **environment**: Define variáveis de ambiente, como o fuso horário (`America/Sao_Paulo`) e configurações de execução[2][4].
* **restart: unless-stopped**: Faz com que o container seja reiniciado automaticamente se falhar ou se o Docker reiniciar[2][3].

---

### Como iniciar o serviço:

1. Salve o código acima em um arquivo chamado **docker-compose.yml** na pasta do seu projeto[1].
2. Pelo terminal ou PowerShell, navegue até essa pasta e execute em segundo plano[7][8]:

```
docker-compose up -d

```

1. Abra o seu navegador e acesse o endereço **http://localhost:5678**[1].

---




