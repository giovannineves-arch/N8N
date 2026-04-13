# Trello- Criar Card Aprendiz

Descrição: Workflow que recebe e-mails via Gmail, formata campos do lead e envia um e-mail resumo para um responsável. Inclui um agente AI para processamento de texto e um sub-workflow chamado "My Sub-Workflow 1".

Gatilho: Gmail Trigger (conta: Giovanni)

Nodes e função lógica:
- Gmail Trigger: Monitora a caixa do Gmail configurada para novos e-mails.
- AI Agent: Recebe os dados do Gmail Trigger, usa modelo de chat para entendimento e pode acionar ferramentas externas.
- OpenAI Chat Model: Modelo gpt-5-mini ligado ao AI Agent para geração de linguagem.
- Simple Memory: Memória de contexto por thread.
- HTTP Request: Ferramenta de requisição externa usada pelo AI Agent.
- Formaliza dados (Code): Normaliza os campos do lead (nome, email, cidade, pais, idade).
- Edit Fields (Set): Mapeia campos extraídos do lead para nomes específicos.
- Send a message (Gmail): Envia um e-mail com os dados do lead para giovanni.neves@ciadeestagios.com.br.
- Call n8n Workflow Tool: Chama um sub-workflow pré-existente (ID: NhaHePvU9oxYB5g2).

Passo a passo (nó a nó):
1. Gmail Trigger captura e-mail.  
2. AI Agent processa conteúdo e, se necessário, usa OpenAI Chat Model e Simple Memory para contexto.  
3. AI Agent passa JSON ao nó Formaliza dados que normaliza os campos.  
4. Edit Fields organiza/renomeia os campos.  
5. Send a message envia o resumo por Gmail.  
6. Opcionalmente o AI Agent pode usar HTTP Request ou chamar o sub-workflow.

Changelog:
- Versão inicial adicionada em 2026-04-13 (importação via automação).
