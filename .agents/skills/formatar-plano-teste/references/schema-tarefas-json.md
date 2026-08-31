# Schema e Estrutura dos Arquivos de Tarefa (`plano-teste/tarefas/{modulo}-tarefas.json`)

O arquivo JSON de tarefas é a ponte programática entre o **Analista de Testes (Agent 02)**, o **Engenheiro de Automação (Agent 03)** e o **Relator de Status (Agent 05)**.

## Status Válidos
* `PENDENTE`: Inicialmente atribuído pelo Analista de Testes ao criar o plano.
* `AUTOMATIZADO`: Atribuído pelo Engenheiro de Automação após codificar e verificar o teste.
* `BLOQUEADO`: Atribuído pelo Engenheiro de Automação se a automação for impedida por bug ou seletor inacessível.

## Template Vazio (Gerado pelo Analista de Testes)

```json
{
  "modulo": "[modulo]",
  "plano_teste_ref": "plano-teste/[modulo]-plan.md",
  "requisitos_ref": "docs/requisitos-refinados/[modulo].md",
  "data_criacao": "[AAAA-MM-DD]",
  "total_casos_teste": 3,
  "resumo_status": {
    "pendentes": 3,
    "automatizados": 0,
    "bloqueados": 0
  },
  "casos_teste": [
    {
      "id": "CT01",
      "titulo": "Login com credenciais válidas",
      "criterios_aceite_cobertos": ["CA01"],
      "status": "PENDENTE",
      "arquivo_spec": null,
      "motivo_bloqueio": null,
      "data_atualizacao": "[AAAA-MM-DD]"
    },
    {
      "id": "CT02",
      "titulo": "Exibição de erro para senha incorreta",
      "criterios_aceite_cobertos": ["CA02"],
      "status": "PENDENTE",
      "arquivo_spec": null,
      "motivo_bloqueio": null,
      "data_atualizacao": "[AAAA-MM-DD]"
    },
    {
      "id": "CT03",
      "titulo": "Bloqueio de conta após 3 tentativas inválidas",
      "criterios_aceite_cobertos": ["CA03", "CA04"],
      "status": "PENDENTE",
      "arquivo_spec": null,
      "motivo_bloqueio": null,
      "data_atualizacao": "[AAAA-MM-DD]"
    }
  ]
}
```

## Exemplo de Estado Atualizado (Após Ação do Agente de Automação)

```json
{
  "modulo": "autenticacao",
  "plano_teste_ref": "plano-teste/autenticacao-plan.md",
  "requisitos_ref": "docs/requisitos-refinados/autenticacao.md",
  "data_criacao": "2026-08-27",
  "total_casos_teste": 3,
  "resumo_status": {
    "pendentes": 0,
    "automatizados": 2,
    "bloqueados": 1
  },
  "casos_teste": [
    {
      "id": "CT01",
      "titulo": "Login com credenciais válidas",
      "criterios_aceite_cobertos": ["CA01"],
      "status": "AUTOMATIZADO",
      "arquivo_spec": "tests/autenticacao/ct01-login-valido.spec.ts",
      "motivo_bloqueio": null,
      "data_atualizacao": "2026-08-27"
    },
    {
      "id": "CT02",
      "titulo": "Exibição de erro para senha incorreta",
      "criterios_aceite_cobertos": ["CA02"],
      "status": "AUTOMATIZADO",
      "arquivo_spec": "tests/autenticacao/ct02-senha-incorreta.spec.ts",
      "motivo_bloqueio": null,
      "data_atualizacao": "2026-08-27"
    },
    {
      "id": "CT03",
      "titulo": "Bloqueio de conta após 3 tentativas inválidas",
      "criterios_aceite_cobertos": ["CA03", "CA04"],
      "status": "BLOQUEADO",
      "arquivo_spec": null,
      "motivo_bloqueio": "Bug na aplicação: o sistema aceita ilimitadas tentativas de senha sem bloquear o usuário (HTTP 200).",
      "detalhes_bug": {
        "titulo": "[BUG][Autenticação] Sistema não bloqueia conta após 3 tentativas com senha incorreta",
        "criticidade": "ALTA",
        "descricao": "Ao tentar realizar o login com senha incorreta 3 vezes consecutivas, a aplicação exibe mensagem de erro genérica porém não altera o status do usuário para BLOQUEADO e aceita novas tentativas ilimitadas.",
        "passo_a_passo": [
          "1. Acessar a página de Login (/login)",
          "2. Informar um e-mail válido no campo de e-mail",
          "3. Informar uma senha incorreta no campo de senha",
          "4. Clicar no botão 'Entrar' por 3 vezes consecutivas",
          "5. Tentar realizar a 4ª tentativa de login"
        ],
        "resultado_esperado": "A conta deve ser bloqueada temporariamente na 3ª tentativa com aviso visual e status HTTP 429/403.",
        "resultado_obtido": "A conta permanece ativa, permitindo a 4ª e 5ª tentativa sem qualquer restrição de segurança.",
        "evidencias": "Resposta HTTP 200 retornado em POST /api/v1/auth/login. Seletor do banner de erro: 'div.alert-error'."
      },
      "data_atualizacao": "2026-08-27"
    }
  ]
}
```
