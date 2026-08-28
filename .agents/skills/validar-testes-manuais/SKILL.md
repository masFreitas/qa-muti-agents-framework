---
name: validar-testes-manuais
description: >-
  Executa a validação manual e exploratória na aplicação viva via Playwright CLI, realiza o isolamento de locators por contêiner, registra a memória técnica em .agents/context/{modulo}.md e atualiza o status na fila de tarefas em plano-teste/tarefas/{modulo}-tarefas.json. Use quando precisar: (1) Validar cenários de teste na aplicação viva em tempo real via Playwright CLI, (2) Inspecionar elementos DOM isolando o nó pai/contêiner de listas, tabelas e modais, (3) Homologar a disponibilidade visual e funcional de recursos, (4) Atualizar seletores na camada de memória viva (.agents/context/), (5) Transicionar status de tarefas para PRONTO_PARA_AUTOMATIZAR ou BLOQUEADO (em caso de bugs ou recursos ausentes na UI).
---

# Validar Testes Manuais e Coletar Locators

Esta skill estabelece o procedimento operacional para o **Agente Validador Manual e Exploratório** executar os testes previstos no plano de testes (`plano-teste/{modulo}-plan.md`) navegando na aplicação real através do **Playwright CLI**.

---

## 1. Fluxo de Trabalho (4 Passos)

### Passo 1: Leitura do Plano e Identificação de Pendências
1. Abra `plano-teste/tarefas/{modulo}-tarefas.json` e identifique os casos de teste com status `"PENDENTE"`.
2. Para cada CT pendente, consulte os passos numerados e o resultado esperado no arquivo `plano-teste/{modulo}-plan.md`.
3. Consulte as credenciais e rotas de acesso em `.agents/context/{modulo}.md` ou `.agents/context/system-overview.md`.

### Passo 2: Execução Exploratória e Inspeção DOM via Playwright CLI (🛑 HARD RULES)
1. Abra a aplicação viva utilizando `playwright-cli open <URL>` ou scripts de automação interativa.
2. Navegue pelas telas executando exatamente as ações descritas no passo a passo do CT (cliques, preenchimentos, navegações).
3. **Isolamento de Contêiner & Desambiguação de Seletores:**
   - Para botões de ação em listas, tabelas ou cards (ex: editar, excluir), extraia a árvore do **contêiner exato do item** (ex: `div.border.rounded-lg` correspondente àquela transação).
   - Nunca assuma que seletores genéricos globais na página pertencem ao item sob teste sem verificar a presença daquele botão dentro do contêiner do elemento.
   - Para modais (criação vs edição), inspecione os atributos reais (`id`, `name`, `placeholder`, `aria-label`) dentro do modal ativo (`div[role="dialog"]`).
4. **Verificação de Conclusão Assíncrona:**
   - Aguarde o término completo das ações e a confirmação visual ou no DOM antes de inferir o resultado do teste. Nunca faça afirmações baseadas em partes parciais do log.

### Passo 3: Homologação e Registro de Locators na Memória Viva
1. **Se o fluxo funcionar conforme o esperado e o recurso existir na UI:**
   - Registre os locators reais e métodos sugeridos na Seção 4 de `.agents/context/{modulo}.md`, indicando o contêiner e o modal de origem.
   - Marque a tarefa como `"PRONTO_PARA_AUTOMATIZAR"` no arquivo `plano-teste/tarefas/{modulo}-tarefas.json`.
2. **Se o recurso não existir no contêiner do item (ex: botão de edição ausente na linha do item), se houver bug ou erro:**
   - 🛑 **NÃO MASCARAR A FALHA OU SIMULAR BOTÕES AUSENTES.**
   - Registre a limitação ou defeito técnico com a justificativa exata em `"motivo_bloqueio"`.
   - Marque a tarefa como `"BLOQUEADO"` no arquivo `plano-teste/tarefas/{modulo}-tarefas.json`.

### Passo 4: Handoff para a Automação
Ao finalizar a validação manual do módulo:
- O arquivo `.agents/context/{modulo}.md` conterá todos os seletores testados e desacoplados por contêiner para o **Engenheiro de Automação**.
- O arquivo `plano-teste/tarefas/{modulo}-tarefas.json` estará atualizado com os status `"PRONTO_PARA_AUTOMATIZAR"` e `"BLOQUEADO"`.

---

## 2. Regras e Restrições Finais
* **PROIBIÇÃO DE CONCLUSÕES PREMATURAS:** Nunca declare um teste como validado com base em hipóteses ou execuções incompletas do Playwright CLI.
* **INSPEÇÃO DE CONTÊINER OBRIGATÓRIA:** Todo elemento de lista/tabela deve ser inspecionado dentro do seu nó pai correspondente.
