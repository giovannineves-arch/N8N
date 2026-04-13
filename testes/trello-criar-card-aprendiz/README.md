# Trello- Criar Card Aprendiz (ID: Zc85wyQAzpZsed9i)

Trigger: Gmail Trigger

Visão geral:
Este workflow recebe e-mails via Gmail Trigger, processa o conteúdo com um agente LangChain/OpenAI, normaliza os dados do lead (nome, email, cidade, país, idade), envia um e-mail de resumo e pode acionar um sub-workflow que trata a criação de um cartão (ex.: Trello) quando configurado.

Passo a Passo (Node a Node):

- Gmail Trigger (Gmail OAuth2 - Giovanni)
  - Dispara quando chegam e-mails conforme configuração de polling. Inicia o fluxo e passa o conteúdo do e-mail adiante.

- AI Agent (@n8n/n8n-nodes-langchain.agent)
  - Recebe o conteúdo do e-mail; usa um prompt definido ("o e-mail tem que rolar um disparo para eu saber oque tem no dia, me avise sempre um dia seguinte") para extrair ou analisar intenções e preparar dados a serem formalizados.

- OpenAI Chat Model (@n8n/n8n-nodes-langchain.lmChatOpenAi)
  - Modelo configurado com gpt-5-mini que funciona como motor de linguagem para o agente LangChain.

- Simple Memory (@n8n/n8n-nodes-langchain.memoryBufferWindow)
  - Mantém contexto por threadId (sessionKey = {{$json.threadId}}) para conversas persistentes entre mensagens relacionadas.

- HTTP Request (n8n-nodes-base.httpRequestTool)
  - Configurado para acessar https://vault.bitwarden.com/#/vault — utilizado como ferramenta auxiliar pelo agente (ex.: recuperar segredos/URLs) quando necessário.

- Formaliza dados (n8n-nodes-base.code)
  - Código JavaScript que monta um objeto lead com campos: nome, email, cidade, pais, idade. Retorna esse objeto como JSON para uso posterior.
  - Trecho importante: se não houver email, o código atualmente apenas continua (sem lançar erro) — pode-se adicionar validação adicional se necessário.

- Edit Fields (n8n-nodes-base.set)
  - Mapeia campos do JSON para variáveis com nomes estáveis (nome, email, cidade, pais, idade) e inclui outros campos presentes.

- Send a message (n8n-nodes-base.gmail)
  - Envia um e-mail para giovanni.neves@ciadeestagios.com.br com assunto "Dados Giovanni" e corpo composto pelos campos normalizados (nome, email, idade).
  - Possui webhookId e credencial Gmail configurada (Giovanni).

- Call n8n Workflow Tool (@n8n/n8n-nodes-langchain.toolWorkflow)
  - Aciona um sub-workflow identificado como NhaHePvU9oxYB5g2. Usado para lógica adicional (por exemplo: criar o cartão no Trello) quando o sub-workflow estiver implementado para isso.

Observações de segurança e credenciais:
- Existem credenciais referenciadas neste fluxo (Gmail OAuth2, OpenAI API). Verifique se os ambientes e secrets estão adequadamente protegidos antes de promover para produção.
- WebhookId e IDs sensíveis estão presentes no arquivo exportado — revisar e rotacionar se necessário.

Changelog:
- 2026-04-13: Documentação adicionada/atualizada. JSON do workflow atualizado (normalização de formato). Recomendação: validar obrigatoriedade do campo email no nó "Formaliza dados".

--
Arquivo gerado/atualizado automaticamente pelo Agente Superior de Automação.