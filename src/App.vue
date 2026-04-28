<script setup lang="ts">
import { ref, watch, nextTick } from 'vue'

interface Message {
  role: 'user' | 'bot'
  text: string
}

const messages = ref<Message[]>([])
const input = ref('')
const loading = ref(false)
const chatBody = ref<HTMLElement | null>(null)

function scrollToBottom() {
  nextTick(() => {
    if (chatBody.value) {
      chatBody.value.scrollTop = chatBody.value.scrollHeight
    }
  })
}

watch([messages, loading], scrollToBottom, { deep: true })

async function fetchBotResponse(userMessage: string): Promise<string> {
  const res = await fetch('https://chatbot-api-6s5o.onrender.com/chat', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ question: userMessage })
  })
  const data = await res.json()
  return data.answer
}

async function sendMessage() {
  if (input.value.trim() === '' || loading.value) return

  messages.value.push({ role: 'user', text: input.value.trim() })
  input.value = ''
  loading.value = true

  try {
    const reply = await fetchBotResponse(messages.value.at(-1)!.text)
    messages.value.push({ role: 'bot', text: reply })
  } finally {
    loading.value = false
  }
}

function handleKeydown(e: KeyboardEvent) {
  if (e.key === 'Enter' && !e.shiftKey) {
    e.preventDefault()
    sendMessage()
  }
}
</script>

<template>
  <div class="app-shell">
    <!-- Sidebar -->
    <aside class="sidebar">
      <div class="sidebar-brand">
        <span class="brand-name">User</span>
      </div>

      <div class="sidebar-section-title">Assistente</div>
      <div class="sidebar-info-card">
        <div class="info-row">
          <span class="info-dot online"></span>
          <span>Online agora</span>
        </div>
        <div class="info-row">
          <svg xmlns="http://www.w3.org/2000/svg" width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M12 20h9"/><path d="M16.5 3.5a2.121 2.121 0 0 1 3 3L7 19l-4 1 1-4L16.5 3.5z"/></svg>
          <span>Respostas inteligentes</span>
        </div>
        <div class="info-row">
          <svg xmlns="http://www.w3.org/2000/svg" width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="10"/><polyline points="12 6 12 12 16 14"/></svg>
          <span>Disponível 24/7</span>
        </div>
      </div>

      <div class="sidebar-section-title">Sessão</div>
      <div class="sidebar-stat">
        <span class="stat-label">Mensagens</span>
        <span class="stat-value">{{ messages.length }}</span>
      </div>

      <div class="sidebar-footer">
        <p>Feito por JONAS</p>
      </div>
    </aside>

    <!-- Main chat area -->
    <div class="chat-container">
      <!-- Header -->
      <header class="chat-header">
        
        <div class="chat-header-info">
          <span class="chat-header-name">Jonas</span>
        </div>
      </header>

      <!-- Messages -->
      <main class="chat-body" ref="chatBody">
        <!-- Empty state -->
        <div class="empty-state" v-if="messages.length === 0 && !loading">
          <div class="empty-icon">💬</div>
          <h2>Olá!</h2>
          <p>Seu assistente virtual inteligente. Como posso ajudar você hoje?</p>
          <div class="suggestions">
            <button class="suggestion-chip" @click="input = 'Quem é você?'">Quem é você?</button>
            <button class="suggestion-chip" @click="input = 'O que você pode fazer?'">O que você pode fazer?</button>
            <button class="suggestion-chip" @click="input = 'Me ajude com uma dúvida'">Me ajude com uma dúvida</button>
          </div>
        </div>

        <!-- Messages list -->
        <transition-group name="msg" tag="div" class="messages-list">
          <div
            v-for="(msg, index) in messages"
            :key="index"
            class="message-row"
            :class="msg.role"
          >
            <div v-if="msg.role === 'bot'" class="avatar bot-avatar">J</div>
            <div class="bubble" :class="msg.role">
              {{ msg.text }}
            </div>
            <div v-if="msg.role === 'user'" class="avatar user-avatar">U</div>
          </div>
        </transition-group>

        <!-- Typing indicator -->
        <div class="message-row bot" v-if="loading">
          <div class="avatar bot-avatar">J</div>
          <div class="bubble bot typing">
            <span class="dot"></span>
            <span class="dot"></span>
            <span class="dot"></span>
          </div>
        </div>
      </main>

      <!-- Footer / Input -->
      <footer class="chat-footer">
        <div class="input-wrapper">
          <textarea
            v-model="input"
            rows="1"
            placeholder="Digite uma mensagem..."
            @keydown="handleKeydown"
            :disabled="loading"
            class="chat-input"
          ></textarea>
          <button
            class="send-btn"
            @click="sendMessage"
            :disabled="loading || input.trim() === ''"
            title="Enviar"
          >
            <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><line x1="22" y1="2" x2="11" y2="13"/><polygon points="22 2 15 22 11 13 2 9 22 2"/></svg>
          </button>
        </div>
        <p class="footer-hint">Enter para enviar &nbsp;·&nbsp; Shift+Enter para nova linha</p>
      </footer>
    </div>
  </div>
</template>

