---
name: gerenciar-contexto-negocio
description: Gerencia a leitura, criacao e atualizacao continua dos arquivos de contexto de negocio e rotas em .agents/context/, mantendo a memoria viva do sistema. Use quando precisar: (1) Consultar regras de negocio ou caminhos de navegacao antes de planejar/automatizar testes (modo LEITURA), (2) Registrar o contexto funcional de um novo modulo (modo CRIACAO), (3) Atualizar regras de negocio ou sincronizar Page Objects criados em pages/ (modo ATUALIZACAO).
---

# Gerenciar Contexto de Negocio

Esta skill estabelece o protocolo formal para consultar, criar e atualizar os arquivos da camada de memoria viva (`.agents/context/`) da aplicacao.

## Quando Usar
* Antes de iniciar o refinamento ou planejamento de testes de uma historia de usuario (Modo Leitura).
* Ao identificar uma funcionalidade ou modulo inedito no sistema (Modo Criacao).
* Apos a validacao de novos requisitos pelo Analista de Requisitos ou implementacao de novos Page Objects pelo Engenheiro de Automacao (Modo Atualizacao).

## Entradas

* `modulo`: Nome do modulo ou dominio em formato kebab-case (ex: `autenticacao`, `carrinho-compras`, `checkout-pagamentos`).
* `operacao`: `LEITURA`, `CRIACAO` ou `ATUALIZACAO`.
* `dados_funcionais` (obrigatorio para CRIACAO e ATUALIZACAO funcional):
  * Finalidade da funcionalidade.
  * Ponto de partida e fluxo de navegacao entre telas.
  * Pre-condicoes de estado e credenciais.
  * Regras de negocio, validacoes e cenarios de excecao.
* `dados_tecnicos` (obrigatorio para ATUALIZACAO tecnica):
  * Classes de Page Objects criadas ou modificadas em `pages/`.
  * Metodos publicos e seletores semanticos disponiveis.

## Procedimento de Execucao

### 1. Operacao: LEITURA
1. Localize o arquivo correspondente em `.agents/context/{modulo}.md`.
2. Se o arquivo existir:
   * Extraia o caminho de navegacao para instruir o fluxo de teste.
   * Extraia as regras de negocio vigentes para garantir a cobertura de assercoes.
   * Extraia a lista de Page Objects para reaproveitamento em codigo.
3. Se o arquivo nao existir:
   * Consulte `.agents/context/system-overview.md`.
   * Notifique o agente de que o modulo nao possui historico prévio.

### 2. Operacao: CRIACAO
1. Crie o arquivo `.agents/context/{modulo}.md` utilizando o template obrigatorio abaixo.
2. Preencha a visao geral, rota, caminho de navegacao e regras de negocio com base nas informacoes validadas com o usuario.
3. Mantenha a secao de Page Objects com indicativo de pendencia ate a etapa de automacao.

### 3. Operacao: ATUALIZACAO
1. Abra o arquivo `.agents/context/{modulo}.md` existente.
2. Para atualizacao funcional (Analista de Requisitos):
   * Acrescente as novas regras de negocio sem apagar regras anteriores ainda validas.
   * Atualize o caminho de telas caso o fluxo de navegacao tenha sido alterado.
3. Para atualizacao tecnica (Engenheiro de Automacao):
   * Registre as novas classes e metodos criados na pasta `pages/`.

## Estrutura Padrao do Arquivo de Contexto

Todo arquivo criado ou atualizado em `.agents/context/{modulo}.md` deve respeitar a estrutura abaixo:

```markdown
# Modulo: [Nome do Modulo]

## 1. Visao Geral
[Finalidade deste modulo no sistema]

## 2. Fluxo de Acesso e Pre-condicoes
* Ponto de Partida: [Ex: Home / Login / Visitante]
* Caminho de Telas: [Ex: Home > Catalogo > Carrinho > Checkout]
* Estado Minimo: [Ex: Usuario autenticado, carrinho com itens]

## 3. Regras de Negocio e Validacoes
* RN01: [Descricao da regra ou comportamento esperado]
* RN02: [Descricao de restricoes funcionais ou bloqueios]
* RN03: [Comportamento em cenarios de excecao]

## 4. Catalogo de Page Objects
* `pages/[NomeDoPageObject].ts`:
  * `[metodoDeAcao]()`: [Descricao da acao executada]

## 5. Dados de Teste Recomendados
* [Massa de dados padrao e credenciais recomendadas]
```

## Regras e Diretrizes
Preservacao de dados: nunca sobrescrever um arquivo apagando regras anteriores sem confirmacao explicita do usuario.

Padrao de escrita: usar sempre kebab-case para nomes de modulos e arquivos dentro de .agents/context/.

Conformidade tecnica: manter os nomes de metodos e Page Objects rigorosamente sincronizados com os arquivos da pasta pages/.