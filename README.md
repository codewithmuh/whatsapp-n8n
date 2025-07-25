# whatsapp-n8n
Thi is Whatsapp AI Agent (Customer support) using N8N, OpenAI and Twilio.


User Message:

User Message:  {{ $json.data.body }}

Today's date and the current time: {{ $now }}

Memory Fields Expression:

{{ $('Twilio Trigger').item.json.data.from.replace('whatsapp:', '') }}



Code Tool:

function removeAnnotations(input) {
  return input.replace(/【[^】]*】/g, '').trim();
}

// Example usage
const output = removeAnnotations(query);
return output;


Twilio - Send an SMS: 

To:

{{ $('Twilio Trigger')..from.replace('whatsapp:', '') }}


From:

{{ $('Twilio Trigger')..replace('whatsapp:', '') }}
