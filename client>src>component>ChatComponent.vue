<template>
  <div class="chat-container">
    <h2>💬 Real-Time Chat</h2>
    <div class="messages">
      <div v-for="(msg, index) in messages" :key="index" class="message">
        {{ msg }}
      </div>
    </div>
    <div class="input-area">
      <input v-model="newMessage" @keyup.enter="sendMessage" placeholder="Type your message..." />
      <button @click="sendMessage">Send</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      ws: null,
      messages: [],
      newMessage: ""
    }
  },
  created() {
    this.ws = new WebSocket("ws://localhost:3000")

    this.ws.onmessage = (event) => {
      const reader = new FileReader();
      reader.onload = () => {
        this.messages.push(reader.result);
      };
      reader.readAsText(event.data);
    }

    this.ws.onopen = () => {
      console.log("Connected to WebSocket server")
    }

    this.ws.onclose = () => {
      console.log("Disconnected from WebSocket server")
    }
  },
  methods: {
    sendMessage() {
      if (this.newMessage.trim() !== "") {
        this.ws.send(this.newMessage)
        this.newMessage = ""
      }
    }
  }
}
</script>

<style scoped>
.chat-container {
  max-width: 450px;
  margin: 30px auto;
  padding: 20px;
  border-radius: 15px;
  border: 1px solid #ddd;
  background: #f9f9f9;
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.1);
}

h2 {
  text-align: center;
  margin-bottom: 20px;
  color: #333;
}

.messages {
  height: 300px;
  overflow-y: auto;
  border: 1px solid #ccc;
  border-radius: 10px;
  margin-bottom: 15px;
  padding: 10px;
  background: #fff;
}

.message {
  padding: 8px 12px;
  margin-bottom: 8px;
  background: #e6f7ff;
  border-radius: 8px;
  max-width: 80%;
  word-wrap: break-word;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.05);
}

.input-area {
  display: flex;
  gap: 8px;
}

input {
  flex: 1;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 8px;
  outline: none;
  font-size: 14px;
}

button {
  padding: 10px 20px;
  border: none;
  background: #1890ff;
  color: #fff;
  border-radius: 8px;
  cursor: pointer;
  font-size: 14px;
  transition: background 0.3s;
}

button:hover {
  background: #007acc;
}

/* Responsive design */
@media (max-width: 500px) {
  .chat-container {
    padding: 15px;
  }

  .messages {
    height: 250px;
  }
}
</style>
