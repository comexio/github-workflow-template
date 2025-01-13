# GitHub Workflow - Repositório `XXXXXX`

Com a centralização dos workflows, a Logcomex consolidou seus processos em um único repositório de templates específicos. Isso garante consistência, padronização e maior controle nos fluxos de desenvolvimento.

### Workflows Detalhados

1. **`create-fc` (Feature Candidate)**  
   - **Descrição**: Contém todas as instruções necessárias para a criação de uma nova branch de *feature candidate* com integração ao Jira.  
   - **Detalhes Técnicos**:
     - Respeita o padrão de nomenclatura definido pelos projetos.
     - Verifica a existência da branch antes de criar.
     - Inclui tags de pré-release no fluxo, associadas às novas branches criadas.
     - Atualiza automaticamente o status no Jira.

2. **`create-rc` (Release Candidate)** e **`create-hotfix`**  
   - **Descrição**: Criam branches para release candidates e hotfixes, utilizando versionamento semântico.  
   - **Fluxo Técnico**:
     - Busca a última tag publicada no GitHub Releases.
     - Instala o plugin de versionamento semântico no *runner* do GitHub Actions.
     - Realiza o bump da versão:
       - `minor` para *feature candidates*.
       - `patch` para *hotfixes*.
     - Valida a existência da branch:
       - Se a branch já existe, apenas cria um Pull Request (PR) com as alterações atuais.
       - Caso contrário, cria uma nova branch e realiza o upload para o repositório.
     - Finaliza o fluxo adicionando uma tag de pré-release (`prerelease`) para sinalizar o estado da branch.

3. **`unit-test`**  
   - **Descrição**: Executa os testes unitários definidos no repositório.  
   - **Detalhes Técnicos**:
     - Configura o ambiente de testes (dependências e configurações).
     - Utiliza ferramentas como PHPUnit ou outras especificadas no projeto.
     - Gera relatórios de cobertura e logs detalhados de execução.

4. **`sonar-scan`**  
   - **Descrição**: Realiza a análise estática de código com o SonarQube e publica os artefatos gerados no SonarCloud.  
   - **Detalhes Técnicos**:
     - Inclui autenticação com tokens do SonarCloud.
     - Configura parâmetros personalizados para o scanner (ex: qualidade do código e métricas específicas).
     - Exibe os resultados diretamente na interface do SonarCloud.

5. **`feature-deploy`**  
   - **Descrição**: Automatiza o deploy de branches *feature candidate* em ambientes de desenvolvimento.  
   - **Fluxo Técnico**:
     - Configura o ambiente de destino.
     - Realiza o deploy utilizando ferramentas como Docker, Kubernetes ou outros pipelines.

6. **`hotfix-deploy`**  
   - **Descrição**: Automatiza o deploy de branches *hotfix* em ambientes de homologação e produção.  
   - **Detalhes**:
     - Valida dependências antes de iniciar o deploy.
     - Inclui verificações pós-deploy para assegurar a integridade do ambiente.

7. **`rc-deploy`**  
   - **Descrição**: Fluxo de versionamento semântico com validação para os ambientes `dev` e `homol`.  
   - **Fluxo Técnico**:
     - Realiza o bump de versão com base no estado atual do repositório.
     - Valida a compatibilidade do código antes de publicar o release.

8. **`prod-deploy`**  
   - **Descrição**: Finaliza o fluxo de versionamento semântico e publica em produção.  
   - **Detalhes**:
     - Inclui validações adicionais específicas para o ambiente de produção.
     - Atualiza automaticamente a documentação com a nova versão publicada.

---

## Fluxo de Versionamento Semântico (Release | Pré-Release)

- **Pré-Release**:  
  Tags que identificam mudanças preliminares nas branches (ex.: `minor` ou `patch`).  
  Utilizado para validações intermediárias antes da finalização do release.

- **Release**:  
  Tags definitivas para versões estáveis, sinalizando que a branch passou com sucesso por todos os fluxos da pipeline.

---

## Etapas do Workflow (Resumo)

1. **Criação da branch `FC` (Feature Candidate)**  
   - Realizada automaticamente após a abertura de uma tarefa no Jira.  

2. **Interação com `FC`**  
   - Gera automaticamente a branch `RC` (Release Candidate).  

3. **Interação entre `FC` e `RC`**  
   - Criação de um Pull Request (PR) da branch `FC` para `RC`.  

4. **Interação entre `RC` e `MAIN`**  
   - Criação de um PR da branch `RC` para `MAIN`.  

---

## Fluxo Gitflow

- **Branches principais**:
  - `MAIN`: Contém o código estável em produção.
  - `DEVELOP`: Código integrado em desenvolvimento.
  
- **Branches de suporte**:
  - `FEATURE`: Desenvolvimento de novas funcionalidades.
  - `RELEASE`: Prepara o código para lançamento.
  - `HOTFIX`: Correções emergenciais diretamente em produção.

- **Padrões de nomenclatura**:
  - `feature/<nome-da-funcionalidade>`
  - `release/<versão>`
  - `hotfix/<correção-específica>`