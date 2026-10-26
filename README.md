# WhatsApp AI Customer Support Agent – n8n, OpenAI & Twilio

<p align="center">
  <a href="https://github.com/codewithmuh/whatsapp-n8n/releases"><img src="https://img.shields.io/github/v/release/codewithmuh/whatsapp-n8n?color=blue&label=version&style=flat-square" alt="GitHub Release"></a>
  <a href="https://github.com/codewithmuh/whatsapp-n8n/stargazers"><img src="https://img.shields.io/github/stars/codewithmuh/whatsapp-n8n?style=flat-square" alt="Stars"></a>
  <a href="https://github.com/codewithmuh/whatsapp-n8n/network/members"><img src="https://img.shields.io/github/forks/codewithmuh/whatsapp-n8n?style=flat-square" alt="Forks"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-MIT-green.svg?style=flat-square" alt="License: MIT"></a>
  <a href="https://youtube.com/@codewithmuh"><img src="https://img.shields.io/badge/YouTube-%40codewithmuh-FF0000?style=flat-square&logo=youtube&logoColor=white" alt="YouTube"></a>
  <br>
  <img src="https://img.shields.io/badge/Platform-n8n-EA4B71?style=flat-square&logo=n8n&logoColor=white" alt="n8n">
  <img src="https://img.shields.io/badge/Messaging-Twilio%20WhatsApp-F22F46?style=flat-square&logo=twilio&logoColor=white" alt="Twilio WhatsApp">
  <img src="https://img.shields.io/badge/Intelligence-OpenAI%20GPT-412991?style=flat-square&logo=openai&logoColor=white" alt="OpenAI">
  <img src="https://img.shields.io/badge/Memory-PostgreSQL-336791?style=flat-square&logo=postgresql&logoColor=white" alt="PostgreSQL">
</p>

This project sets up a production **WhatsApp AI Agent (Customer Support Assistant)** using **n8n**, **OpenAI**, **PostgreSQL**, and **Twilio**. The assistant automatically handles customer queries, provides responses based on logic and memory, and can escalate issues when needed.

```mermaid
flowchart LR
    Customer["Customer on WhatsApp"] --> TwilioIn["Twilio WhatsApp API"]
    TwilioIn --> N8N["n8n Webhook Trigger"]
    N8N --> Memory["PostgreSQL Memory<br/>(Conversation History)"]
    Memory --> OpenAI["OpenAI GPT Model<br/>(Knowledge Base + FAQ Prompt)"]
    OpenAI --> Clean["Annotation Cleaner<br/>(Custom JavaScript Tool)"]
    Clean --> TwilioOut["Twilio Message Dispatcher"]
    TwilioOut --> Customer
```

<img width="1920" height="1080" alt="DEPLOY REACT ON S3 (3)" src="https://github.com/user-attachments/assets/abbc0619-d997-4305-9b6e-bf9fbef39d1d" />

## 🔧 Tech Stack

- [n8n](https://n8n.io): Workflow automation tool
- [Twilio](https://www.twilio.com/whatsapp): WhatsApp messaging API
- [OpenAI](https://platform.openai.com/): Natural language processing for intelligent responses

---

## 💡 Features

- Receives and reads incoming WhatsApp messages via Twilio
- Processes messages using OpenAI to generate context-aware replies
- Automatically removes irrelevant annotations (e.g., 【text】) using a custom JavaScript function
- Responds back via WhatsApp through Twilio
- Memory expressions for user identification and contextual conversations
- Logs message content with timestamps for tracking

---

## 🧠 Expressions & Logic

### 📩 User Message Capture

```handlebars
User Message:  {{ $json.data.body }}
```

### 📅 Current Timestamp

```handlebars
Today's date and the current time: {{ $now }}
```

### 📱 Phone Number (Memory Field)

```handlebars
{{ $('Twilio Trigger').item.json.data.from.replace('whatsapp:', '') }}
```

---

## 🧹 Code Tool: Annotation Cleaner

This function removes surrounding annotation blocks often found in AI-generated or formatted messages:

```javascript
function removeAnnotations(input) {
  return input.replace(/【[^】]*】/g, '').trim();
}

// Example usage
const output = removeAnnotations(query);
return output;
```

---

## 📤 Sending Reply via Twilio

**To:**

```handlebars
{{ $('Twilio Trigger').item.json.data.from.replace('whatsapp:', '') }}
```

**From:**

```handlebars
{{ $('Twilio Trigger').item.json.data.to.replace('whatsapp:', '') }}
```

Use these expressions in the `Twilio - Send an SMS` node to direct replies to the right user.

---

## 🚀 Getting Started

1. **Clone this repository**

2. **Configure Environment Variables** (Twilio SID, Auth Token, OpenAI API Key, etc.)

3. **Create a Workflow in n8n**
   - Set up a **Twilio Trigger** node to listen to incoming WhatsApp messages.
   - Add a **Code node** with the annotation removal logic.
   - Use the **OpenAI node** to generate a reply.
   - Send the response using **Twilio - Send an SMS** node.

4. **Deploy the Workflow**
   - You can use n8n Cloud or self-host using Docker.

---

## 📌 Notes

- Ensure your Twilio account is WhatsApp-enabled.
- Customize your OpenAI prompt to match your support tone (e.g., friendly, concise, professional).
- Memory expressions can be expanded to store chat history for better continuity.

---

## 🤝 Contributions

Feel free to open issues or submit PRs to improve functionality, edge case handling, or integrations!

---

## 📜 License

MIT License
