# Modelos de Markdown para Exercícios
Uma coleção de modelos para uso em exercícios.

## Coleções de Modelos

- `/readme/*` - modelos destinados a atualizar o arquivo readme raiz de um exercício.
- `/step-feedback/*` - modelos para compartilhar o progresso das etapas, avaliação de aprovação/falha e parabenizações. Normalmente usados em comentários de issues.

## Variáveis de Modelo

Vários modelos contêm templating de variáveis estilo [Nunjucks](https://mozilla.github.io/nunjucks/). Eles são destinados para uso com as GitHub Actions [skills/action-text-variables](https://github.com/skills/action-text-variables) ou [GrantBirki/comment](https://github.com/GrantBirki/comment), ambas suportam templating completo de Nunjucks.



### Exemplo

#### hello.md

```markdown
Olá {{ login }}, fico feliz em conhecer você!
```

#### entrada json

```json
{
  "login": "${{ github.actor }}"
}
```

#### entrada yaml
```yaml
login: "${{ github.actor }}"
```

> [!TIP]
> Veja a [documentação de templating Nunjucks](https://mozilla.github.io/nunjucks/templating.html) para todas as capacidades como iteração, condicionais e muito mais.
