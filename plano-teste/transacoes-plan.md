# Plano de Testes: Módulo Transações (Receitas e Despesas)

## 1. Informações Gerais & Insumos
* **Módulo:** `transacoes` (Gestão de Receitas e Despesas - Fluxo de Caixa)
* **Documento de Requisitos:** [`docs/requisitos-refinados/transacoes.md`](file:///C:/Users/MateusArthurdaSilvad/Documents/qa-muti-agents-framework/docs/requisitos-refinados/transacoes.md)
* **Contexto de Negócio:** [`.agents/context/transacoes.md`](file:///C:/Users/MateusArthurdaSilvad/Documents/qa-muti-agents-framework/.agents/context/transacoes.md)
* **Visão Geral do Sistema:** [`.agents/context/system-overview.md`](file:///C:/Users/MateusArthurdaSilvad/Documents/qa-muti-agents-framework/.agents/context/system-overview.md)
* **Data de Criação:** 2026-08-27
* **Autor:** Analista de Testes (QA Squad)

## 2. Pré-condições & Ambiente
* **URL / Ponto de Entrada:** `https://love-money-simple.lovable.app/`
* **Estado do Sistema:** Usuário cadastrado e autenticado com sessão ativa (`mateusasfreitas@gmail.com` / `Pic@12345`).
* **Navegadores Homologados:** Chromium, Firefox, WebKit (via Playwright CLI / Automação).

## 3. Massa de Dados Recomendada
* **Dados Válidos:**
  * Usuário Autenticado: `mateusasfreitas@gmail.com` / `Pic@12345`
  * Transação Entrada: Descrição: `Salário Mensal`, Valor: `5000.00`, Data: `2026-08-27`, Meio: `Pix`, Categoria: `Renda`
  * Transação Saída Comum: Descrição: `Conta de Luz`, Valor: `150.00`, Data: `2026-08-27`, Meio: `Boleto`, Categoria: `Moradia`
  * Transação Saída Cartão: Descrição: `Supermercado`, Valor: `350.00`, Data: `2026-08-27`, Meio: `Cartão de Crédito`, Categoria: `Alimentação`
* **Dados Inválidos / Exceção:**
  * Valor Zerado: `0.00`
  * Valor Negativo: `-150.00`
  * Campos vazios: Descrição `""`, Valor `""`, Data `""`

---

## 4. Casos de Teste Detalhados

### CT01: Cadastro de Transação de Entrada (Income) e Atualização de Saldo
* **Objetivo:** Validar que o cadastro de uma receita do tipo Entrada soma o valor ao saldo consolidado no dashboard.
* **Pré-condições:** Usuário autenticado na tela do Dashboard. Anotar o saldo inicial `S0`.
* **Massa de Dados:** Tipo: `Entrada (income)`, Descrição: `Freelance Projeto A`, Valor: `1200.00`, Data: `2026-08-27`, Meio: `Pix`, Categoria: `Serviços`.
* **Passos de Execução:**
  1. Acessar o formulário/modal de inclusão de nova transação.
  2. Selecionar o tipo de transação `Entrada` (ou `Receita`).
  3. Preencher o campo **Descrição** com `Freelance Projeto A`.
  4. Preencher o campo **Valor** com `1200.00`.
  5. Selecionar a **Data** `2026-08-27`.
  6. Selecionar o **Meio de Pagamento** `Pix`.
  7. Selecionar a **Categoria** `Serviços`.
  8. Clicar no botão **Salvar** / **Cadastrar**.
* **Resultado Esperado:** O registro é exibido na lista de transações com o rótulo/cor de Entrada, o modal fecha e o saldo consolidado no dashboard é atualizado para `S0 + 1200.00`.

---

### CT02: Cadastro de Transação de Saída (Expense) com Meio de Pagamento Comum
* **Objetivo:** Validar que o cadastro de uma despesa do tipo Saída subtrai o valor do saldo consolidado no dashboard.
* **Pré-condições:** Usuário autenticado na tela do Dashboard. Anotar o saldo inicial `S0`.
* **Massa de Dados:** Tipo: `Saída (expense)`, Descrição: `Aluguel Residencial`, Valor: `1500.00`, Data: `2026-08-27`, Meio: `Transferência / Pix`, Categoria: `Moradia`.
* **Passos de Execução:**
  1. Abrir o formulário de inclusão de nova transação.
  2. Selecionar o tipo de transação `Saída` (ou `Despesa`).
  3. Preencher a **Descrição** com `Aluguel Residencial`.
  4. Preencher o **Valor** com `1500.00`.
  5. Selecionar a **Data** `2026-08-27`.
  6. Selecionar o **Meio de Pagamento** `Transferência`.
  7. Selecionar a **Categoria** `Moradia`.
  8. Clicar no botão **Salvar**.
* **Resultado Esperado:** A transação é salva com sucesso, exibida na lista e o saldo consolidado no dashboard é atualizado para `S0 - 1500.00`.

---

### CT03: Cadastro de Despesa com Cartão de Crédito e Vinculação Automática de Fatura
* **Objetivo:** Validar que uma despesa com Meio de Pagamento "Cartão de Crédito" é vinculada automaticamente à fatura do cartão referente ao mês da transação.
* **Pré-condições:** Usuário autenticado. Anotar o valor atual da fatura do mês `F0`.
* **Massa de Dados:** Tipo: `Saída`, Descrição: `Compra de Eletrônicos`, Valor: `800.00`, Data: `2026-08-27`, Meio: `Cartão de Crédito`, Categoria: `Tecnologia`.
* **Passos de Execução:**
  1. Abrir o formulário de cadastrar transação.
  2. Selecionar o tipo `Saída`.
  3. Preencher **Descrição** `Compra de Eletrônicos` e **Valor** `800.00`.
  4. Informar a **Data** `2026-08-27`.
  5. Selecionar o **Meio de Pagamento** `Cartão de Crédito`.
  6. Clicar em **Salvar**.
  7. Navegar até a seção de Cartões de Crédito / Faturas.
* **Resultado Esperado:** A despesa é salva e vinculada automaticamente à fatura do mês correspondente (Agosto/2026), e o valor consolidado da fatura é atualizado para `F0 + 800.00`.

---

### CT04: Edição de Transação Existente e Recálculo de Saldo
* **Objetivo:** Validar que ao editar os campos de uma transação salva (ex: alterar valor), as mudanças são persistidas e o saldo recarregado/recalculado.
* **Pré-condições:** Transação cadastrada (ex: Entrada de `500.00`). Anotar o saldo consolidado atual `S0`.
* **Massa de Dados:** Novo Valor: `750.00`, Nova Descrição: `Reajuste Freelance`.
* **Passos de Execução:**
  1. Localizar a transação na lista de registros.
  2. Clicar na ação **Editar** da transação.
  3. Alterar a **Descrição** para `Reajuste Freelance`.
  4. Alterar o **Valor** de `500.00` para `750.00`.
  5. Clicar no botão **Salvar Alterações**.
* **Resultado Esperado:** O registro na listagem passa a exibir a nova descrição `Reajuste Freelance` e valor `750.00`, e o saldo consolidado no dashboard é ajustado em `+250.00` (diferença entre o novo e o antigo valor).

---

### CT05: Exclusão de Transação e Estorno de Valor no Saldo
* **Objetivo:** Validar que ao excluir uma transação, o registro é removido das listagens e seu valor é estornado dos cálculos de saldo e fatura.
* **Pré-condições:** Transação de Saída de `300.00` existente na lista. Anotar o saldo consolidado atual `S0`.
* **Massa de Dados:** N/A (Ação na transação de `300.00`).
* **Passos de Execução:**
  1. Localizar a transação de `300.00` na listagem de transações.
  2. Clicar na ação **Excluir** / **Remover**.
  3. Confirmar a exclusão no modal de confirmação (caso seja exibido).
* **Resultado Esperado:** A transação é removida da listagem visual e o saldo consolidado do dashboard é estornado/recalculado para `S0 + 300.00`.

---

### CT06: Cadastro de Transação sem Preencher Categoria (Campo Opcional)
* **Objetivo:** Validar que o campo Categoria é opcional e permite salvar a transação com sucesso sem selecionar uma categoria.
* **Pré-condições:** Formulário de inclusão aberto.
* **Massa de Dados:** Tipo: `Entrada`, Descrição: `Venda de Item Usado`, Valor: `100.00`, Data: `2026-08-27`, Meio: `Pix`, Categoria: `[Em Branco]`.
* **Passos de Execução:**
  1. Preencher **Descrição** `Venda de Item Usado`.
  2. Preencher **Valor** `100.00`.
  3. Preencher **Data** `2026-08-27`.
  4. Selecionar **Meio de Pagamento** `Pix`.
  5. Deixar o campo **Categoria** em branco / não selecionado.
  6. Clicar em **Salvar**.
* **Resultado Esperado:** A transação é salva com sucesso, sem erros de validação, exibindo Categoria como `Sem Categoria` ou `N/A`.

---

### CT07: Tentativa de Cadastro com Valor Zero ou Negativo
* **Objetivo:** Validar o bloqueio e exibição de mensagem de erro ao tentar salvar uma transação com valor igual a zero ou menor que zero.
* **Pré-condições:** Formulário de transação aberto.
* **Massa de Dados:** Valor 1: `0.00`, Valor 2: `-50.00`.
* **Passos de Execução:**
  1. Preencher **Descrição** `Teste Valor Inválido`.
  2. Preencher **Valor** com `0.00`.
  3. Preencher **Data** `2026-08-27` e **Meio de Pagamento** `Pix`.
  4. Clicar no botão **Salvar**.
  5. Observar o comportamento do sistema.
  6. Alterar o **Valor** para `-50.00` e tentar salvar novamente.
* **Resultado Esperado:** O sistema impede o salvamento em ambas as tentativas e exibe a mensagem de erro de validação (ex: *"O valor deve ser maior que zero"*).

---

### CT08: Tentativa de Submissão com Campos Obrigatórios em Branco
* **Objetivo:** Validar que o sistema não permite cadastrar transações com Descrição, Valor, Data ou Meio de Pagamento ausentes.
* **Pré-condições:** Formulário de inclusão aberto com todos os campos limpos.
* **Massa de Dados:** Campos em branco.
* **Passos de Execução:**
  1. Deixar todos os campos do formulário em branco.
  2. Clicar no botão **Salvar**.
* **Resultado Esperado:** O formulário não é enviado, o cadastro é bloqueado e os campos obrigatórios em branco são destacados com alerta/mensagem de validação.

---

### CT09: Cadastro de Transações com Datas Passadas e Futuras
* **Objetivo:** Validar que o sistema aceita registros com datas retroativas e datas futuras sem restrições.
* **Pré-condições:** Formulário de inclusão aberto.
* **Massa de Dados:** Data Passada: `2025-01-15`, Data Futura: `2027-12-31`.
* **Passos de Execução:**
  1. Cadastrar uma transação de Entrada com a Data `2025-01-15`.
  2. Verificar o resultado do salvamento.
  3. Cadastrar uma transação de Saída com a Data `2027-12-31`.
  4. Verificar o resultado do salvamento.
* **Resultado Esperado:** Ambas as transações são gravadas com sucesso e alocadas corretamente nas listagens/períodos respectivos.

---

### CT10: Alteração do Meio de Pagamento na Edição (Cartão de Crédito <-> Outros Meios)
* **Objetivo:** Validar que ao editar uma despesa mudando o meio de pagamento de Cartão de Crédito para Pix, a despesa é desvinculada da fatura do cartão e o saldo consolidado ajustado.
* **Pré-condições:** Despesa de `200.00` cadastrada no Cartão de Crédito (vinculada à fatura).
* **Massa de Dados:** Alteração do Meio de Pagamento de `Cartão de Crédito` para `Pix`.
* **Passos de Execução:**
  1. Abrir a edição da despesa de `200.00`.
  2. Alterar o **Meio de Pagamento** de `Cartão de Crédito` para `Pix`.
  3. Clicar em **Salvar Alterações**.
  4. Verificar a fatura do cartão e o saldo consolidado do dashboard.
* **Resultado Esperado:** O valor de `200.00` é removido/estornado da fatura do cartão e passa a afetar diretamente o saldo consolidado do dashboard via Pix.

---

## 5. Matriz de Rastreabilidade de Requisitos

Esta matriz garante que 100% dos Critérios de Aceite (CA) especificados pelo **Analista de Requisitos** possuem cobertura completa por Casos de Teste.

| ID do Critério de Aceite (CA) | Descrição Resumida do Critério | ID do Caso de Teste (CT) | Status da Cobertura |
| :--- | :--- | :--- | :--- |
| **CA01** | Permite cadastro de Entrada e soma ao saldo consolidado | CT01 | Coberto |
| **CA02** | Permite cadastro de Saída e subtrai do saldo consolidado | CT02 | Coberto |
| **CA03** | Vincula despesa no Cartão de Crédito automaticamente à fatura do mês | CT03 | Coberto |
| **CA04** | Permite editar transação com recálculo de saldo | CT04 | Coberto |
| **CA05** | Permite remover transação com estorno de saldo e fatura | CT05 | Coberto |
| **CA06** | Permite cadastro com o campo Categoria opcional (em branco) | CT06 | Coberto |
| **CA07** | Impede cadastro com valor zero ou negativo (> 0 obrigatório) | CT07 | Coberto |
| **CA08** | Impede submissão com campos obrigatórios em branco | CT08 | Coberto |
| **CA09** | Suporta cadastro de transações com datas passadas e futuras | CT09 | Coberto |
| **CA10** | Trata a desvinculação de fatura ao alterar o meio de pagamento na edição | CT10 | Coberto |

---
*Plano de Testes gerado pelo esquadrão QA Agêntico em estrita conformidade com as diretrizes do framework.*
