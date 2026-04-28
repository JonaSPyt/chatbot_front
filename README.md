# Jonas — Chatbot Front-end

> **Descrição para o GitHub:**
> Interface web para o assistente virtual Jonas. Desenvolvido com Vue 3 + TypeScript + Vite, consome uma API REST de IA e oferece uma experiência de chat moderna e responsiva.

---

## Sobre o projeto

Frontend de um chatbot com interface profissional, construído com **Vue 3 (Composition API)**, **TypeScript** e **Vite**. O Jonas é um assistente virtual inteligente que se comunica com um backend Python (FastAPI) e apresenta as respostas com animações fluidas, indicador de digitação e sugestões de perguntas.

### Funcionalidades

- 💬 Chat em tempo real com backend via API REST
- ✨ Animação de entrada nas mensagens
- ⌨️ Indicador de digitação animado (três pontinhos)
- 💡 Sugestões de perguntas na tela inicial
- 📱 Layout responsivo — sidebar oculta em telas pequenas
- ⌨️ Atalho `Enter` para enviar, `Shift+Enter` para nova linha

### Tecnologias

| Camada | Tecnologia |
|--------|-----------|
| Framework | Vue 3 (Composition API + `<script setup>`) |
| Linguagem | TypeScript |
| Build tool | Vite |
| Estilo | CSS puro (sem frameworks) |
| Backend esperado | FastAPI (Python) — `POST /chat` |

---

## Configuração do projeto

### Instalar dependências

```sh
npm install
```

### Rodar em desenvolvimento

```sh
npm run dev
```

### Build para produção

```sh
npm run build
```

### Variáveis

Por padrão, o frontend aponta para `http://127.0.0.1:8000/chat`. Para alterar, edite a URL em `src/App.vue` na função `fetchBotResponse`.

---

## Estrutura

```
src/
  App.vue        # Componente principal (template + lógica)
  main.ts        # Ponto de entrada
assets/
  style.css      # Todos os estilos globais
index.html       # HTML base (inclui fonte Inter)
```
