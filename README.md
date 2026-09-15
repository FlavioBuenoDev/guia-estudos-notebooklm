# Guia de Estudos: Treinamento de IA - DIO

Repositório dedicado ao projeto prático de curadoria de conhecimento, engenharia de prompts e aprendizagem ativa utilizando o **NotebookLM**.

## 🎯 Objetivos
Comporte-se como um desenvolvedor de software e ajude a compreender a arquitetura de containers do n8n, dominar o uso de webhooks e criar um guia prático de referência rápida para implantação em servidores Debian e Ubuntu.

## 📚 Curadoria de Fontes
As fontes utilizadas para alimentar o caderno de estudos no NotebookLM foram:

  - https://www.youtube.com/watch?v=wMachQ0Ph-k
  - https://www.youtube.com/watch?v=bz5e6u3OfpU
  - https://aws.amazon.com/pt/free/containers/?trk=ae9f5ec7-1e7a-4148-b014-2392144368e1&sc_channel=ps&ef_id=CjwKCAjwtp7VBhBjEiwAJfpV-6OcUQ7mDJINzJw3FCD0BQvPpsDQ4YqF7Of--COMwWnoMqeYLsHiehoC8tgQAvD_BwE:G:s&gads_camp=23533256329&gads_ag=197930742883&gads_ad=816175851817&gads_kw=aplicativos%20em%20cont%C3%AAineres&gads_matchtype=p&gads_network=g&gads_device=c&gads_geo=9196892&gad_campaignid=23533256329&gbraid=0AAAAADjHtp_zWd3CaBi5--IX6oK7p5i23&gclid=CjwKCAjwtp7VBhBjEiwAJfpV-6OcUQ7mDJINzJw3FCD0BQvPpsDQ4YqF7Of--COMwWnoMqeYLsHiehoC8tgQAvD_BwE
  - https://www.youtube.com/watch?v=bre2yEUFBnc
  - https://www.youtube.com/watch?v=DoDuN-BARwA

## 🛠️ Engenharia de Prompts e "Cicatrizes" (Troubleshooting)
### Prompt 1: Configuração de Virtualização
  - Prompt: Explique como configurar a virtualização na BIOS para usar Docker.
  - Resposta: Para utilizar o Docker Desktop sem interrupções no seu computador, é necessário garantir que a virtualização de hardware esteja ativada na BIOS. O passo a passo envolve:

1. Acessar a BIOS: Reinicie o computador e acesse a BIOS durante a inicialização.
2. Navegar até as Configurações Avançadas: Procure pela seção de configurações do sistema ou do processador (geralmente em menus como Avançado, Configuração da CPU ou Segurança).
3. Habilitar o Recurso:
   - Processadores Intel: Localize a opção de virtualização (Intel VT-x) e ative-a.
   - Processadores AMD: Navegue até as configurações avançadas da CPU e ative o AMD-V.
4. Salvar e Reiniciar: Salve as alterações e reinicie o sistema.

- Nota/Cicatriz: Em ambientes Windows, certifique-se também de habilitar o WSL 2 (Windows Subsystem for Linux) para o correto funcionamento dos containers.

### Prompt 2: O que é o Docker Compose?
 - Prompt: O que é o Docker Compose?
 - Resposta: O Docker Compose é uma ferramenta do ecossistema Docker utilizada para definir, orquestrar e gerenciar aplicações multi-container a partir de um único arquivo de configuração YAML (docker-compose.yml).

- Vantagens principais:

  - Orquestração de múltiplos serviços: Roda e conecta vários containers simultaneamente (ex: n8n junto com PostgreSQL).
  - Gerenciamento simplificado: Inicia todo o ambiente com um único comando (docker-compose up -d) e encerra com (docker-compose down).
  - Padronização: Centraliza portas, volumes e variáveis de ambiente em um só lugar.

### Prompt 3: Instalação no Ubuntu vs. Debian
  - Prompt: Quais diferenças na instalação do Docker no Ubuntu ou no Debian?
  - Resposta: Nos materiais analisados, a instalação do Docker no Linux é exemplificada utilizando o Ubuntu, sendo realizada via gerenciador de pacotes apt com os comandos tradicionais de repositório. Contudo, as fontes de estudo não aprofundam as pequenas variações de chaves GPG ou repositórios específicos entre o Debian e o Ubuntu, mantendo o processo base muito similar.

### Prompt 4: Arquivo Docker Compose para o n8n
  - Prompt: Me mostre um docker-compose.yml para o n8n.
  - Resposta: Abaixo está o modelo padrão utilizado para rodar o n8n com persistência de dados:

```
YAML
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

- Como iniciar:
  1. Salve o código acima em um arquivo chamado docker-compose.yml.
  2. No terminal, na mesma pasta do arquivo, execute: docker-compose up -d
  3. Acesse no navegador: http://localhost:5678

### 📖 Miniguia de Estudo (Entrega Final)
1. Resumos Estruturados
  - Arquitetura de Instalação via Docker Compose:
    - Imagem oficial: n8nio/n8n (versão slim ou padrão).
    - Persistência: O diretório interno /home/node/.n8n armazena credenciais, chaves de criptografia e histórico de execuções, devendo ser mapeado para um volume no host.
    - Portas: A porta padrão 5678 expõe o serviço, idealmente protegida posteriormente por um proxy reverso (Nginx/Traefik) com SSL/TLS.

  - Gerenciamento de Webhooks e Gatilhos:
    - Webhooks funcionam como pontos de extremidade HTTP que escutam eventos externos em tempo real.
    - No n8n, os nós de gatilho processam payloads JSON recebidos de sistemas externos de forma assíncrona.

2. Glossário de Termos Técnicos
  - Container: Unidade padrão de software que empacota o código e suas dependências para execução isolada e confiável.
  - Volume (Docker): Mecanismo utilizado para persistir dados gerados por containers de forma independente do ciclo de vida deles.
  - Webhook: Forma leve e baseada em eventos para uma aplicação notificar outra em tempo real.
  - Variáveis de Ambiente: Parâmetros de execução dinâmicos usados para injetar configurações sensíveis (como credenciais e chaves) sem alterar o código-fonte.

### 📋 Banco de Prompts Reutilizáveis
  - Para auditoria de segurança:
    "Com base nas diretrizes de segurança das fontes, analise os riscos potenciais de expor a porta 5678 do n8n diretamente na internet sem um proxy reverso com autenticação."
  - Para otimização de fluxos:
    "Liste as melhores práticas de tratamento de erros (Error Handling) descritas na documentação para fluxos de longa duração no n8n."
  - Para suporte a troubleshooting:
    "Quais são os passos recomendados para debugar falhas de conexão entre um container n8n e um banco de dados PostgreSQL externo na mesma rede Docker?"
