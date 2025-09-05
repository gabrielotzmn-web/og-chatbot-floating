[!DOCTYPE.html](https://github.com/user-attachments/files/22182962/DOCTYPE.html)
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>OG Landscaping Chatbot</title>
  <style>
    body {
      margin: 0;
      padding: 0;
      font-family: Arial, sans-serif;
      background: transparent; /* transparente */
    }
    #chat-toggle {
      position: fixed;
      bottom: 20px;
      right: 20px;
      width: 60px;
      height: 60px;
      background: #0b5ed7;
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
      color: white;
      font-size: 28px;
      cursor: pointer;
      box-shadow: 0 5px 15px rgba(0,0,0,0.2);
      z-index: 9999;
    }
    #chat-toggle::before { content: "💬"; font-size: 28px; }
    #chatbox {
      position: fixed;
      bottom: 90px;
      right: 20px;
      width: 350px;
      height: 500px;
      background: white;
      border-radius: 12px;
      padding: 12px;
      box-shadow: 0 5px 15px rgba(0,0,0,0.3);
      overflow-y: auto;
      display: none; /* oculto al inicio */
      flex-direction: column;
      z-index: 9999;
    }
    .bot, .user {
      margin: 8px 0;
      padding: 8px 12px;
      border-radius: 8px;
      max-width: 80%;
      line-height: 1.4em;
    }
    .bot { background: #0b5ed7; color: white; }
    .user { background: #e0e0e0; color: #333; text-align: right; margin-left: auto; }
    button {
      margin: 5px 5px 0 0;
      padding: 7px 12px;
      cursor: pointer;
      border-radius: 6px;
      border: none;
      background: #0b5ed7;
      color: white;
      font-size: 14px;
    }
    input {
      padding: 6px;
      width: 85%;
      margin-top: 6px;
      border-radius: 6px;
      border: 1px solid #ccc;
    }
  </style>
</head>
<body>

<!-- Burbuja de chat -->
<div id="chat-toggle"></div>

<!-- Ventana del chat -->
<div id="chatbox"></div>

<script>
const services = [
  { name: "Install Sod", unit: "Sq Ft", price: 1.5 },
  { name: "Install Edging - Plastic", unit: "L Ft", price: 6 },
  { name: "Install Edging - Bullet Paver", unit: "L Ft", price: 15 },
  { name: "Remove Rocks", unit: "Cu Yd", price: 200 },
  { name: "Install River/Crushed Rock", unit: "Cu Yd", price: 200 },
  { name: "Remove Mulch", unit: "Cu Yd", price: 150 },
  { name: "Spread Mulch", unit: "Cu Yd", price: 150 },
  { name: "Install Paver Patio", unit: "Sq Ft", price: 50 },
  { name: "Install Retaining Wall", unit: "Sq Ft", price: 50 },
  { name: "Planting Pot (≤ 5 gal)", unit: "Unit", price: 30 },
  { name: "Labor Work", unit: "Hour", price: 45 }
];

let currentService = null;
const chatbox = document.getElementById('chatbox');
const toggleBtn = document.getElementById('chat-toggle');

function addMessage(sender, text) {
  const div = document.createElement('div');
  div.className = sender;
  div.innerHTML = text;
  chatbox.appendChild(div);
  chatbox.scrollTop = chatbox.scrollHeight;
}

function addButtons(options) {
  const div = document.createElement('div');
  options.forEach(opt => {
    const btn = document.createElement('button');
    btn.innerText = opt.label;
    btn.onclick = opt.action;
    div.appendChild(btn);
  });
  chatbox.appendChild(div);
  chatbox.scrollTop = chatbox.scrollHeight;
}

function startChat() {
  chatbox.innerHTML = "";
  addMessage('bot', 'Hi! I am a virtual assistant of OG Landscaping. What can I do for you today?');
  addButtons([
    { label: "Get a Free Estimate", action: freeEstimate },
    { label: "schedule an appointment", action: scheduleanappointment }
  ]);
}

function scheduleanappointment() {
  window.open('https://mail.google.com/mail/?view=cm&fs=1&to=gabrielotzmn@gmail.com', '_blank');
}

function freeEstimate() {
  addMessage('bot', 'Great! I’ll ask you a few questions.');
  askService();
}

function askService() {
  addMessage('bot', 'What would you like to do?');
  addButtons(services.map((s, index) => ({
    label: s.name,
    action: () => askQuantity(index)
  })));
}

function askQuantity(serviceIndex) {
  currentService = services[serviceIndex];
  addMessage('bot', `How many ${currentService.unit}?`);
  const input = document.createElement('input');
  input.type = 'number';
  input.min = 1;
  input.placeholder = `Enter quantity in ${currentService.unit}`;
  input.onkeydown = function(e) {
    if(e.key === 'Enter' && input.value) {
      calculatePrice(parseFloat(input.value));
      input.remove();
    }
  };
  chatbox.appendChild(input);
  input.focus();
}

function calculatePrice(quantity) {
  const total = (quantity * currentService.price).toFixed(2);
  addMessage('bot', `Great! Here’s an approximate calculation: <b>$${total}</b><br>This is an approximate calculation. Contact us for the final price.`);
  addButtons([
    { label: "Calculate Another Estimate", action: askService },
    { label: "Contact Us", action: contactUs }
  ]);
}

toggleBtn.addEventListener('click', () => {
  if (chatbox.style.display === "none" || chatbox.style.display === "") {
    chatbox.style.display = "flex";
    startChat();
  } else {
    chatbox.style.display = "none";
  }
});
</script>

</body>
</html>
