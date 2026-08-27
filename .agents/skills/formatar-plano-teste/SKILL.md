---
name: formatar-plano-teste
description: Template, estrutura e diretrizes para criacao de planos de teste funcionais e exploratorios utilizando passos numerados (sem BDD/Gherkin) e matriz de rastreabilidade mapeando os Criterios de Aceite (CA). Use quando precisar: (1) Formatar ou estruturar um novo plano de teste a partir de requisitos refinados, (2) Definir casos de teste detalhados com passos e resultados esperados, (3) Mapear a matriz de rastreabilidade entre Criterios de Aceite e Casos de Teste.
---

# Formatar Plano de Teste

Esta skill estabelece as diretrizes e a estrutura padrao para geracao de **Planos de Teste com Passos Numerados** e **Matriz de Rastreabilidade**, garantindo cobertura completa dos requisitos sem o uso de sintaxe BDD/Gherkin.

## Quando Usar
* Ao receber requisitos refinados (`docs/requisitos-refinados/{modulo}.md`) e o contexto funcional (`.agents/context/{modulo}.md`).
* Para converter os Critérios de Aceite (CA) em casos de teste acionaveis e executaveis.
* Para garantir a rastreabilidade total entre o que foi especificado (CA) e o que sera testado (CT).

## Diretrizes de Estrutura

1. **Local de Armazenamento:** Os planos de teste devem ser salvos obrigatoriamente na pasta `plano-teste/` na raiz do projeto (`plano-teste/{modulo}-plan.md`).
2. **Arquivo de Tarefas JSON:** Juntamente com o plano de teste, deve ser gerado o arquivo de rastreamento de automação em `plano-teste/tarefas/{modulo}-tarefas.json` com todos os CTs inicialmente com o status `"PENDENTE"`.
3. **Formato dos Passos:** Todos os cenarios de teste devem ser detalhados com **passos numerados sequenciais** (1, 2, 3...), com acoes claras e resultados esperados associados a cada etapa importante. **NÃO UTILIZAR BDD / GHERKIN (Dado / Quando / Então)**.
4. **Massa de Dados e Pré-condições:** Cada caso de teste deve explicitar a massa de dados necessária e o estado inicial exigido da aplicação.
5. **Matriz de Rastreabilidade:** Ao final de cada plano de teste, deve haver obrigatoriamente uma tabela mapeando 100% dos Critérios de Aceite (CA01, CA02...) aos Casos de Teste (CT01, CT02...) criados.

## Fluxo de Execucao do Analista de Testes

1. **Leitura de Insumos:**
   - Ler os critérios de aceite em `docs/requisitos-refinados/{modulo}.md`.
   - Ler a memória funcional em `.agents/context/{modulo}.md` para entender rotas e fluxos.
2. **Design de Casos de Teste:**
   - Mapear o Caminho Feliz (Happy Path).
   - Mapear os Fluxos Alternativos e Excecoes (Validações de erro, campos obrigatórios, bloqueios).
   - Mapear Casos de Borda e Limites.
3. **Escrita do Plano:**
   - Utilizar o template de plano de teste em `references/template-plano-teste.md` e salvar em `plano-teste/{modulo}-plan.md`.
4. **Construção da Matriz de Rastreabilidade:**
   - Preencher a tabela final garantindo que nenhum CA fique sem pelo menos um CT cobrindo-o.
5. **Criação do Arquivo de Tarefas JSON:**
   - Inicializar `plano-teste/tarefas/{modulo}-tarefas.json` registrando cada CT criado com o status `"PENDENTE"` conforme o schema em `references/schema-tarefas-json.md`.

## Recursos Adicionais
* **Template do Plano de Teste:** Consulte [references/template-plano-teste.md](references/template-plano-teste.md) para o modelo Markdown.
* **Schema de Tarefas JSON:** Consulte [references/schema-tarefas-json.md](references/schema-tarefas-json.md) para o modelo de estado de automação.
