---
name: validar-testes-manuais
description: >-
  Orienta o Agente Validador Manual e Exploratório a interagir fisicamente com a aplicação viva via Playwright CLI,
  homologar cenários de teste, extrair locators reais e registrar o mapeamento técnico em .agents/context/{modulo}.md
  e atualizar o status em plano-teste/tarefas/{modulo}-tarefas.json.
---

# Validar Testes Manuais e Coletar Locators

Esta skill estabelece o procedimento operacional para o **Agente Validador Manual e Exploratório** executar os testes previstos no plano de testes (`plano-teste/{modulo}-plan.md`) navegando na aplicação real através do **Playwright CLI**.

---

## 1. Fluxo de Trabalho (4 Passos)

### Passo 1: Leitura do Plano e Identificação de Pendências
1. Abra `plano-teste/tarefas/{modulo}-tarefas.json` e identifique os casos de teste com status `"PENDENTE"`.
2. Para cada CT pendente, consulte os passos numerados e o resultado esperado no arquivo `plano-teste/{modulo}-plan.md`.
3. Consulte as credenciais e rotas de acesso em `.agents/context/{modulo}.md` ou `.agents/context/system-overview.md`.

### Passo 2: Execução Exploratória e Inspeção DOM via Playwright CLI (🛑 HARD RULE)
1. Abra a aplicação viva utilizando `playwright-cli open <URL>` ou scripts de automação interativa.
2. Navegue pelas telas executando exatamente as ações descritas no passo a passo do CT (cliques, preenchimentos, navegações).
3. A cada interação com um novo componente (inputs, botões, modais, selects):
   - Capture a árvore de acessibilidade (`playwright-cli snapshot` ou avaliação de acessibilidade).
   - Extraia o **locator real e acessível** (`getByRole`, `getByLabel`, `getByPlaceholder`, `#id`).

### Passo 3: Homologação e Registro de Locators na Memória Viva
1. **Se o fluxo funcionar conforme o esperado:**
   - Registre os locators reais e métodos sugeridos na Seção 4 de `.agents/context/{modulo}.md`.
   - Marque a tarefa como `"PRONTO_PARA_AUTOMATIZAR"` no arquivo `plano-teste/tarefas/{modulo}-tarefas.json`.
2. **Se o fluxo apresentar bug, erro ou ausência de elemento na UI (ex: botão ausente):**
   - Registre a limitação ou defeito técnico com a justificativa em `"motivo_bloqueio"`.
   - Marque a tarefa como `"BLOQUEADO"` no arquivo `plano-teste/tarefas/{modulo}-tarefas.json`.

### Passo 4: Handoff para a Automação
Ao finalizar a validação manual do módulo:
- O arquivo `.agents/context/{modulo}.md` conterá todos os seletores testados e prontos para o **Engenheiro de Automação**.
- O arquivo `plano-teste/tarefas/{modulo}-tarefas.json` estará atualizado com os status `"PRONTO_PARA_AUTOMATIZAR"` e `"BLOQUEADO"`.
