# Refinamento de Requisitos: US01 - Gestão de Receitas e Despesas (Fluxo de Caixa)

## 1. Módulo e Contexto Atualizado
* **Módulo Associado:** [.agents/context/transacoes.md](file:///C:/Users/MateusArthurdaSilvad/Documents/qa-muti-agents-framework/.agents/context/transacoes.md)
* **Status da Memória:** Atualizada com novas regras de negócio, fluxos de navegação e pré-condições.

## 2. Fluxo de Navegação e Pré-condições
* **Ponto de Partida:** Tela de Autenticação (`https://love-money-simple.lovable.app/`)
* **Caminho:** Login > Dashboard Principal > Seção / Modal de Transações
* **Pré-requisitos:** 
  * Usuário registrado e autenticado com credenciais válidas (`mateusasfreitas@gmail.com`).
  * Conexão ativa com o ambiente de homologação (`https://love-money-simple.lovable.app/`).

---

## 3. Critérios de Aceite (Padrão INVEST)

### Cenários Principais (Caminho Feliz)

* **CA01 [Cadastro de Entrada (Income)]:** O sistema deve permitir cadastrar uma transação do tipo **Entrada (income)** preenchendo Descrição, Valor positivo (> 0), Data e Meio de Pagamento, somando imediatamente o valor ao saldo consolidado e atualizando os indicadores do dashboard.
* **CA02 [Cadastro de Saída (Expense)]:** O sistema deve permitir cadastrar uma transação do tipo **Saída (expense)** preenchendo Descrição, Valor positivo (> 0), Data e Meio de Pagamento (diferente de cartão de crédito), subtraindo imediatamente o valor do saldo consolidado no dashboard.
* **CA03 [Vinculação Automática com Cartão de Crédito]:** Quando o usuário selecionar o Meio de Pagamento **Cartão de Crédito** em uma despesa (Saída), o sistema deve vincular automaticamente o valor da despesa à fatura do cartão correspondente ao mês da Data informada.
* **CA04 [Edição de Transação e Recálculo de Saldo]:** O sistema deve permitir que o usuário altere qualquer campo (Descrição, Valor, Data, Meio de Pagamento, Categoria) de uma transação previamente salva, persistindo as alterações e recalculando imediatamente o saldo consolidado e a fatura vinculada.
* **CA05 [Exclusão de Transação e Estorno de Valor]:** O sistema deve permitir que o usuário remova uma transação existente, garantindo que o registro não seja mais exibido na listagem e que seu valor seja estornado do saldo consolidado e das faturas de cartão.
* **CA06 [Cadastro com Categoria Opcional]:** O sistema deve permitir salvar com sucesso transações informando ou omitindo o campo **Categoria**, sem impedir a gravação do registro.

---

### Cenários de Exceção e Regras de Bloqueio (Caminhos Alternativos)

* **CA07 [Bloqueio de Valor Zero ou Negativo]:** O sistema deve impedir a gravação de transações cujo campo **Valor** seja igual a zero (`0`) ou negativo (`< 0`), exibindo uma mensagem de validação clara (ex: *"O valor deve ser maior que zero"*).
* **CA08 [Obrigatoriedade de Campos Requeridos]:** O sistema deve impedir a submissão do formulário caso qualquer um dos campos obrigatórios (**Descrição**, **Valor**, **Data** ou **Meio de Pagamento**) esteja em branco, destacando visualmente os campos faltantes.
* **CA09 [Transação com Data Futura ou Passada]:** O sistema deve aceitar e processar corretamente cadastros de transações com datas passadas, presentes ou futuras, atualizando o saldo conforme a regra de consolidação de datas do sistema.
* **CA10 [Mudança de Meio de Pagamento na Edição]:** Ao editar uma despesa alterando o Meio de Pagamento de **Cartão de Crédito** para outro meio (ou vice-versa), o sistema deve remover o vínculo com a fatura do cartão e reajustar o saldo consolidado adequadamente.

---

## 4. Próxima Etapa do Squad
* Entregar este documento refinado ao **02-analista-testes** (`analista-testes`) para elaboração do plano de testes detalhado, matriz de rastreabilidade e cenários BDD/Gherkin.
