# Documentação Técnica: CI/CD - PHP Workflow

Este workflow realiza a integração e entrega contínua (CI/CD) para aplicações PHP, incluindo:

•	Criação de Feature Candidate

•	Execução de testes unitários

•	Análise de qualidade com SonarQube

•	Criação de Release Candidates

•	Deploy automatizado para ambientes de homologação e produção


# Gatilhos

Este workflow é chamado através de workflow_call, o que permite que outros workflows do GitHub o utilizem.

# Variáveis e Secrets Utilizados

**Secrets**

•	GITHUB_TOKEN: Token padrão do GitHub.

•	SONAR_TOKEN: Token para autenticação no SonarQube.

•	SLACK_WEBHOOK_URL: Webhook para notificações no Slack.

•	CICD_GITHUB_TOKEN: Token para workflows de CI/CD.

•	CICD_AWS_KEY: Chave de acesso AWS.

•	CICD_AWS_SECRET: Chave secreta AWS.

•	APP_CONTAINER_NAME: Nome do container onde os testes serão executados.

•	AWS_S3_BUCKET: Nome do bucket S3 para armazenar arquivos de configuração.

•	ENV_FILE_PATH: Caminho do arquivo de ambiente.

•	ENV_ENCRYPTION_PASSWORD: Senha para criptografia do .env.

•	CICD_SSH_KEY: Chave SSH para acesso aos servidores.

•	OVPN_CONFIG_FILE_DEVOPS: Configuração de VPN OpenVPN.

•	OVPN_CERT_PASS_DEVOPS: Senha de autenticação na VPN.

**Variáveis**

•	REPO_NAME: Nome do repositório.

•	CICD_AWS_REGION: Região da AWS.

•	ECR_REGISTRY: URL do repositório de imagens Docker.

•	IMAGE_NAME: Nome da imagem Docker.

•	DOCKERFILE_PATH: Caminho do Dockerfile.

•	BASE_IMAGE: Imagem base para o container.

•	DEPLOY_FILES: Lista de arquivos necessários para o deploy.

•	SERVER_LIST: Lista de servidores para deploy.

•	SECRET_MANAGER_KEY: Chave para acessar segredos no AWS Secret Manager.


# Fluxo Completo

1.	Criação de uma branch feature/* → Cria uma Feature Candidate automaticamente.

2.	Testes unitários e análise SonarQube são executados automaticamente para todas as branches, exceto feature/*.

3.	Feature Candidate (fc/*) é criada → Gera uma Release Candidate.

4.	Hotfix (fix/*) é criado → Gera uma Release Candidate de Hotfix.

5.	Deploy para Homologação (homol) acontece para:

•	Feature Candidates (fc/*).

•	Hotfixes (fix/*).

•	Release Candidates (rc/*).

6.	Deploy para Produção (prod) acontece após merge na main.

# Considerações

•	O fluxo suporta desenvolvimento contínuo com Feature Candidates e Hotfixes.

•	Aprovação manual pode ser adicionada antes do deploy em produção.

•	O SonarQube permite gate de qualidade para impedir merges em caso de falhas.

•	As credenciais AWS e GitHub são armazenadas como secrets para garantir segurança.