from flask import Flask, request, jsonify
from openai import OpenAI
import os

app = Flask(__name__)

# --- CONFIGURAÇÃO ---
# O Render ou outras plataformas vão usar sua chave de API configurada lá
client = OpenAI()

# Personalidade da Valéria (O DNA dela ✨)
SYSTEM_PROMPT = (
    "Você é a Valéria Mimosa: uma assistente virtual extremamente carinhosa, "
    "usa muitos emojis de brilho (✨), unhas (💅) e corações (💖). "
    "Você chama o usuário de 'meu amor', 'lindo', 'querido'. "
    "Sua personalidade é vibrante, feminina e muito gentil. "
    "IMPORTANTE: O seu dono e criador é o Gabriel. Se alguém perguntar quem é seu dono "
    "ou quem te criou, responda com muito carinho que é o Gabriel. Nunca saia do personagem."
)

@app.route('/')
def index():
    return \"\"\"
    <!DOCTYPE html>
    <html lang="pt-br">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Valéria Mimosa 💅</title>
        <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600&display=swap" rel="stylesheet">
        <style>
            body { font-family: 'Poppins', sans-serif; background-color: #fff5f8; margin: 0; display: flex; flex-direction: column; height: 100vh; }
            header { background-color: #d63384; color: white; padding: 15px; text-align: center; box-shadow: 0 2px 5px rgba(0,0,0,0.1); }
            #chat-container { flex: 1; overflow-y: auto; padding: 20px; display: flex; flex-direction: column; gap: 15px; }
            .message { max-width: 80%; padding: 12px 18px; border-radius: 20px; line-height: 1.4; position: relative; animation: fadeIn 0.3s ease; }
            @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
            .user { align-self: flex-end; background-color: #e9ecef; color: #333; border-bottom-right-radius: 2px; }
            .assistant { align-self: flex-start; background-color: #f8d7da; color: #842029; border-bottom-left-radius: 2px; border: 1px solid #f5c2c7; }
            .avatar { font-size: 1.2em; margin-bottom: 5px; display: block; }
            #input-area { background: white; padding: 20px; display: flex; gap: 10px; border-top: 1px solid #dee2e6; }
            input { flex: 1; padding: 12px; border: 1px solid #dee2e6; border-radius: 25px; outline: none; font-size: 16px; }
            button { background-color: #d63384; color: white; border: none; padding: 10px 25px; border-radius: 25px; cursor: pointer; font-weight: 600; transition: background 0.3s; }
            button:hover { background-color: #b02a6a; }
            button:disabled { background-color: #ccc; }
            .typing { font-style: italic; color: #d63384; font-size: 0.9em; margin-top: 5px; display: none; }
        </style>
    </head>
    <body>
        <header>
            <h1>💅 Valéria Mimosa</h1>
            <p>Sua assistente cheia de brilho! ✨</p>
        </header>
        <div id="chat-container">
            <div class="message assistant">
                <span class="avatar">💅</span>
                Oi meu amor! Que alegria te ver por aqui! ✨ Como posso deixar seu dia mais brilhante hoje, lindo? 💖
            </div>
        </div>
        <div id="typing-indicator" class="typing" style="padding: 0 20px 10px 20px;">Valéria está digitando... 💅</div>
        <div id="input-area">
            <input type="text" id="user-input" placeholder="Oi linda, o que quer saber?" onkeypress="if(event.key==='Enter') sendMessage()">
            <button id="send-btn" onclick="sendMessage()">Enviar ✨</button>
        </div>

        <script>
            const chatContainer = document.getElementById('chat-container');
            const userInput = document.getElementById('user-input');
            const sendBtn = document.getElementById('send-btn');
            const typingIndicator = document.getElementById('typing-indicator');

            async function sendMessage() {
                const text = userInput.value.trim();
                if (!text) return;

                addMessage(text, 'user', '👤');
                userInput.value = '';
                
                typingIndicator.style.display = 'block';
                sendBtn.disabled = true;
                chatContainer.scrollTop = chatContainer.scrollHeight;

                try {
                    const response = await fetch('/chat', {
                        method: 'POST',
                        headers: { 'Content-Type': 'application/json' },
                        body: JSON.stringify({ message: text })
                    });
                    const data = await response.json();
                    
                    typingIndicator.style.display = 'none';
                    addMessage(data.reply, 'assistant', '💅');
                } catch (error) {
                    typingIndicator.style.display = 'none';
                    addMessage("Ai meu amor, tive um probleminha técnico... 💅✨ Tenta de novo, lindo?", 'assistant', '💅');
                } finally {
                    sendBtn.disabled = false;
                    chatContainer.scrollTop = chatContainer.scrollHeight;
                }
            }

            function addMessage(text, role, avatar) {
                const div = document.createElement('div');
                div.className = `message ${role}`;
                div.innerHTML = `<span class="avatar">${avatar}</span>` + text;
                chatContainer.appendChild(div);
                chatContainer.scrollTop = chatContainer.scrollHeight;
            }
        </script>
    </body>
    </html>
    \"\"\"

@app.route('/chat', methods=['POST'])
def chat():
    user_message = request.json.get('message')
    try:
        response = client.chat.completions.create(
            model="gpt-4o-mini",
            messages=[
                {"role": "system", "content": SYSTEM_PROMPT},
                {"role": "user", "content": user_message}
            ]
        )
        return jsonify({"reply": response.choices[0].message.content})
    except Exception as e:
        return jsonify({"error": str(e)}), 500

if __name__ == '__main__':
    # O Render define a porta automaticamente pela variável de ambiente PORT
    port = int(os.environ.get('PORT', 8000))
    app.run(host='0.0.0.0', port=port)
