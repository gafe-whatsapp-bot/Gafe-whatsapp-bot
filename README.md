gafe_barberia_chatbot/
├── app.py
├── requirements.txt
├── Procfile
├── README.md


---

1. app.py

from flask import Flask, request, jsonify
from twilio.twiml.messaging_response import MessagingResponse
import openai
import os
import random

app = Flask(__name__)

openai.api_key = os.getenv("OPENAI_API_KEY")

# Frases motivacionales estilo barbería urbana
motivational_phrases = [
    "Cada corte es una oportunidad para crear arte y confianza. ¡Vamos con todo!",
    "La vida se peina con actitud, hermano. Aquí te respondo al cien.",
    "Tu estilo habla más fuerte que mil palabras. ¡Sigue brillando!",
    "El éxito no se improvisa, se construye paso a paso, como un fade perfecto.",
    "Aquí GAFE, siempre afilado, para que tu día esté a la altura."
]

@app.route("/webhook", methods=["POST"])
def webhook():
    incoming_msg = request.values.get('Body', '').strip()
    resp = MessagingResponse()
    msg = resp.message()

    if not incoming_msg:
        msg.body("Ey, no recibí nada. Manda tu mensaje y te contesto con todo el flow.")
        return str(resp)

    # Consulta a OpenAI GPT-4
    try:
        completion = openai.ChatCompletion.create(
            model="gpt-4",
            messages=[
                {"role": "system", "content": "Eres un asistente amable, profesional, con estilo urbano y motivador."},
                {"role": "user", "content": incoming_msg}
            ],
            max_tokens=150,
            temperature=0.7
        )
        reply = completion.choices[0].message.content.strip()
    except Exception as e:
        reply = "Oops, algo pasó con la conexión a la inteligencia artificial. Intenta de nuevo más tarde."

    # Añade una frase motivacional al final (aleatoria)
    motivational = random.choice(motivational_phrases)
    full_reply = f"{reply}\n\n💈 {motivational}"

    msg.body(full_reply)
    return str(resp)

if __name__ == "__main__":
    app.run(debug=True)


---

2. requirements.txt

flask
twilio
openai


---

3. Procfile

web: python app.py


---

4. README.md

# Chatbot WhatsApp GPT - Barbería GAFE

Este proyecto es un chatbot para WhatsApp que usa la API de OpenAI GPT-4, conectado vía Twilio, con respuestas motivacionales estilo barbería urbana.
## Hi there 👋

<!--
**gafe-whatsapp-bot/Gafe-whatsapp-bot** is a ✨ _special_ ✨ repository because its `README.md` (this file) appears on your GitHub profile.

Here are some ideas to get you started:

- 🔭 I’m currently working on ...
- 🌱 I’m currently learning ...
- 👯 I’m looking to collaborate on ...
- 🤔 I’m looking for help with ...
- 💬 Ask me about ...
- 📫 How to reach me: ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...
-->
