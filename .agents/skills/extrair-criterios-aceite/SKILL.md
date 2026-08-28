---
name: extrair-criterios-aceite
description: >-
  Extrai e estrutura critérios de aceite no padrão INVEST a partir de histórias de usuário e validações com a camada de memória viva. Use quando precisar: (1) Refinar requisitos e histórias de usuário, (2) Estruturar critérios de aceite de caminho feliz e exceção, (3) Gerar a documentação de requisitos refinados em docs/requisitos-refinados/{modulo}.md.
---

# Extrair Critérios de Aceite (Padrão INVEST)

Esta skill orienta o **Analista de Requisitos e Shift-Left QA** na transformação de histórias de usuário e regras de negócio validadas em especificações de requisitos refinados prontas para o planejamento de testes.

---

## 1. Fluxo de Trabalho (3 Passos)

### Passo 1: Validação de Memória Viva e Pré-condições
1. Verifique se a pasta `.agents/context/` possui o arquivo `system-overview.md`.
   * 🛑 **HARD STOP:** Se `system-overview.md` não existir ou contiver dados pendentes, execute a skill `inicializar-system-overview` e aguarde as respostas do usuário. É estritamente proibido inventar dados fictícios.
2. Consulte o arquivo do módulo em `.agents/context/{modulo}.md` via skill `gerenciar-contexto-negocio`. Se o arquivo não existir, crie-o com os fluxos e pré-condições validados.

### Passo 2: Estruturação dos Critérios no Padrão INVEST
Garanta que cada critério extraído seja **I**ndependente, **N**egociável, **V**alioso, **E**stimável, **S**mall (Pequeno) e **T**estável.

Divida os critérios em duas seções obrigatórias:
* **Cenários Principais (Caminho Feliz):** Comportamentos de sucesso, preenchimento válido e fluxos padrão.
* **Cenários de Exceção e Regras de Bloqueio:** Mensagens de erro, validações de formulário (campos obrigatórios, limites numéricos) e tratamentos de falha.

### Passo 3: Persistência dos Requisitos Refinados
Salve a entrega obrigatoriamente no arquivo `docs/requisitos-refinados/{modulo}.md` utilizando a estrutura padrão abaixo.

---

## 2. Template Padrão de Entrega (`docs/requisitos-refinados/{modulo}.md`)

```markdown
# Refinamento de Requisitos: [ID da História] - [Título da Funcionalidade]

## 1. Módulo e Contexto Atualizado
* **Módulo Associado:** `.agents/context/{modulo}.md`
* **Status da Memória:** Atualizada com novas regras e fluxo de navegação.

## 2. Fluxo de Navegação e Pré-condições
* **Ponto de Partida:** [Ex: Tela de Login / Dashboard]
* **Caminho:** [Ex: Login > Dashboard > Modal Lançamentos]
* **Pré-requisitos:** [Ex: Usuário autenticado com perfil válido]

## 3. Critérios de Aceite (Padrão INVEST)

### Cenários Principais (Caminho Feliz)
* **CA01 [Título]:** O sistema deve permitir [ação] quando [condição], resultando em [resultado esperado].
* **CA02 [Título]:** O sistema deve exibir [informação/componente] após [gatilho].

### Cenários de Exceção e Regras de Bloqueio (Caminhos Alternativos)
* **CA03 [Título]:** O sistema deve bloquear [ação inválida] e exibir a mensagem [mensagem exata].
* **CA04 [Título]:** O campo [Nome] deve aceitar exclusivamente [regra de validação].

## 4. Próxima Etapa do Squad
* Entregar este documento ao **02-analista-testes** para criação do plano de testes em `plano-teste/{modulo}-plan.md` e fila de tarefas em `plano-teste/tarefas/{modulo}-tarefas.json`.
```

---

## 3. Regras e Restrições
* **NADA DE DADOS FICTÍCIOS:** Não assuma URLs ou credenciais inventadas sem validação prévia.
* **NADA DE TERMOS VAGOS:** Evite expressões como "rápido", "intuitivo" ou "fácil".
* **FOCO FUNCIONAL:** Não inclua código Playwright ou classes de Page Object neste documento.
