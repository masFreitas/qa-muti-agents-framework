---
name: analisar-falhas-playwright
description: >-
  Guia e protocolo de diagnóstico e correção (self-healing) para o agente Revisor e Correção (revisor-correcao).
  Fornece uma árvore de decisão sistemática para analisar falhas em testes Playwright, identificar a causa raiz
  (seletores alterados, asserções desatualizadas, problemas de sincronismo, mudanças funcionais ou bugs do sistema),
  inspecionar a aplicação viva via playwright-cli, editar Page Objects e Specs, e sincronizar o estado da memória e tarefas.
---

# Analisar Falhas em Testes Playwright (Self-Healing Protocol)

## Visão Geral
Esta skill orienta o processo de diagnóstico, correção e manutenção de testes automatizados Playwright que pararam de funcionar ou que falharam em execuções de regressão/CI. Ela não faz parte do fluxo sequencial principal (Requisitos -> Testes -> Automação), sendo acionada **sob demanda** quando um teste que já existe falha.

---

# Gatilhos de Acionamento
Esta skill é ativada quando o usuário solicita correções pontuais de testes falhos ou fornece logs de execução.
Exemplos de gatilhos:
- *"Corrija o teste de login que está falhando"*
- *"O teste CT02 de transações parou de funcionar na suíte de regressão"*
- Envio de trechos de log de erro, stacktrace ou relatórios de execução (`playwright-report`, status report da automação).

---

# Fluxo de Diagnóstico e Correção (5 Passos)

## Passo 1: Absorção dos Logs e Contexto de Falha
1. Analise o log de erro fornecido pelo usuário ou extraído da execução.
2. Identifique:
   - **Módulo e arquivo de spec afetado:** ex: `tests/transacoes.spec.ts`
   - **Caso de teste (CT) que falhou:** ex: `CT02 - Realizar transferência`
   - **Mensagem de erro exata e linha da falha:** ex: `Timeout 5000ms exceeded waiting for locator('button#btn-submit')`
   - **Causa preliminar apontada pelo runner:** locator timeout, assertion error, navigation timeout, etc.
3. Consulte a memória técnica do módulo em `.agents/context/{modulo}.md` para entender as rotas de acesso, credenciais e Page Objects catalogados.

## Passo 2: Inspeção Obrigatória da Aplicação Viva via Playwright CLI (🛑 HARD RULE)
**NUNCA TENTE ADIVINHAR O SELETOR CORRETO APENAS LENDO O CÓDIGO DA ESPECIFICAÇÃO!**
1. 🛑 **OBRIGATÓRIO:** Utilize a skill `playwright-cli` para navegar até a tela onde a falha ocorreu no ambiente real/homologação.
2. Capture o snapshot completo do DOM e da árvore de acessibilidade da página (`playwright-cli snapshot`).
3. Extraia o locator verdadeiro e atualizado do componente diretamente da árvore de acessibilidade (`getByRole`, `getByLabel`, `getByPlaceholder`, `locator('#id-real')`).

## Passo 3: Aplicação da Árvore de Decisão de Causa Raiz

| Categoria | Sintoma no Log | Causa Provável | Ação de Correção |
| :--- | :--- | :--- | :--- |
| **A. Seletor Alterado** | `Timeout exceeded waiting for locator(...)` ou `element not found` | A interface mudou IDs, classes ou estrutura HTML | 1. Usar `playwright-cli` para obter o novo locator acessível.<br>2. Atualizar o locator no Page Object (`pages/{modulo}.page.ts`).<br>3. Não alterar o teste se o fluxo de negócio continuar o mesmo. |
| **B. Sincronismo / Timeout** | `Element is not visible`, `element is detached`, `intercepted click` | Elemento demorou para carregar ou transição assíncrona não aguardada | 1. Remover `waitForTimeout()` estáticos.<br>2. Substituir por web-first assertions reativas (`await expect(locator).toBeVisible()`).<br>3. Aguardar o estado final do elemento antes da ação. |
| **C. Mudança de Regra / Fluxo** | Assertion error: `Expected 'Sucesso' received 'Confirmado'` ou novos passos requeridos | A regra de negócio ou texto/layout mudou intencionalmente | 1. Validar a nova regra em `.agents/context/{modulo}.md`.<br>2. Atualizar métodos do Page Object e asserções na spec (`tests/{modulo}.spec.ts`).<br>3. Documentar a atualização no contexto técnico. |
| **D. Bug Real da Aplicação** | Erro 500 na API, botão desabilitado indevidamente, funcionalidade indisponível | Falha ou regresso no software sob teste | 1. **Não forçar a passagem do teste** corrigindo asserção para aceitar erro.<br>2. Marcar o teste temporariamente com `test.fixme()`.<br>3. Adicionar comentário detalhado antes do bloco do teste.<br>4. Atualizar o status do CT para `"BLOQUEADO"` em `plano-teste/tarefas/{modulo}-tarefas.json`. |

## Passo 4: Re-execução e Validação Local
1. Execute o teste modificado usando a CLI do Playwright (`npx playwright test tests/{modulo}.spec.ts`).
2. Confirme se o teste executa de forma determinística e passa limpo (verde).
3. Se houver falhas secundárias, repita o processo de diagnóstico um erro por vez.

## Passo 5: Atualização da Memória e Status das Tarefas
Após validar a correção do teste:
1. **Memória Contextual:** Se novos locators ou métodos foram adicionados ao Page Object, invoque a skill `gerenciar-contexto-negocio` (modo `ATUALIZACAO`) para atualizar a Seção 4 do arquivo `.agents/context/{modulo}.md`.
2. **Fila de Tarefas JSON:** Invoque a skill `gerenciar-tarefas-teste` para atualizar `plano-teste/tarefas/{modulo}-tarefas.json`:
   - Se o teste foi corrigido com sucesso: atualizar o status para `"AUTOMATIZADO"`, renovando a data de atualização.
   - Se o teste foi marcado como `test.fixme()` devido a bug real: atualizar o status para `"BLOQUEADO"`, preenchendo a justificativa detalhada em `"motivo_bloqueio"`.

---

# Regras e Proibições Estritas (Hard Rules)
- 🛑 **PROIBIÇÃO ABSOLUTA DE DADOS/LOCATORS ADIVINHADOS:** Nunca edite um seletor quebrado sem antes inspecionar a interface viva via `playwright-cli`.
- **MANTENHA TESTES DETERMINÍSTICOS:** Proibido utilizar `page.waitForTimeout()` ou sleeps arbitrários para contornar problemas de sincronismo.
- **NAO MASCARAR BUGS:** É proibido alterar asserções esperadas para fazer o teste passar quando houver um bug real no sistema sob teste.
- **PRESERVAÇÃO DO PADRÃO POM:** Toda alteração de locators DEVE ser feita na classe de Page Object em `pages/`, mantendo os arquivos em `tests/` focados na sequência de passos e asserções.
