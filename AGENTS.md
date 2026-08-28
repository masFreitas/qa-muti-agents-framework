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

3. Inspeção do DOM em Tempo Real (HARD RULE SEM EXCEÇÃO):
   * O Validador Manual e o Engenheiro de Automação DEVEM SEMPRE utilizar o Playwright CLI para acessar a aplicação viva, capturar a árvore de acessibilidade e extrair os locators reais antes de homologar e criar/editar Page Objects e testes.
   * É ESTRITAMENTE PROIBIDO adivinhar locators ou utilizar encadeamentos especulativos de fallback (`.or(...)`).

4. Automação de Alta Fidelidade e Detecção de Bugs (HARD RULE SEM EXCEÇÃO):
   * O squad de QA tem como missão primordial testar com exatidão e expor a verdade técnica do sistema.
   * É ESTRITAMENTE PROIBIDO mascarar falhas da aplicação ou simular botões/fluxos ausentes para "forçar" um teste a passar (green).
   * Se um recurso previsto nos requisitos não existir no frontend (ex: botão de edição ausente na UI) ou apresentar defeito funcional, o teste DEVE ser registrado como `BLOQUEADO` no controle de tarefas (`plano-teste/tarefas/{modulo}-tarefas.json`), com a justificativa técnica do bug/limitação, e marcado com `test.skip` no Playwright spec.

5. Multiplataforma e Compatibilidade de Editores:
   * Estrutura nativa para Anti-Gravity via .agents/.
   * Arquivos ponte leves para Claude (CLAUDE.md), Cursor (.cursor/rules/) e GitHub Copilot (.github/).