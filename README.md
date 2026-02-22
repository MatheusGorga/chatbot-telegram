# 🤖 Telegram Weather Chatbot (n8n + AI Agent)

Este repositório contém a solução para o desafio da pós-graduação: um chatbot inteligente para Telegram desenvolvido no **n8n**. O sistema integra a **OpenWeather API** para dados climáticos e o **Google Gemini** para geração de respostas humanizadas.

## 🚀 Diferenciais do Projeto
- **Lógica Linear com Fallback:** O fluxo foi desenhado para ser resiliente. Se a IA falhar ou não houver credenciais configuradas, um nó de código assume o controle para garantir que o usuário receba a informação de forma determinística.
- **AI Agent (LangChain):** Em vez de uma resposta estática, o bot utiliza o modelo **Gemini-1.5-Pro** para criar mensagens naturais, amigáveis e contextualizadas.
- **Normalização de Dados:** Tratamento rigoroso de strings (remoção de acentos, conversão para minúsculas e limpeza de espaços) para garantir alta taxa de sucesso nas consultas à API.

## 📦 Arquivos do Projeto
- `workflow-chatbot-telegram.json`: O fluxo completo pronto para importação no n8n.
- `README.md`: Este guia de documentação e instruções.

---

## 🛠️ Instruções de Importação

1. No seu painel do **n8n**, clique no menu no canto superior direito e selecione **Import from File**.
2. Selecione o arquivo `workflow-chatbot-telegram.json` presente neste repositório.
3. Após a importação, salve o workflow.

## 🔑 Configuração de Credenciais e Variáveis

O projeto segue boas práticas de segurança, utilizando variáveis para evitar a exposição de chaves privadas.

### 1. Telegram API
- No nó **Telegram Trigger**, selecione ou crie uma nova credencial do tipo "Telegram Bot API" e insira o seu `BOT_TOKEN` (gerado pelo @BotFather).

### 2. OpenWeather API
- O nó **HTTP Request** está configurado para buscar a chave na variável de ambiente `OPENWEATHER_API_KEY`.
- **Dica:** Caso não utilize variáveis de ambiente no seu n8n, você pode inserir sua chave diretamente no campo `Value` do parâmetro `appid` dentro do nó.

### 3. Google Gemini (Requisito Opcional)
- Para o funcionamento da IA, configure a credencial no nó **Google Gemini Chat Model** utilizando uma API Key obtida no [Google AI Studio](https://aistudio.google.com/).
- **Resiliência:** O workflow foi projetado para funcionar mesmo sem esta credencial, acionando automaticamente o modo Fallback.

---

## 🧠 Arquitetura do Workflow

O fluxo foi estruturado de forma linear para garantir uma única resposta por interação:

1. **Captura:** O `Telegram Trigger` recebe a mensagem do usuário.
2. **Tratamento:** O nó `Edit Fields` limpa e normaliza o texto da cidade.
3. **Consulta:** O `HTTP Request` consome a API OpenWeather.
4. **Validação (IF):** Filtra respostas com sucesso (Status 200) e garante que a cidade seja do Brasil (`BR`).
5. **IA (AI Agent):** O Gemini processa os dados e cria uma frase criativa. Está configurado com `On Error: Continue` para não interromper o fluxo em caso de falha.
6. **Fallback (Code):** Nó JavaScript que verifica se a IA retornou um texto válido.
   - **Prioridade:** Se a IA responder, envia a frase da IA.
   - **Segurança (Fallback):** Se a IA falhar, gera a frase padrão: *"🌤️ A temperatura em [Cidade] é de [X]°C."*
7. **Resposta:** O `Telegram Send Message` entrega o resultado final ao usuário.


---

## 🧪 Como Testar

1. Certifique-se de que o workflow está no modo **Active**.
2. No Telegram, envie o nome de uma cidade (Ex: `Belo Horizonte, MG`).
3. **Cenário Sucesso (IA):** O bot responderá com uma frase personalizada e emojis.
4. **Cenário Fallback (Sem IA):** O bot responderá: `🌤️ A temperatura em Belo Horizonte é de 25°C.`
5. **Cenário Erro:** Envie um termo aleatório para validar a mensagem: `❌ Cidade não encontrada. Use o formato Cidade,UF (ex.: São Paulo,SP).`

---
*Desenvolvido por Matheus para o desafio da Rocketseat 💜*
