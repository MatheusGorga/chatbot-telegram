# 🤖 Telegram Weather Chatbot - n8n

Este projeto foi desenvolvido como desafio para a Pós-Graduação, com o objetivo de criar um chatbot automatizado no Telegram utilizando a ferramenta **n8n**. O bot informa a temperatura atual de qualquer cidade brasileira consultando a API da OpenWeather.

## 🚀 Funcionalidades
- **Interface via Telegram**: Recebe nomes de cidades e estados.
- **Tratamento de Dados**: Normalização de strings (acentuação, caixa baixa e espaços) para garantir compatibilidade com a API.
- **Integração Real-time**: Consulta à API OpenWeather com parâmetros de localização e idioma.
- **Validação de Erros**: Verificação de Status Code e consistência de dados (garantindo que a cidade seja do Brasil).
- **UX Amigável**: Respostas claras com temperatura arredondada e emojis.

## 📋 Pré-requisitos
1. Uma instância do **n8n** (Docker, Desktop ou Cloud).
2. Um bot criado no Telegram via [@BotFather](https://t.me/botfather).
3. Uma conta e chave de API gratuita na [OpenWeather](https://openweathermap.org/).

## 📥 Como Importar o Workflow
1. Baixe o arquivo `workflow-chatbot-telegram.json` deste repositório.
2. No seu painel n8n, clique em **Workflows** > **Import from File**.
3. Selecione o arquivo JSON baixado.

## 🔑 Configuração de Credenciais
O workflow utiliza referências dinâmicas para segurança. Você precisará configurar:

1. **Telegram API**: No nó `Telegram Trigger`, adicione uma nova credencial e insira o seu `BOT_TOKEN`.
2. **OpenWeather API**: No nó `HTTP Request`, o parâmetro `appid` está configurado para ler a variável `{{ $vars.OPENWEATHER_API_KEY }}`. 
   - Se estiver usando variáveis de ambiente no n8n, configure-a com sua chave.
   - Caso contrário, você pode inserir sua chave diretamente no valor do parâmetro `appid` dentro do nó.

## 🧪 Como Testar
1. Ative o workflow no n8n (botão **Active**).
2. No Telegram, envie o nome de uma cidade (Ex: `Porto Alegre, RS`).
3. O bot responderá: `🌤️ A temperatura em Porto Alegre é de 22°C.`
4. Para testar o erro, envie um nome inexistente. O bot responderá: `❌ Cidade não encontrada. Use o formato Cidade,UF (ex.: São Paulo,SP).`

---
*Desenvolvido como parte do desafio da Rocketseat 💜*
