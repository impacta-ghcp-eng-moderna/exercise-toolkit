# Exercises Toolkit :hammer_and_wrench:

> 📝 **Nota**: Este é um fork personalizado do repositório original [impacta-ghcp-eng-moderna/exercise-toolkit](https://github.com/impacta-ghcp-eng-moderna/exercise-toolkit), adaptado e traduzido para as necessidades da Impacta.

- [Exercises Toolkit :hammer\_and\_wrench:](#exercises-toolkit-hammer_and_wrench)
  - [Propósito](#propósito)
    - [Conteúdo](#conteúdo)
  - [Exemplos](#exemplos)
    - [⚙️ Fluxos de Trabalho Reutilizáveis](#️-fluxos-de-trabalho-reutilizáveis)
      - [Iniciando um exercício](#iniciando-um-exercício)
      - [Encontrando um exercício](#encontrando-um-exercício)
    - [📋 Modelos Markdown](#-modelos-markdown)
      - [Usando com GrantBirki/comment para comentários em issues](#usando-com-grantbirkicomment-para-comentários-em-issues)
      - [Usando com action-text-variables para atualizações de arquivo](#usando-com-action-text-variables-para-atualizações-de-arquivo)
  - [Recursos Notáveis](#recursos-notáveis)

## Propósito

Este repositório funciona como um kit de ferramentas abrangente para criar e gerenciar exercícios. Fornece uma coleção de ferramentas, modelos e utilitários projetados para otimizar o processo de desenvolvimento de conteúdo educacional.

### Conteúdo

- **[.github/workflows](/.github/workflows)**: Fluxos de trabalho do GitHub Actions para automatizar partes comuns dos Exercícios Skills
- **[markdown-templates](/markdown-templates)**: Modelos Markdown prontos para uso para criar documentação de exercícios consistente, instruções e arquivos README
- **[actions](/actions)**: Ações compostas simples para ajudar na construção de exercícios

## Exemplos

### ⚙️ Fluxos de Trabalho Reutilizáveis

Para uma lista completa de fluxos de trabalho reutilizáveis, acesse o diretório **[.github/workflows](/.github/workflows)**.

#### Iniciando um exercício

```yaml
jobs:
  start_exercise:
    name: Iniciar Exercício
    uses: impacta-ghcp-eng-moderna/exercise-toolkit/.github/workflows/start-exercise.yml@<git-tag>
    with:
      exercise-title: "Introdução ao GitHub Copilot"
      intro-message: "Vamos começar com o GitHub Copilot :robot: ! Aprenderemos ..."
```

#### Encontrando um exercício

```yaml
jobs:
  find_exercise:
    name: Encontrar Issue do Exercício
    uses: impacta-ghcp-eng-moderna/exercise-toolkit/.github/workflows/find-exercise-issue.yml@<git-tag>
```

### 📋 Modelos Markdown

Para uma lista completa de modelos markdown, acesse o diretório **[markdown-templates](/markdown-templates)**.

```yaml
steps:
  - name: Obter modelos markdown
    uses: actions/checkout@v6
    with:
      repository: impacta-ghcp-eng-moderna/exercise-toolkit
      path: exercise-toolkit
      ref: <git-tag>

  - name: Usar o modelo
    run: |
      cat exercise-toolkit/markdown-templates/step-feedback/checking-work.md
```

#### Usando com GrantBirki/comment para comentários em issues

Os modelos geralmente são usados com [GrantBirki/comment](https://github.com/GrantBirki/comment) para criar comentários dinâmicos em issues ou pull requests:

```yaml
steps:
  - name: Obter modelos markdown
    uses: actions/checkout@v6
    with:
      repository: impacta-ghcp-eng-moderna/exercise-toolkit
      path: exercise-toolkit
      ref: <git-tag>

  - name: Criar comentário - etapa concluída
    uses: GrantBirki/comment@v2.1.1
    with:
      file: exercise-toolkit/markdown-templates/step-feedback/step-finished-prepare-next-step.md
      issue-number: ${{ env.ISSUE_NUMBER }}
      repository: ${{ env.ISSUE_REPOSITORY }}
      vars: |
        next_step_number: 2
```

#### Usando com action-text-variables para atualizações de arquivo

Os modelos Markdown também podem ser usados com [skills/action-text-variables](https://github.com/skills/action-text-variables) para gerar conteúdo dinâmico para qualquer finalidade, por exemplo, atualizar um arquivo.

```yaml
steps:
  - name: Obter modelos markdown
    uses: actions/checkout@v6
    with:
      repository: impacta-ghcp-eng-moderna/exercise-toolkit
      path: exercise-toolkit
      ref: <git-tag>

  - name: Construir README a partir do modelo
    id: build-readme
    uses: skills/action-text-variables@v4
    with:
      template-file: exercise-toolkit/markdown-templates/readme/exercise-started.md
      template-vars: |
        title: ${{ inputs.exercise-title }}
        login: ${{ github.actor }}
        issue_url: ${{ needs.create_exercise.outputs.issue-url }}

  - name: Atualizar arquivo README
    run: echo "$README_CONTENT" > README.md
    env:
      README_CONTENT: ${{ steps.build-readme.outputs.updated-text }}
```

## Recursos Notáveis

Essas GitHub Actions são particularmente úteis ao criar exercícios:

- **[skills/action-text-variables](https://github.com/skills/action-text-variables)**: Substitua variáveis em arquivos de modelo por conteúdo dinâmico
- **[skills/action-keyphrase-checker](https://github.com/skills/action-keyphrase-checker)**: Verifique se frases-chave específicas existem em arquivos ou conteúdo
- **[GrantBirki/comment](https://github.com/GrantBirki/comment)**: Crie comentários em issues ou pull requests do GitHub com suporte para templating Nunjucks
