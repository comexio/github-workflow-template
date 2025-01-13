## Github Workflow - Repositorio XXXXXX
## Template - XXXXXX - Link XXXXXX

## Github Workflow
Com a centralização dos workflows agora a ATARB2B possue somente 1 repositório de templates para Py2 e Py3.

- Workflow ```create-fc``` contém todas as instrucoes para criacao de uma nova branch feature candidate, ainda com integracao do jira e respeitando o padrao de nomenclaturas dos projetos (pbo|maa|asd|gdp|asdp|ae|at).

- Workflow ```create-rc``` e ```create-hotfix``` contém todas as instrucoes para execucao da criacao da release candidate, busca no github releases a ultima tag, instala no agent o plugin de versionamento semantico, realiza o bump de minor(fc) ou patch(hotfix), verifica se a branch já existe caso já exista apenas criará uma pull request contendo as alteracoes atuais e caso nao exista irá criar uma nova branch e realizara o upload para o github. a ultima etapa desse workflow será inserir a tag de prerelease para as novas branch criadas.

- Workflow ```unit-test``` contém todas as instrucoes para execucao dos testes unitários existente do repositório.

- Workflow ```sonar-scan``` contém todas as instrucoes para execucao do scanner do sonarqube e publish dos artefatos gerados para o sonar cloud.

- Workflow ```feature-deploy``` contém todas as instrucoes para execucao do deploy da feature candidate.

- Workflow ```hotfix-deploy``` contém todas as instrucoes para execucao do deploy da hotfix branch.

- Workflow ```rc-deploy``` contém todas as instrucoes para execucao do fluxo de versionamento semantico (explicado no passo a passo abaixo) e realizado validacao para ambiente de destino (dev) com a instrucao de **--no-promote**  no ambiente.

- Workflow ```prod-deploy``` contém todas as instrucoes para execucao do fluxo de versionamento semantico (explicado no passo a passo abaixo) e realizado validacao para ambiente de destino (prod) com a instrucao de **--promote** no ambiente.

## Fluxo de Versionamento semantico ( Release | Pre-Release )

- PreRelease = tag que sinaliza o fluxo de versionamento semantico, onde de acordo com a origem e realizado o bump de minor ou patch
- Release = tag que sinaliza branchs que executaram todos os fluxos da pipeline e foram finalizadas com sucesso

## Etapas do Workflow

- abertura de branch **FC** (feature candidate ) é realizada automaticamente após fluxo do jira;
- após interacao com **FC**, é aberto um branch **RC** (release candidate) automaticamente;
- após interacao com **FC**, é aberto um pull request da **FC** para **RC** automaticamente;
- após interacao com **RC**, é aberto um pull request da **RC** para **MAIN** automaticamente;