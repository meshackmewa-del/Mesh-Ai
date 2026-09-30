let currentMessages = [];

const messagesEl = document.getElementById('messages');
const userInput = document.getElementById('userInput');
const sendBtn = document.getElementById('sendBtn');
const typingIndicator = document.getElementById('typingIndicator');
const modeSelect = document.getElementById('modeSelect');

async function handleSend() {
  const text = userInput.value.trim();
  if (!text) return;

  appendMessage('user', text);
  currentMessages.push({ role: 'user', text });
  userInput.value = '';

  typingIndicator.classList.remove('hidden');

  try {
    const res = await fetch('/api/chat', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        messages: currentMessages,
        mode: modeSelect.value
      })
    });

    const data = await res.json();
    typingIndicator.classList.add('hidden');

    if (data.text) {
      appendMessage('model', data.text);
      currentMessages.push({ role: 'model', text: data.text });
    } else {
      appendMessage('model', '⚠️ Mewa AI Error: ' + (data.error || 'No response returned from server.'));
    }
  } catch (err) {
    typingIndicator.classList.add('hidden');
    appendMessage('model', '⚠️ Network Error: Unable to connect to Mewa AI server.');
  }
}

function appendMessage(role, text) {
  const msgDiv = document.createElement('div');
  msgDiv.className = `message ${role}`;

  if (role === 'model' && window.marked) {
    msgDiv.innerHTML = marked.parse(text);

    const copyBtn = document.createElement('button');
    copyBtn.className = 'copy-btn';
    copyBtn.innerText = 'Copy';
    copyBtn.onclick = () => {
      navigator.clipboard.writeText(text);
      copyBtn.innerText = 'Copied!';
      setTimeout(() => copyBtn.innerText = 'Copy', 2000);
    };
    msgDiv.appendChild(copyBtn);
  } else {
    msgDiv.innerText = text;
  }

  messagesEl.appendChild(msgDiv);
  messagesEl.scrollTop = messagesEl.scrollHeight;
}

sendBtn.addEventListener('click', handleSend);
userInput.addEventListener('keydown', (e) => {
  if (e.key === 'Enter' && !e.shiftKey) {
    e.preventDefault();
    handleSend();
  }
});

document.getElementById('newChatBtn').addEventListener('click', () => {
  currentMessages = [];
  messagesEl.innerHTML = `
    <div class="welcome-box">
      <h1>New Conversation</h1>
      <p>How can Mewa AI help you right now?</p>
    </div>`;
});

document.getElementById('themeToggle').addEventListener('click', () => {
  document.body.classList.toggle('light-theme');
});

document.getElementById('menuToggle').addEventListener('click', () => {
  document.getElementById('sidebar').classList.toggle('open');
});
