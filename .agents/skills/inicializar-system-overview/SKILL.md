---
name: inicializar-system-overview
description: Realiza a entrevista inicial com o usuario para coletar dados globais da aplicacao e gera o arquivo .agents/context/system-overview.md.
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
Interrompa a execucao e apresente as seguintes perguntas ao usuario:
1. Qual e o nome do sistema e a URL base utilizada nos testes?
2. Quais sao os perfis de acesso padrao (ex: Visitante, Admin, Cliente) e como funciona a autenticacao?
3. Quais sao as telas principais da aplicacao e o fluxo macro de navegacao entre elas?

### 3. Geracao do Arquivo
Com as respostas, crie o arquivo `.agents/context/system-overview.md` preenchendo todos os blocos estruturais definidos no template.

## Regras e Diretrizes
* Salvar obrigatoriamente como `system-overview.md` dentro de `.agents/context/`.
* Nao assumir URLs ou credenciais ficticias; colete sempre os dados reais com o usuario.
* Registrar apenas rotas macro, deixando o detalhamento de campos e botoes para os arquivos de modulos especificos.