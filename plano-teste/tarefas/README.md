# Pasta de Tarefas e Status de Automação (`plano-teste/tarefas/`)

Esta pasta contém os arquivos de controle em formato JSON com o status individual de cada Caso de Teste (CT) a ser automatizado pelo **Engenheiro de Automação** (`03-automador-playwright`).

## Nomenclatura dos Arquivos
- `plano-teste/tarefas/{modulo}-tarefas.json`

## Status Possíveis dos Casos de Teste
- `PENDENTE`: Caso de teste criado no plano de testes, aguardando validação manual e extração de locators.
- `PRONTO_PARA_AUTOMATIZAR`: Caso de teste homologado manualmente pelo Validador Manual, com locators reais coletados em `.agents/context/{modulo}.md` e pronto para codificação.
- `AUTOMATIZADO`: Código de teste Playwright gerado em `tests/` e verificado com sucesso pelo Engenheiro de Automação.
- `BLOQUEADO`: Automação/Validação impedida devido a bug na aplicação, limitação de UI ou dependência não atendida.

## Estrutura Padrão do JSON

```json
{
  "modulo": "nome-do-modulo",
  "plano_teste_ref": "plano-teste/nome-do-modulo-plan.md",
  "requisitos_ref": "docs/requisitos-refinados/nome-do-modulo.md",
  "data_criacao": "YYYY-MM-DD",
  "total_casos_teste": 2,
  "resumo_status": {
    "pendentes": 2,
    "automatizados": 0,
    "bloqueados": 0
  },
  "casos_teste": [
    {
      "id": "CT01",
      "titulo": "Título do Caso de Teste",
      "criterios_aceite_cobertos": ["CA01"],
      "status": "PENDENTE",
      "arquivo_spec": null,
      "motivo_bloqueio": null,
      "data_atualizacao": "YYYY-MM-DD"
    }
  ]
}
```
