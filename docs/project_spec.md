# Especificação de Projeto: Template de Squad de QA Agêntico com Playwright

## 1. Visão Geral do Projeto
Criar um repositório template open-source para automação de testes de software com Inteligência Artificial. O objetivo é permitir que qualquer engenheiro de QA clone este repositório e utilize um time estruturado de agentes (agents) e habilidades (skills) diretamente dentro de seu ambiente de desenvolvimento (Anti-Gravity, Claude, Cursor, Copilot), sem depender de plataformas pagas ou frameworks complexos de orquestração em Python.

O projeto segue a abordagem de Agentic Workspace com Living Knowledge Base (camada de memória viva), garantindo que a IA compreenda o contexto completo da aplicação antes de planejar e automatizar os testes.

## 2. Princípios Arquiteturais

1. Separação de Papéis e Capacidades:
   * Agentes (.agents/agents/): Personas com objetivos de negócio, responsabilidade de julgamento e poder de decisão.
   * Skills (.agents/skills/): Padrões técnicos, templates estruturais e comandos de execução reutilizáveis.

2. Memória Contextual Persistente (.agents/context/):
   * A IA consulta arquivos de regras de negócio e rotas de navegação antes de gerar testes.
   * A cada nova entrega automatizada, a IA atualiza os arquivos de contexto com as novas telas, métodos de Page Object e regras aprendidas.

3. Multiplataforma e Compatibilidade de Editores:
   * Estrutura nativa para Anti-Gravity via .agents/.
   * Arquivos ponte leves para Claude (CLAUDE.md), Cursor (.cursor/rules/) e GitHub Copilot (.github/).

## 3. Estrutura de Diretórios Alvo

qa-agentic-workspace/
|-- .agents/
|   |-- agents/
|   |   |-- 01-analista-requisitos/
|   |   |   `-- instructions.md
|   |   |-- 02-analista-testes/
|   |   |   `-- instructions.md
|   |   |-- 03-automador-playwright/
|   |   |   `-- instructions.md
|   |   |-- 04-revisor-correcao/
|   |   |   `-- instructions.md
|   |   `-- 05-relator-status/
|   |       `-- instructions.md
|   |-- skills/
|   |   |-- extrair-criterios-aceite/
|   |   |   `-- skill.md
|   |   |-- formatar-plano-teste/
|   |   |   `-- skill.md
|   |   |-- gerenciar-contexto-negocio/
|   |   |   `-- skill.md
|   |   |-- gerar-page-object/
|   |   |   `-- skill.md
|   |   |-- gerar-spec-playwright/
|   |   |   `-- skill.md
|   |   |-- analisar-falhas-playwright/
|   |   |   `-- skill.md
|   |   `-- gerar-status-report/
|   |       `-- skill.md
|   `-- context/
|       |-- system-overview.md
|       `-- README.md
|-- pages/
|-- tests/
|-- .cursor/
|   `-- rules/
|       `-- qa-framework.mdc
|-- .github/
|   `-- copilot-instructions.md
|-- CLAUDE.md
|-- playwright.config.ts
|-- package.json
`-- README.md

## 4. Mapeamento de Agentes e Responsabilidades

### 01. Analista de Requisitos
* Responsabilidade: Ler histórias de usuário brutas, identificar regras de negócio, critérios de aceite no padrão INVEST e cenários de exceção.
* Skills associadas: extrair-criterios-aceite, gerenciar-contexto-negocio (leitura).
* Entrada: História de usuário ou descrição funcional da tarefa.
* Saída: Documento com critérios de aceite estruturados e regras validadas.

### 02. Analista de Testes
* Responsabilidade: Consultar a pasta .agents/context/ para entender dependências de tela e converter os critérios de aceite em planos de teste detalhados e cenários BDD/Gherkin.
* Skills associadas: gerenciar-contexto-negocio (leitura), formatar-plano-teste.
* Entrada: Critérios de aceite refinados e contexto do sistema.
* Saída: Plano de testes completo com cenários BDD e matriz de validação.

### 03. Engenheiro de Automação Playwright
* Responsabilidade: Criar e atualizar classes de Page Objects em pages/ e especificações de teste em tests/, garantindo o reaproveitamento de componentes e asserções semânticas.
* Skills associadas: gerar-page-object, gerar-spec-playwright, gerenciar-contexto-negocio (atualização).
* Entrada: Plano de teste validado e Page Objects existentes.
* Saída: Código Playwright (Page Objects e Specs) funcional e executável.

### 04. Engenheiro de Revisão e Correção (Self-Healing)
* Responsabilidade: Analisar logs de falha do Playwright, relatórios de execução e traces para identificar se o erro é decorrente de seletor desatualizado, timeout ou bug real da aplicação.
* Skills associadas: analisar-falhas-playwright, gerar-page-object.
* Entrada: Logs de erro, stacktrace e código de teste que falhou.
* Saída: Correção pontual do código ou abertura de relatório de bug.

### 05. Relator de Status de QA
* Responsabilidade: Consolidar o resultado da execução, cobertura de regras e lista de bugs encontrados em um relatório executivo claro para o time.
* Skills associadas: gerar-status-report.
* Entrada: Resultados de execução, casos de teste cobertos e eventuais falhas.
* Saída: Status report executivo em Markdown.

## 5. Mapeamento das Skills

1. extrair-criterios-aceite: Guia e checklist para extração e refinamento de requisitos a partir de histórias de usuário.
2. formatar-plano-teste: Padrão estrutural para planos de teste, contemplando pré-condições, passos numerados, dados de teste e resultado esperado.
3. gerenciar-contexto-negocio: Protocolo de leitura e atualização incremental dos arquivos de contexto (.agents/context/*.md) para registrar fluxos e Page Objects criados.
4. gerar-page-object: Padrão arquitetural de Page Object Model em Playwright com métodos semânticos e seletores resilientes.
5. gerar-spec-playwright: Padrão para escrita de testes com fixtures, hooks e asserções determinísticas com auto-retry.
6. analisar-falhas-playwright: Árvore de decisão para diagnóstico de falhas em testes automatizados.
7. gerar-status-report: Template e métricas para consolidação do relatório de qualidade da sprint ou ciclo de testes.

## 6. Próximos Passos no Anti-Gravity

1. Criar a estrutura de pastas indicada na seção 3.
2. Gerar os arquivos instructions.md para cada agente em .agents/agents/.
3. Gerar os arquivos skill.md para cada skill em .agents/skills/.
4. Inicializar o arquivo base .agents/context/system-overview.md.
5. Configurar os arquivos de ponte CLAUDE.md, .cursor/rules/qa-framework.mdc e .github/copilot-instructions.md.