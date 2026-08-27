# Módulo: Gestão de Transações (Receitas e Despesas)

## 1. Visão Geral
Permite ao usuário autenticado realizar a gestão financeira completa (fluxo de caixa) através do cadastro, edição, exclusão e visualização de transações do tipo Entrada (income) e Saída (expense), garantindo a atualização imediata das métricas do dashboard e do saldo financeiro consolidado.

## 2. Fluxo de Acesso e Pré-condições
* **Punto de Partida:** Tela de Login (`https://love-money-simple.lovable.app/`)
* **Caminho de Telas:** Login > Dashboard > Seção / Modal de Transações
* **Estado Mínimo:** Usuário autenticado com sessão ativa (`mateusasfreitas@gmail.com`).

## 3. Regras de Negócio e Validações
* **RN01 (Tipos de Transação):** O sistema deve aceitar exclusivamente os tipos `Entrada (income)` e `Saída (expense)`.
* **RN02 (Obrigatoriedade de Campos):** Os campos `Descrição`, `Valor`, `Data` e `Meio de Pagamento` são de preenchimento obrigatório. O campo `Categoria` é opcional.
* **RN03 (Validação do Valor):** O campo `Valor` deve aceitar estritamente valores numéricos positivos maiores que zero (> 0). Valores nulos, negativos ou zerados devem ser bloqueados com mensagem de erro.
* **RN04 (Vinculação com Cartão de Crédito):** Quando o `Meio de Pagamento` selecionado for `Cartão de Crédito`, a transação de Saída deve ser vinculada automaticamente à fatura do cartão correspondente ao mês da `Data` da transação.
* **RN05 (Cálculo Financeiro):** Transações do tipo `Entrada` devem somar ao saldo geral consolidado. Transações do tipo `Saída` devem subtrair do saldo geral consolidado.
* **RN06 (Atualização em Tempo Real / Dashboard):** Qualquer inclusão, alteração ou exclusão de transação deve recalcular e atualizar imediatamente o saldo consolidado e os indicadores do dashboard principal sem necessidade de recarregar manualmente a página.
* **RN07 (Edição de Registros):** O usuário pode alterar qualquer informação de uma transação previamente salva (Descrição, Valor, Data, Categoria, Meio de Pagamento). Ao salvar, os saldos e cartões/faturas devem ser recalculados.
* **RN08 (Exclusão e Estorno):** O usuário pode remover uma transação. O registro deve ser removido das listagens e seu valor deve ser estornado dos cálculos de saldo e faturas afetadas.
* **RN09 (Suporte a Intervalos de Data):** O campo `Data` aceita qualquer data válida (passada, presente ou futura).

## 4. Catálogo de Page Objects (Pendentes de Implementação)
* `pages/LoginPage.ts`:
  * *(Pendente de registro durante a fase de automação)*
* `pages/TransactionsPage.ts`:
  * *(Pendente de registro durante a fase de automação)*
* `pages/DashboardPage.ts`:
  * *(Pendente de registro durante a fase de automação)*

## 5. Dados de Teste Recomendados
* **Credenciais:** `mateusasfreitas@gmail.com` / `Pic@12345`
* **Transação Entrada Padrão:** Descrição: `Salário Mensal`, Valor: `5000.00`, Data: `Atual`, Meio: `Pix / Transferência`
* **Transação Saída Cartão:** Descrição: `Supermercado`, Valor: `350.00`, Data: `Atual`, Meio: `Cartão de Crédito`
