# QA Multi-Agents Framework 🚀

> **Template Open-Source de Squad de QA Agêntico com Playwright**

Um ambiente de testes automatizados com Inteligência Artificial estruturado para funcionar diretamente dentro de sua IDE (Antigravity, Claude, Cursor, GitHub Copilot), sem depender de plataformas pagas ou frameworks complexos de orquestração.

---

## 🎯 Visão Geral

Este projeto adota o conceito de **Agentic Workspace com Living Knowledge Base (Memória Viva)**. A IA compreende o contexto de negócio, rotas e Page Objects da aplicação antes de planejar e codificar os testes automatizados em Playwright.

### 🌟 Diferenciais

- **Squad Especializado**: 6 agentes com papéis bem definidos (Requisitos, Análise, Validação Manual, Automação, Self-Healing, Relator de Status).
- **Skills Reutilizáveis**: Padrões estruturais para BDD, Page Objects e asserções resilientes.
- **Memória Persistente**: Contexto do sistema (`.agents/context/`) atualizado continuamente a cada nova automação.
- **Multi-Editor**: Compatível com Antigravity (`.agents/`), Claude (`CLAUDE.md`), Cursor (`.cursor/rules/`) e Copilot (`.github/`).

---

## 🤖 Squad de Agentes & Skills

| Agente | Arquivo | Responsabilidade | Skills Chave |
| :--- | :--- | :--- | :--- |
| **01. Analista de Requisitos** | `.agents/agents/analista-requisitos.agent.md` | Mapeia regras de negócio e critérios de aceite no padrão INVEST | `extrair-criterios-aceite`, `gerenciar-contexto-negocio`, `inicializar-system-overview` |
| **02. Analista de Testes** | `.agents/agents/analista-testes.agent.md` | Converte critérios de aceite em planos de teste com passos numerados e matriz de rastreabilidade | `formatar-plano-teste`, `gerenciar-contexto-negocio` |
| **03. Validador Manual & Exploratório** | `.agents/agents/validador-manual.agent.md` | Homologar funcionalidades na UI viva via Playwright CLI, coletar locators reais e transicionar tarefas para `PRONTO_PARA_AUTOMATIZAR` ou `BLOQUEADO` | `validar-testes-manuais`, `playwright-cli`, `gerenciar-contexto-negocio` |
| **04. Engenheiro de Automação** | `.agents/agents/engenheiro-automacao.agent.md` | Desenvolve e mantém Page Objects (`pages/*.page.ts`), suítes por módulo (`tests/{modulo}.spec.ts`) usando locators homologados e gerencia a fila de tarefas | `playwright-e2e`, `gerenciar-tarefas-teste`, `gerenciar-contexto-negocio`, `playwright-cli` |
| **05. Revisor & Correção (Self-Healing)** | `.agents/agents/revisor-correcao.agent.md` | Diagnostica falhas nos relatórios/logs e corrige seletores/timeouts sob demanda | `analisar-falhas-playwright`, `playwright-cli`, `playwright-e2e` |
| **06. Relator de Status** | `.agents/agents/relator-status.agent.md` | Consolida relatórios executivos de cobertura e qualidade | `gerar-status-report`, `gerenciar-tarefas-teste`, `gerenciar-contexto-negocio` |

---

## 💡 Como o Fluxo Opera no Primeiro Dia

1. **Início do Setup**: Você clona o template no Anti-Gravity e abre o chat com o **Analista de Requisitos**.
2. **Detecção da Memória Viva**: O agente percebe que a pasta `.agents/context/` não possui o `system-overview.md`.
3. **Onboarding Interativo**: O agente invoca a skill `inicializar-system-overview` e faz as 3 perguntas de onboarding para você.
4. **Resposta e Configuração**: Você responde com a URL base e o fluxo geral da sua empresa.
5. **Mapeamento do Sistema**: O agente salva o `system-overview.md` e, a partir desse momento, todo o time de QA passa a ter o mapa raiz da aplicação disponível para qualquer história de usuário subsequente.
6. **Planejamento, Homologação e Automação**: O **Analista de Testes** gera os planos e tarefas (`plano-teste/tarefas/{modulo}-tarefas.json`), o **Validador Manual** homologa a aplicação viva via Playwright CLI transicionando para `PRONTO_PARA_AUTOMATIZAR`, e o **Engenheiro de Automação** implementa os Page Objects em `pages/` e os testes em `tests/{modulo}.spec.ts`, atualizando o catálogo no contexto.

---

## 📁 Estrutura do Projeto

```text
qa-muti-agents-framework/
├── .agents/
│   ├── agents/            # Instruções e personas dos 6 agentes de QA (.agent.md)
│   ├── skills/            # Habilidades técnicas reutilizáveis (playwright-e2e, gerenciar-tarefas-teste, etc.)
│   └── context/           # Memória viva e contexto de negócio da aplicação (system-overview.md, {modulo}.md)
├── docs/                  # Especificação, requisitos refinados e arquitetura do projeto
├── plano-teste/           # Planos de teste markdown e fila de tarefas JSON (tarefas/)
├── pages/                 # Classes de Page Object Model (pages/*.page.ts)
├── tests/                 # Especificações de testes automatizados por módulo (tests/{modulo}.spec.ts)
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
