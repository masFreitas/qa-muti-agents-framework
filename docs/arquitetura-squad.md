# Arquitetura do Squad de QA Agêntico & Skills

> **QA Multi-Agents Framework** - Estrutura de Agentes, Skills, Memória Contextual Persistente e Pontes Multi-IDE.

---

## 1. Visão Geral da Arquitetura

O **QA Multi-Agents Framework** segue o conceito de **Agentic Workspace com Living Knowledge Base (Memória Viva)**. A inteligência do squad é estruturada em três pilares fundamentais:

1. **Agentes (`.agents/agents/`)**: Personas especializadas com objetivos de negócio, autonomia de decisão e papéis definidos no ciclo de testes.
2. **Skills Reutilizáveis (`.agents/skills/`)**: Padrões técnicos, templates estruturais, utilitários e comandos de execução consumidos pelos agentes.
3. **Memória Contextual Persistente (`.agents/context/`)**: Camada de conhecimento funcional viva que registra a visão global do sistema (`system-overview.md`), rotas e o catálogo de Page Objects por módulo.

---

## 2. Diagrama Arquitetural & Fluxo de Trabalho (Mermaid)

```mermaid
flowchart TD
    subgraph MultiIDE[" Bridge Multi-Editor"]
        AG["Anti-Gravity (.agents/)"]
        CL["Claude (CLAUDE.md)"]
        CR["Cursor (.cursor/rules/)"]
        CP["GitHub Copilot (.github/)"]
    end

    subgraph UserInput[" Entradas do Usuário / Negócio"]
        US["User Story / Requisito Bruto"]
    end

    subgraph MemoryLayer[" Camada de Memória Viva (.agents/context/)"]
        SO["system-overview.md\n(Mapa Global & Rotas)"]
        CTX["{modulo}.md\n(Regras, Rotas & Catálogo POM)"]
    end

    subgraph Squad[" Squad de Agentes (.agents/agents/)"]
        A1["01. Analista de Requisitos\n(analista-requisitos.agent.md)"]
        A2["02. Analista de Testes\n(analista-testes.agent.md)"]
        A3["03. Engenheiro de Automação\n(engenheiro-automacao.agent.md)"]
        A4["04. Revisor & Correção\n(revisor-correcao.agent.md)"]
        A5["05. Relator de Status\n(05-relator-status)"]
    end

    subgraph SkillSet[" Habilidades Reutilizáveis (.agents/skills/)"]
        S_SO["inicializar-system-overview"]
        S_CTX["gerenciar-contexto-negocio"]
        S_REQ["extrair-criterios-aceite"]
        S_PLN["formatar-plano-teste"]
        S_E2E["playwright-e2e"]
        S_TSK["gerenciar-tarefas-teste"]
        S_CLI["playwright-cli"]
        S_FIX["analisar-falhas-playwright"]
        S_REP["gerar-status-report"]
    end

    subgraph Artifacts[" Artefatos & Código Gerados"]
        DOC_REQ["docs/requisitos-refinados/{modulo}.md"]
        DOC_PLN["plano-teste/{modulo}-plan.md"]
        DOC_TSK["plano-teste/tarefas/{modulo}-tarefas.json"]
        POM_CODE["pages/*.page.ts (Page Objects)"]
        TEST_CODE["tests/{modulo}.spec.ts (Suítes Playwright)"]
        EXEC_LOG["playwright-report / Execution Logs"]
        REP_OUT["Relatório de Cobertura e Qualidade"]
    end

    %% Conexões do Fluxo
    US --> A1
    
    %% Agente 1
    A1 -->|Skill: inicializar-system-overview| S_SO
    A1 <-->|Leitura & Atualização| S_CTX
    S_CTX <--> MemoryLayer
    A1 -->|Skill: extrair-criterios-aceite| S_REQ
    A1 -->|Gera| DOC_REQ

    %% Agente 2
    DOC_REQ --> A2
    A2 <-->|Consulta Memória| S_CTX
    A2 -->|Skill: formatar-plano-teste| S_PLN
    A2 -->|Gera| DOC_PLN
    A2 -->|Cria Fila| DOC_TSK

    %% Agente 3
    DOC_PLN & DOC_TSK --> A3
    A3 -->|Skill: gerenciar-tarefas-teste| S_TSK
    A3 -->|Skill: playwright-e2e & playwright-cli| S_E2E & S_CLI
    A3 -->|Gera/Atualiza| POM_CODE & TEST_CODE
    A3 -->|Atualiza Catálogo POM no Contexto| S_CTX

    %% Execução & Agente 4 (Self-Healing)
    TEST_CODE -->|Execução Playwright| EXEC_LOG
    EXEC_LOG -->|Em caso de falhas| A4
    A4 -->|Skill: analisar-falhas-playwright| S_FIX
    A4 -->|Corrige Seletores/Asserts| POM_CODE & TEST_CODE

    %% Agente 5
    EXEC_LOG & DOC_TSK --> A5
    A5 -->|Skill: gerar-status-report| S_REP
    A5 -->|Gera| REP_OUT

    %% Estilização
    classDef agentStyle fill:#1e293b,stroke:#38bdf8,stroke-width:2px,color:#fff
    classDef skillStyle fill:#0f172a,stroke:#a855f7,stroke-width:1px,color:#cbd5e1
    classDef memoryStyle fill:#14532d,stroke:#22c55e,stroke-width:2px,color:#fff
    classDef artifactStyle fill:#451a03,stroke:#f97316,stroke-width:1px,color:#ffedd5
    classDef ideStyle fill:#312e81,stroke:#818cf8,stroke-width:1px,color:#e0e7ff

    class A1,A2,A3,A4,A5 agentStyle
    class S_SO,S_CTX,S_REQ,S_PLN,S_E2E,S_TSK,S_CLI,S_FIX,S_REP skillStyle
    class SO,CTX memoryStyle
    class DOC_REQ,DOC_PLN,DOC_TSK,POM_CODE,TEST_CODE,EXEC_LOG,REP_OUT artifactStyle
    class AG,CL,CR,CP ideStyle
```

