---
name: gerenciar-tarefas-teste
description: >-
  Gerencia a leitura, consulta de pendências e atualização de status da fila de automação em plano-teste/tarefas/{modulo}-tarefas.json.
  Use quando o Engenheiro de Automação precisar: (1) Identificar casos de teste pendentes de automação para um módulo,
  (2) Atualizar o status de um CT para AUTOMATIZADO com o caminho do spec gerado, (3) Registrar o status BLOQUEADO em caso de falhas ou bugs impeditivos.
---

# Gerenciar Tarefas de Teste

Esta skill define o protocolo para consultar e atualizar a fila de tarefas de automação de testes armazenada em `plano-teste/tarefas/{modulo}-tarefas.json`.

## Quando Usar

* Ao iniciar a automação de um módulo para identificar quais Casos de Teste (CT) possuem status `"PENDENTE"`.
* Após codificar e verificar um caso de teste automatizado para transitar o status para `"AUTOMATIZADO"`.
* Ao encontrar uma impossibilidade técnica ou bug que impeça a automação de um CT para marcar o status como `"BLOQUEADO"`.

## Insumos e Estrutura dos Arquivos

1. **Arquivo de Tarefas JSON:** `plano-teste/tarefas/{modulo}-tarefas.json`
2. **Plano de Teste Associado:** `plano-teste/{modulo}-plan.md` (contém a descrição detalhada dos passos numerados de cada CT)

---

## Protocolo de Execução

### Step 1: Leitura e Seleção de Pendências (Modo CONSULTA)

1. Localize o arquivo `plano-teste/tarefas/{modulo}-tarefas.json`.
2. Filtre a lista `casos_teste` onde `status == "PENDENTE"`.
3. Para cada CT pendente:
   - Extraia o `id` (ex: `CT01`), o `titulo` e os `criterios_aceite_cobertos`.
   - Consulte o arquivo `plano-teste/{modulo}-plan.md` no bloco correspondente ao `id` para obter a sequência exata de passos numerados e o resultado esperado.

---

### Step 2: Atualização de Tarefa Automatizada (Modo SUCESSO)

Após criar ou atualizar o teste correspondente na suíte de testes do módulo (ex: `tests/{modulo}.spec.ts`) e validar que a execução foi bem-sucedida:

1. Atualize o item em `casos_teste`:
   - `"status"`: `"AUTOMATIZADO"`
   - `"arquivo_spec"`: `"tests/{modulo}.spec.ts"` (caminho relativo do arquivo de spec)
   - `"motivo_bloqueio"`: `null`
   - `"data_atualizacao"`: Data atual no formato `"AAAA-MM-DD"`
2. Recalcule o objeto `resumo_status`:
   - `pendentes`: Contagem atualizada de CTs com status `"PENDENTE"`
   - `automatizados`: Contagem atualizada de CTs com status `"AUTOMATIZADO"`
   - `bloqueados`: Contagem atualizada de CTs com status `"BLOQUEADO"`
3. Salve as alterações em `plano-teste/tarefas/{modulo}-tarefas.json`.

---

### Step 3: Atualização de Tarefa Bloqueada (Modo BLOQUEIO)

Se o caso de teste não puder ser automatizado por conta de um erro/bug evidente na aplicação ou seletores inacessíveis:

1. Atualize o item em `casos_teste`:
   - `"status"`: `"BLOQUEADO"`
   - `"arquivo_spec"`: `null` (ou o arquivo parcial se houver)
   - `"motivo_bloqueio"`: Descrição clara e concisa da causa do bloqueio (ex: `"Bug na aplicação: botão Salvar desabilitado permanentemente"` ou `"Seletor inacessível no shadow DOM"`).
   - `"data_atualizacao"`: Data atual no formato `"AAAA-MM-DD"`
2. Recalcule o objeto `resumo_status` (`pendentes`, `automatizados`, `bloqueados`).
3. Salve as alterações em `plano-teste/tarefas/{modulo}-tarefas.json`.

---

## Exemplo de Transição de Estado

### Antes (Pendentes)
```json
{
  "modulo": "transacoes",
  "plano_teste_ref": "plano-teste/transacoes-plan.md",
  "requisitos_ref": "docs/requisitos-refinados/transacoes.md",
  "data_criacao": "2026-08-27",
  "total_casos_teste": 2,
  "resumo_status": {
    "pendentes": 2,
    "automatizados": 0,
    "bloqueados": 0
  },
  "casos_teste": [
    {
      "id": "CT01",
      "titulo": "Realizar transferência com saldo suficiente",
      "criterios_aceite_cobertos": ["CA01"],
      "status": "PENDENTE",
      "arquivo_spec": null,
      "motivo_bloqueio": null,
      "data_atualizacao": "2026-08-27"
    },
    {
      "id": "CT02",
      "titulo": "Bloquear transferência sem saldo",
      "criterios_aceite_cobertos": ["CA02"],
      "status": "PENDENTE",
      "arquivo_spec": null,
      "motivo_bloqueio": null,
      "data_atualizacao": "2026-08-27"
    }
  ]
}
```

### Depois (Concluído)
```json
{
  "modulo": "transacoes",
  "plano_teste_ref": "plano-teste/transacoes-plan.md",
  "requisitos_ref": "docs/requisitos-refinados/transacoes.md",
  "data_criacao": "2026-08-27",
  "total_casos_teste": 2,
  "resumo_status": {
    "pendentes": 0,
    "automatizados": 2,
    "bloqueados": 0
  },
  "casos_teste": [
    {
      "id": "CT01",
      "titulo": "Realizar transferência com saldo suficiente",
      "criterios_aceite_cobertos": ["CA01"],
      "status": "AUTOMATIZADO",
      "arquivo_spec": "tests/transacoes.spec.ts",
      "motivo_bloqueio": null,
      "data_atualizacao": "2026-08-27"
    },
    {
      "id": "CT02",
      "titulo": "Bloquear transferência sem saldo",
      "criterios_aceite_cobertos": ["CA02"],
      "status": "AUTOMATIZADO",
      "arquivo_spec": "tests/transacoes.spec.ts",
      "motivo_bloqueio": null,
      "data_atualizacao": "2026-08-27"
    }
  ]
}
```

## Regras e Restrições
* **Sincronia com o Plano:** O `id` do teste (ex: `CT01`) deve corresponder exatamente ao identificador no plano de teste markdown.
* **Integridade do Resumo:** O objeto `resumo_status` deve ser mantido rigorosamente atualizado com a soma matemática das contagens de cada status.
* **Caminhos Relativos:** `arquivo_spec` deve armazenar o caminho relativo a partir da raiz do repositório (ex: `tests/login.spec.ts`).
