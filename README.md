# 🤖 Telegram Weather Chatbot (n8n + AI Agent)

Este repositório contém a solução aprimorada para o desafio da pós-graduação: um chatbot inteligente para Telegram desenvolvido no **n8n**. O sistema integra a **OpenWeather API** para dados climáticos e o **Google Gemini** para geração de respostas humanizadas.

## 🚀 Diferenciais do Projeto
- **Lógica Linear com Fallback:** Fluxo resiliente que utiliza um nó de código para garantir resposta determinística caso a IA falhe.
- **Tratamento de Erros Granular:** Diferenciação entre cidade não encontrada (Erro 404) e instabilidade nos serviços externos (Erro 500), melhorando a comunicação com o usuário.
- **AI Agent (LangChain):** Uso do modelo **Gemini-1.5-Pro** para criar mensagens naturais e contextualizadas em Português (pt-BR).
- **UX Aprimorada:** Respostas formatadas com **Markdown** (negrito e emojis) para maior legibilidade no Telegram.
- **Normalização de Dados:** Sanitização completa do input (acentos, caixa e espaços) via expressões JavaScript.

## 📦 Arquivos do Projeto
- `workflow-chatbot-telegram.json`: Fluxo completo (versão atualizada).
- `README.md`: Documentação técnica.
- `.env.example`: Modelo para configuração de variáveis de ambiente.

---

## 🛠️ Instruções de Importação

1. No seu painel do **n8n**, clique no menu superior direito e selecione **Import from File**.
2. Selecione o arquivo `workflow-chatbot-telegram.json`.
3. **Importante:** Caso o nó `HTTP Request` apresente erro de versão, delete-o e insira a versão mais recente disponível na sua instância, conectando-o entre os nós `Edit Fields` e `If`.
4. Após a importação, salve e ative o workflow.

## 🔑 Configuração de Credenciais

O projeto utiliza o sistema de credenciais do n8n e variáveis de ambiente para garantir a portabilidade e segurança (evitando exposição de chaves no JSON).

| Variável | Nó correspondente | Finalidade |
| :--- | :--- | :--- |
| `TELEGRAM_BOT_TOKEN` | Telegram Trigger / Send Message | Comunicação com o BotFather. |
| `OPENWEATHER_API_KEY` | HTTP Request | Acesso aos dados meteorológicos. |
| `GOOGLE_GEMINI_API_KEY` | Gemini Chat Model | Processamento de linguagem natural. |

---

## 🧠 Arquitetura do Workflow

1. **Captura:** O `Telegram Trigger` recebe o texto do usuário.
2. **Sanitização:** O nó `Edit Fields` aplica Regex e normalização NFD para tratar o input.
3. **Consulta:** O `HTTP Request` consome a API OpenWeather.
4. **Validação (IF):** - **Sucesso (200):** Encaminha para a IA.
   - **Não Encontrado (404):** Responde que a cidade não foi localizada.
   - **Erro de Serviço (500):** Informa instabilidade momentânea.
5. **IA (AI Agent):** O Gemini gera uma frase criativa. Configurado com `On Error: Continue` para garantir a continuidade do fluxo.
6. **Fallback (Code):** Garante que, se a IA falhar ou a credencial estiver ausente, o bot envie: *"🌤️ A temperatura em **[Cidade]** é de **[X]°C**."*
7. **Resposta:** Envio final via Telegram com **Parse Mode: Markdown**.



---

## 🧪 Como Testar

1. Certifique-se de que o workflow está **Active**.
2. No Telegram, envie o nome de uma cidade (Ex: `Rio de Janeiro, RJ`).
3. **Cenário Sucesso (IA):** O bot responderá com uma frase personalizada em negrito.
4. **Cenário Fallback (Sem IA):** O bot responderá a temperatura de forma clara e determinística.
5. **Cenário Erro (404):** Envie `CidadeInexistente123` para testar a orientação de formato.

---
*Desenvolvido por Matheus para o desafio da Rocketseat 💜*
