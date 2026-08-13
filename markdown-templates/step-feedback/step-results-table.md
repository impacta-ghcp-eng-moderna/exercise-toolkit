{%- set all_passed = (results_table | selectattr("passed") | length) == (results_table | length) %}

{%- if all_passed %}

## Etapa {{ step_number }} - Aprovado ✅

{%- else %}

## Etapa {{ step_number }} - Falha ❌

Algumas verificações falharam. Por favor, revise os resultados abaixo e tente novamente.

Hora de encontrar o erro! 🤔
{%- endif %}

| Status | Descrição |
| ------ | ----------- |

{%- for row in results_table %}
| {% if row.passed -%}✅ - Aprovado{%- else -%}❌ - Falhou{%- endif %} | {{ row.description }} |
{%- endfor %}

{%- if tips and tips.length %}

### Dicas

{%- for tip in tips %}

- {{ tip }}
  {%- endfor %}

{%- endif %}
