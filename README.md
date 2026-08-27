# QA Multi-Agents Framework 🚀

> **Template Open-Source de Squad de QA Agêntico com Playwright**

Um ambiente de testes automatizados com Inteligência Artificial estruturado para funcionar diretamente dentro de sua IDE (Antigravity, Claude, Cursor, GitHub Copilot), sem depender de plataformas pagas ou frameworks complexos de orquestração.

---

## 🎯 Visão Geral

Este projeto adota o conceito de **Agentic Workspace com Living Knowledge Base (Memória Viva)**. A IA compreende o contexto de negócio, rotas e Page Objects da aplicação antes de planejar e codificar os testes automatizados em Playwright.

### 🌟 Diferenciais

- **Squad Especializado**: 5 agentes com papéis bem definidos (Requisitos, Análise, Automação, Self-Healing, Reports).
- **Skills Reutilizáveis**: Padrões estruturais para BDD, Page Objects e asserções resilientes.
- **Memória Persistente**: Contexto do sistema (`.agents/context/`) atualizado continuamente a cada nova automação.
- **Multi-Editor**: Compatível com Antigravity (`.agents/`), Claude (`CLAUDE.md`), Cursor (`.cursor/rules/`) e Copilot (`.github/`).

---

## 🤖 Squad de Agentes & Skills

| Agente | Responsabilidade | Skills Chave |
| :--- | :--- | :--- |
| **01. Analista de Requisitos** | Mapeia regras de negócio e critérios de aceite no padrão INVEST | `extrair-criterios-aceite`, `gerenciar-contexto-negocio`, `inicializar-system-overview` |
| **02. Analista de Testes** | Converte critérios de aceite em planos de teste BDD/Gherkin | `formatar-plano-teste`, `gerenciar-contexto-negocio` |
| **03. Engenheiro de Automação** | Gera e mantém Page Objects e Specs no Playwright | `gerar-page-object`, `gerar-spec-playwright` |
| **04. Revisor & Correção (Self-Healing)** | Diagnostica falhas nos relatórios/logs e corrige seletores/timeouts | `analisar-falhas-playwright` |
| **05. Relator de Status** | Consolida relatórios executivos de cobertura e qualidade | `gerar-status-report` |

---

## 💡 Como o Fluxo Opera no Primeiro Dia

1. **Início do Setup**: Você clona o template no Anti-Gravity e abre o chat com o **Analista de Requisitos**.
2. **Detecção da Memória Viva**: O agente percebe que a pasta `.agents/context/` não possui o `system-overview.md`.
3. **Onboarding Interativo**: O agente invoca a skill `inicializar-system-overview` e faz as 3 perguntas de onboarding para você.
4. **Resposta e Configuração**: Você responde com a URL base e o fluxo geral da sua empresa.
5. **Mapeamento do Sistema**: O agente salva o `system-overview.md` e, a partir desse momento, todo o time de QA passa a ter o mapa raiz da aplicação disponível para qualquer história de usuário subsequente.

---

## 📁 Estrutura do Projeto

```text
qa-muti-agents-framework/
├── .agents/
│   ├── agents/            # Instruções e personas dos 5 agentes de QA
│   ├── skills/            # Habilidades técnicas reutilizáveis (BDD, POM, etc.)
│   └── context/           # Memória viva e contexto de negócio da aplicação
├── docs/                  # Especificação e documentação do projeto
├── pages/                 # Classes de Page Object Model (POM)
├── tests/                 # Especificações de testes automatizados (.spec.ts)
├── AGENTS.md              # Regras de arquitetura do squad agêntico
├── playwright.config.ts   # Configuração do Playwright Test
└── package.json           # Dependências e scripts do projeto
```

---

## 🛠️ Pré-requisitos & Instalação

- **Node.js** (versão 18 ou superior)
- **npm** ou **yarn**

```bash
# 1. Clonar o repositório
git clone https://github.com/masFreitas/qa-muti-agents-framework.git
cd qa-muti-agents-framework

# 2. Instalar dependências
npm install

# 3. Instalar navegadores do Playwright
npx playwright install
```

---

## 🧪 Executando os Testes

```bash
# Executar todos os testes
npm test

# Executar testes com a interface gráfica do Playwright (UI Mode)
npm run test:ui

# Visualizar o relatório de testes HTML
npm run report
```

---

## 📄 Licença

Este projeto é open-source e distribuído sob a licença [ISC](LICENSE).
