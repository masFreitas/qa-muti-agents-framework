---
name: inicializar-system-overview
description: Realiza a entrevista inicial com o usuario para coletar dados globais da aplicacao e gera o arquivo .agents/context/system-overview.md. Use quando: (1) Estiver configurando um novo repositorio ou workspace de QA, (2) O arquivo .agents/context/system-overview.md nao existir ou estiver vazio, (3) For solicitada a entrevista ou setup inicial do sistema sob teste.
---

# Inicializar System Overview

Esta skill guia o setup inicial do workspace de QA, mapeando a arquitetura basica da aplicacao antes do inicio das automacoes.

## Quando Usar
* No primeiro contato com um repositorio clonado.
* Quando o arquivo `.agents/context/system-overview.md` ainda nao existir ou estiver vazio.

## Entradas
* `nome_sistema`: Nome da aplicacao sob teste.
* `url_base`: Endereco do ambiente local ou de homologacao.
* `arquitetura`: Tipo da aplicacao (Single Page Application, SSR, Mobile Web, etc.).
* `perfis_acesso`: Lista de perfis de usuario e regras de sessao.
* `rotas_principais`: Mapeamento de alto nivel das URLs e paginas core.

## Procedimento de Execucao

### 1. Checagem de Existencia
1. Verifique se `.agents/context/system-overview.md` ja existe.
2. Se existir, informe os dados atuais e pergunte se o usuario deseja apenas atualizar campos especificos.

### 2. Entrevista de Setup (Se o arquivo nao existir)
🛑 **BLOQUEIO OBRIGATORIO / PAUSA NA EXECUCAO:**
1. Interrompa IMEDIATAMENTE a criacao de qualquer arquivo.
2. Apresente as perguntas abaixo ao usuario (utilizando a ferramenta `ask_question` ou no chat):
   * Qual e o nome do sistema e a URL base utilizada nos testes?
   * Quais sao os perfis de acesso padrao (ex: Visitante, Admin, Cliente) e como funciona a autenticacao?
   * Quais sao as telas principais da aplicacao e o fluxo macro de navegacao entre elas?
3. **AGUARDE A RESPOSTA DO USUARIO.** Nao prossiga para a etapa 3 nem crie o arquivo `system-overview.md` antes de obter a resposta com os dados reais.

### 3. Geracao do Arquivo
Apenas **APOS** receber as respostas reais do usuario:
Crie o arquivo `.agents/context/system-overview.md` preenchendo todos os blocos estruturais definidos no template de referencia [references/system-overview-template.md](references/system-overview-template.md).

## Regras e Diretrizes
* **PROIBICAO DE DADOS FICTICIOS:** Jamais assuma URLs ficticias (como `localhost:3000`), nomes de sistema ou credenciais inventadas.
* **BLOQUEIO HUMANO (Human-in-the-Loop):** Se o usuario nao responder imediatamente, a execucao DEVE permanecer paralisada. Nao crie arquivos com suposicoes para "avancar no fluxo".
* Salvar obrigatoriamente como `system-overview.md` dentro de `.agents/context/`.
* Registrar apenas rotas macro, deixando o detalhamento de campos e botoes para os arquivos de modulos especificos.