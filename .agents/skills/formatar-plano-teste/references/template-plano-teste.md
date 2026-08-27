# Plano de Testes: Módulo [Nome do Módulo]

## 1. Informações Gerais & Insumos
* **Módulo:** [ex: autenticacao / carrinho / checkout]
* **Documento de Requisitos:** `docs/requisitos-refinados/[modulo].md`
* **Contexto de Negócio:** `.agents/context/[modulo].md`
* **Data de Criação:** [AAAA-MM-DD]
* **Autor:** Analista de Testes (Agent 02)

## 2. Pré-condições & Ambiente
* **URL / Ponto de Entrada:** [ex: https://app.exemplo.com/login]
* **Estado do Sistema:** [ex: Banco de dados zerado / Usuário cadastrado]
* **Navegadores Homologados:** Chromium, Firefox, WebKit (via Playwright)

## 3. Massa de Dados Recomendada
* **Dados Válidos:**
  * Usuário: `teste_qa@exemplo.com` / Senha: `SenhaValida123!`
  * Produto ID: `101` (Em estoque)
* **Dados Inválidos / Exceção:**
  * E-mail fora do padrão: `usuario_invalido.com`
  * Credenciais inexistentes: `nao_existe@exemplo.com` / `123456`

---

## 4. Casos de Teste Detalhados

### CT01: [Título do Caso de Teste - Caminho Feliz]
* **Objetivo:** Validar que [ação principal] funciona com sucesso atendendo ao fluxo padrão.
* **Pré-condições:** [ex: Usuário na página inicial sem sessão ativa]
* **Massa de Dados:** [ex: Credenciais válidas]
* **Passos de Execução:**
  1. Navegar até a URL `[URL]`.
  2. Preencher o campo `[Nome do Campo]` com `[Valor]`.
  3. Clicar no botão `[Nome do Botão]`.
  4. Observar o redirecionamento para a tela `[Tela de Destino]`.
* **Resultado Esperado:** O sistema exibe a mensagem `[Mensagem exata]` e o elemento `[Seletor/Texto]` fica visível na tela.

---

### CT02: [Título do Caso de Teste - Fluxo de Exceção / Validação]
* **Objetivo:** Validar o bloqueio ao tentar [ação inválida] sem preencher os campos obrigatórios.
* **Pré-condições:** [ex: Usuário no formulário de cadastro]
* **Massa de Dados:** N/A (Campos em branco)
* **Passos de Execução:**
  1. Acessar a tela de cadastro.
  2. Deixar os campos obrigatórios vazios.
  3. Clicar no botão "Submeter".
* **Resultado Esperado:** O formulário impede o envio e exibe a mensagem de validação `[Mensagem de Erro]` abaixo de cada campo obrigatório.

---

### CT03: [Título do Caso de Teste - Caso de Borda / Limites]
* **Objetivo:** Validar o comportamento do sistema ao inserir o limite máximo de caracteres no campo `[Campo]`.
* **Pré-condições:** [ex: Usuário autenticado]
* **Massa de Dados:** String com 255 caracteres
* **Passos de Execução:**
  1. Acessar a tela `[Nome da Tela]`.
  2. Digitar uma string contendo 255 caracteres no campo `[Campo]`.
  3. Salvar as alterações.
* **Resultado Esperado:** O sistema aceita até 255 caracteres e trunca qualquer caractere excedente sem gerar erro interno (HTTP 500).

---

## 5. Matriz de Rastreabilidade de Requisitos

Esta matriz garante que 100% dos Critérios de Aceite (CA) especificados pelo **Analista de Requisitos** possuem cobertura de testes planejados.

| ID do Critério de Aceite (CA) | Descrição Resumida do Critério | ID do Caso de Teste (CT) | Status da Cobertura |
| :--- | :--- | :--- | :--- |
| **CA01** | Permite login com credenciais válidas | CT01 | Coberto |
| **CA02** | Exibe alerta de erro ao falhar autenticação | CT02 | Coberto |
| **CA03** | Valida limite máximo de caracteres no formulário | CT03 | Coberto |

---
*Plano de Testes gerado pelo agente `analista-testes` conforme as especificações do esquadrão QA Agêntico.*
