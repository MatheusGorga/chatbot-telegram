# 🤖 Telegram Weather Chatbot (n8n + AI Agent)

Este repositório contém a solução avançada para o desafio da pós-graduação: um chatbot inteligente para Telegram desenvolvido no **n8n**. O sistema utiliza **AI Agents (LangChain)** para oferecer previsões climáticas humanizadas, com um sistema robusto de resiliência e tratamento de erros.

## 🚀 Diferenciais Sênior Aplicados
- **Arquitetura de Resiliência (Fallback):** O fluxo utiliza um nó de código JavaScript para garantir que o usuário receba uma resposta determinística caso a API do Gemini falhe ou não possua credenciais configuradas.
- **Diferenciação de Erros:** Sistema encadeado de validação que distingue entre:
  - **Sucesso (200 OK):** Processamento via IA.
  - **Cidade não encontrada (404):** Mensagem de orientação ao usuário.
  - **Instabilidade (Outros):** Aviso de serviço indisponível.
- **UX com Formatação Rica:** Uso de **Markdown** para destacar informações críticas (Cidade e Temperatura) em negrito, facilitando a leitura escaneável no Telegram.
- **AI Agent Contextual:** Prompt otimizado para garantir respostas estritamente em Português (pt-BR), com tom amigável e uso estratégico de emojis.
- **Validação Antecipada:** Implementada validação via Regex no input para filtrar números e caracteres especiais antes da chamada à API, economizando recursos e evitando requisições inválidas.

## 📦 Arquivos do Projeto
- `workflow-chatbot-telegram.json`: Fluxo completo exportado.
- `README.md`: Documentação técnica detalhada.
- `.env.example`: Modelo para configuração segura das variáveis de ambiente.

---

## 🛠️ Instruções de Importação

1. No seu painel do **n8n**, selecione **Import from File** e escolha o arquivo `.json` deste repositório.
2. Certifique-se de que os nós de integração (Telegram, OpenWeather e Gemini) estejam conectados às suas respectivas credenciais.
3. **Dica Sênior:** O nó `HTTP Request` foi atualizado para a versão mais recente. Caso sua instância seja antiga e apresente erro, substitua-o mantendo os parâmetros de query ativos.

## 🔑 Configuração de Credenciais

Para total portabilidade, o projeto utiliza as seguintes nomenclaturas de variáveis:

| Variável | Finalidade |
| :--- | :--- |
| `TELEGRAM_BOT_TOKEN` | Token do bot gerado pelo @BotFather. |
| `OPENWEATHER_API_KEY` | Chave de acesso à API OpenWeather. |
| `GOOGLE_GEMINI_API_KEY` | (Opcional) Chave para processamento via IA Agent. |

---

## 🧠 Lógica do Workflow

1. **Input:** O `Telegram Trigger` captura a mensagem e o nó `Edit Fields` sanitiza o texto (remoção de acentos e espaços).
2. **API Weather:** O nó `HTTP Request` consulta os dados climáticos. O parâmetro `On Error` está configurado para `Continue` para permitir o tratamento de erro posterior.
3. **Filtro de Erros:** - O primeiro `If` valida o sucesso (200) e o país (BR).
   - O segundo `If` (no caminho negativo) identifica erros 404 específicos.
4. **Inteligência:** O `AI Agent` processa os dados com temperatura baixa (0.2) para garantir precisão.
5. **Fallback:** O nó `Code - fallback` prioriza o output da IA, mas gera uma mensagem padrão em **Markdown** caso necessário.
6. **Output:** Envio da mensagem final com o `Parse Mode` definido como **Markdown**.

---

## 🧪 Como Testar

1. **Ative o workflow** no n8n.
2. **Cenário Ideal:** Envie `São Paulo, SP`. O bot deve responder com uma frase criativa em negrito.
3. **Cenário Sem IA:** Remova a credencial do Gemini; o bot deve responder com a frase padrão formatada.
4. **Cenário de Erro:** Envie um nome inválido (ex: `12345`) e receba o aviso de "Cidade não encontrada".

---

## ✅ Teste
![IMG_5FB226D2B000-1](https://github.com/user-attachments/assets/0be32ef7-9885-4de4-83f9-dbf0532999b9)



*Desenvolvido por Matheus para o desafio da Rocketseat 💜*
