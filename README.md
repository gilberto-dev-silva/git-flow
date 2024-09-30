# Fluxo de Trabalho do Gitflow

## O que é Gitflow?
Gitflow é um modelo de ramificação do Git que utiliza ramificações de recursos e múltiplas ramificações principais. Popularizado por Vincent Driessen, é mais complexo que o desenvolvimento baseado em tronco, envolvendo várias ramificações de longa duração.

## Como funciona?
- **Ramificações Principais**: Utiliza `main` para o histórico oficial de lançamentos e `develop` como ramificação de integração.
- **Ramificações de Recursos**: Cada recurso tem sua própria ramificação a partir de `develop` e é mesclado de volta ao concluir.

## Branchs develop and main
1. **Ao usar a biblioteca de extensão git-flow**: a execução em um repositório existente criará a ramificação: `develop`.

	```bash
	git flow init develop
2. Logo em seguida execute

```bash
$ git flow init

Initialized empty Git repository in ~/project/.git/
No branches exist yet. Base branches must be created now.
Branch name for production releases: [main]
Branch name for "next release" development: [develop]

How to name your supporting branch prefixes?
Feature branches? [feature/]
Release branches? [release/]
Hotfix branches? [hotfix/]
Support branches? [support/]
Version tag prefix? []

$ git branch
* develop
  main
```
## Feature branches
1. **Criação do Repositório**: Cada novo recurso deve ser desenvolvido em sua própria branch, enviada para o repositório central para colaboração e backup.
2. **Estrutura de Branches**: As branches de recursos (feature branches) são criadas a partir da branch `develop`, não da `main`.
3. **Mesclagem**: Quando um recurso está concluído, ele é mesclado de volta na branch `develop`. As branches de recursos não devem interagir diretamente com a `main`.
4. **Fluxo de Trabalho**: O uso de feature branches com a branch `develop` caracteriza o Feature Branch Workflow, que é parte do fluxo de trabalho do Gitflow.

### Observação
Feature branches são geralmente criadas a partir da versão mais recente da branch `develop`.

### Criando um Branch de Recurso

#### Sem as Extensões git-flow:
```bash
git checkout develop
git checkout -b feature_branch
```	
#### Com a Extensões git-flow:
```bash
git flow feature start feature_branch
```
### Finalizando um Branch de Recurso

> Quando você terminar o trabalho de desenvolvimento do recurso, o próximo passo > é mesclá-lo `feature_branch` em `develop`.

#### Sem as Extensões git-flow:
```bash
git checkout develop
git merge feature_branch
```

#### Usando as extensões git-flow:
```bash
git flow feature finish feature_branch
```

## Gerenciando Release branches
> Quando a branch `develop` acumula recursos suficientes para um lançamento ou uma  data de lançamento se aproxima, você bifurca um `release branch` de `develop`.
> Este processo marca o início do próximo ciclo de lançamento, e a partir desse
> ponto, nenhum novo recurso pode ser adicionado — apenas correções de bugs e
> tarefas relacionadas ao lançamento.

### Processo de Lançamento:
1. **Mesclagem**: O `release branch` é mesclado na branch `main` e marcado com um número de versão.
2. **Mesclagem de Volta**: Deve ser mesclado novamente em `develop`, que pode ter avançado durante o processo de lançamento.

### Vantagens dos Release Branches
- Permitem que uma equipe trabalhe na polimento do lançamento atual, enquanto o  utra continua a desenvolver novos recursos.
- Criam fases bem definidas de desenvolvimento, facilitando a comunicação sobre o que está sendo preparado (por exemplo, "Estamos nos preparando para a versão 4.0").

#### Sem as Extensões git-flow:
```bash
git checkout develop
git checkout -b release/0.1.0
```
#### Usando as extensões git-flow:
```bash
git flow release start 0.1.0
```

### Finalizando um Release Branch
Quando o lançamento estiver pronto, ele será mesclado em `main` e `develop`, e o `release branch` será excluído. É crucial mesclar novamente em `develop`, pois atualizações críticas podem ter sido adicionadas ao `release branch`, e essas mudanças precisam estar disponíveis para novos recursos. Se sua organização prioriza a revisão de código, este é um bom momento para uma solicitação de pull.

### Métodos para Finalizar um Release Branch

#### Sem as Extensões git-flow:
```bash
git checkout main
git merge release/0.1.0
```
#### Usando as extensões git-flow:
```bash
git flow release finish '0.1.0'
```

## Gerenciando Hotfix branches
> As ramificações de manutenção, ou hotfix, são usadas para corrigir rapidamente lançamentos em produção. Elas são semelhantes às ramificações de lançamento e de recursos, mas são baseadas na branch `main` em vez de `develop`. Este é o único tipo de branch que deve ser criado diretamente a partir de `main`.

#### Processo de Hotfix
1. **Correção**: Assim que a correção estiver completa, ela deve ser mesclada em `main` e `develop` (ou na branch de lançamento atual).
2. **Tagging**: A branch `main` deve ser marcada com um número de versão atualizado.

> Ter uma linha de desenvolvimento dedicada para correções de bugs permite que sua equipe aborde problemas sem interromper o fluxo de trabalho ou esperar pelo próximo ciclo de lançamento. As ramificações de manutenção podem ser vistas como ramificações de lançamento ad hoc que trabalham diretamente com `main`.

#### Criação de Hotfix Branches
Um hotfix branch pode ser criado utilizando os seguintes métodos:

#### Sem as Extensões git-flow:
```bash
git checkout main
git checkout -b hotfix_branch
```
#### Usando as extensões git-flow:
```bash
git flow hotfix start hotfix_branch
```
### Resumo
> Aqui discutimos o Gitflow Workflow, que é um dos muitos estilos de fluxos de trabalho do Git que você e sua equipe podem utilizar.

#### Lições Importantes sobre o Gitflow:
- O fluxo de trabalho é ótimo para um desenvolvimento de software baseado em lançamentos.
- O Gitflow oferece um canal dedicado para hotfixes em produção.

#### Fluxo Geral do Gitflow:
1. Um branch `develop` é criado a partir de `main`.
2. Um branch `release` é criado a partir de `develop`.
3. Feature branches são criadas a partir de `develop`.
4. Quando uma feature é concluída, ela é mesclada ao branch `develop`.
5. Quando o branch `release` é finalizado, ele é mesclado em `main` e `develop`.
6. Se um problema em `main` for detectado, um branch `hotfix` será criado a partir de `main`.
7. Uma vez que o hotfix é concluído, ele é mesclado em ambos `main` e `develop`.