---

## 3. Detalhamento do Squad de Agentes

| Agente | Arquivo de Configuração | Responsabilidade Principal | Skills Utilizadas | Entrada | Saída Esperada |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **01. Analista de Requisitos** | `.agents/agents/analista-requisitos.agent.md` | Refinar histórias de usuário, validar regras de negócio e atualizar a memória funcional | `inicializar-system-overview`, `extrair-criterios-aceite`, `gerenciar-contexto-negocio` | User Story / Requisito bruto | `docs/requisitos-refinados/{modulo}.md` |
| **02. Analista de Testes** | `.agents/agents/analista-testes.agent.md` | Criar planos de teste detalhados em passos numerados, matriz de rastreabilidade e arquivo JSON de tarefas | `formatar-plano-teste`, `gerenciar-contexto-negocio` | Requisitos Refinados + `.agents/context/` | `plano-teste/{modulo}-plan.md` e `{modulo}-tarefas.json` |
| **03. Engenheiro de Automação** | `.agents/agents/engenheiro-automacao.agent.md` | Desenvolver classes Page Object (POM), suítes de teste Playwright executáveis e atualizar status | `playwright-e2e`, `gerenciar-tarefas-teste`, `gerenciar-contexto-negocio`, `playwright-cli` | Plano de Testes + Fila JSON | `pages/*.page.ts`, `tests/{modulo}.spec.ts` e `{modulo}-tarefas.json` atualizado |
| **04. Revisor & Correção** | `.agents/agents/revisor-correcao.agent.md` | Diagnosticar falhas de execução, atualizar seletores quebrados, ajustar timeouts e restaurar testes falhos sob demanda | `analisar-falhas-playwright`, `playwright-cli`, `playwright-e2e`, `gerenciar-contexto-negocio`, `gerenciar-tarefas-teste` | Logs, Stacktraces, Specs, Keywords ("corrija o teste X") | Correção em `pages/`, `tests/`, `.agents/context/` e tarefas JSON |
| **05. Relator de Status** | `05-relator-status` | Consolidar relatórios executivos de qualidade, cobertura de requisitos e lista de bugs | `gerar-status-report` | Logs do Playwright + Tarefas JSON | Relatório executivo de status |

---

## 4. Matriz de Skills Reutilizáveis

- **`inicializar-system-overview`**: Skill de onboarding interativo para gerar `.agents/context/system-overview.md` com a URL base e o fluxo global do sistema.
- **`gerenciar-contexto-negocio`**: Protocolo de leitura, criação e atualização contínua dos arquivos de memória em `.agents/context/{modulo}.md`.
- **`extrair-criterios-aceite`**: Checklist e padrão para estruturação de critérios de aceite no padrão INVEST.
- **`formatar-plano-teste`**: Template de plano de testes com passos numerados sequenciais, matriz de rastreabilidade e schema JSON.
- **`playwright-e2e`**: Padrões de escrita do Playwright com TypeScript, Page Object Model (POM), seletores acessíveis e web-first assertions.
- **`gerenciar-tarefas-teste`**: Leitura de pendências e atualização do status de automação (`AUTOMATIZADO`/`BLOQUEADO`) no arquivo JSON de tarefas.
- **`playwright-cli`**: Habilidade de navegação, captura de snapshots, gravação de passos e inspeção DOM em tempo real via Playwright MCP Server.
- **`analisar-falhas-playwright`**: Árvore de decisão para diagnóstico de falhas em testes automatizados.
- **`gerar-status-report`**: Consolidação de métricas e relatório de qualidade.
- **`skill-creator`**: Utilitário interno para criação, validação e empacotamento de novas skills.

---

## 5. Estrutura de Pastas do Projeto

```text
qa-muti-agents-framework/
├── .agents/
│   ├── agents/            # Personas e instruções dos 5 Agentes de QA
│   ├── skills/            # Habilidades técnicas reutilizáveis
│   └── context/           # Camada de Memória Viva (system-overview.md e {modulo}.md)
├── docs/
│   ├── requisitos-refinados/  # Requisitos extraídos pelo Analista de Requisitos
│   ├── project_spec.md        # Especificação original do template
│   └── arquitetura-squad.md   # Esta documentação da arquitetura
├── plano-teste/
│   ├── {modulo}-plan.md       # Planos de teste com passos numerados e matriz
│   └── tarefas/
│       └── {modulo}-tarefas.json  # Controle de status da automação
├── pages/                 # Page Objects gerados e mantidos pela automação
├── tests/                 # Specs automatizadas do Playwright (.spec.ts)
├── CLAUDE.md              # Ponte de integração para Claude
├── .cursor/rules/         # Ponte de integração para Cursor
└── .github/               # Ponte de integração para GitHub Copilot
```
