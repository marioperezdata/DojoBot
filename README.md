# DojoBot 🥋

**Chatbot RAG para consultas y atención automatizada**

Sistema de chatbot con RAG integrado en landing page para resolución de consultas en lenguaje natural sobre documentación interna, combinado con gestión automatizada de formularios de contacto.

---

## 🎯 Qué hace

DojoBot permite a cualquier visitante de la web hacer preguntas en lenguaje natural sobre la organización y obtener respuestas precisas basadas en documentación interna real:

> *"¿Cuál es el horario de clases para adultos?"*  
> *"¿Cuánto cuesta la cuota mensual?"*  
> *"¿Hay descuento si me apunto con mi hijo?"*

Además, cuando un visitante rellena el formulario de contacto, el sistema responde automáticamente por email y notifica al propietario vía Telegram — sin intervención humana.

---

## 🏗️ Arquitectura del sistema

```
Documentación interna (PDF/DOCX)
        ↓
Embeddings (OpenAI)
        ↓
Vector Database (Supabase pgvector)
        ↓
┌─────────────────────────────────────────┐
│              n8n Orchestration           │
│                                          │
│  Usuario → Webhook → Recuperación RAG   │
│               ↓                          │
│          LLM (OpenAI)                   │
│               ↓                          │
│          Respuesta contextual            │
└─────────────────────────────────────────┘
        ↓
Widget embebido en landing page

┌─────────────────────────────────────────┐
│         Flujo de formulario de contacto  │
│                                          │
│  Formulario web → Webhook → Email auto  │
│                          → Telegram     │
└─────────────────────────────────────────┘
```

---

## 🛠️ Stack técnico

| Componente | Tecnología |
|---|---|
| Vector Database | Supabase (pgvector) |
| Embeddings | OpenAI text-embedding-ada-002 |
| LLM | OpenAI GPT-4o-mini |
| Orquestación | n8n |
| Notificaciones | Email + Telegram Bot API |
| Frontend | HTML/CSS + n8n Chat Widget |
| Hosting | EasyPanel |

---

## 📁 Estructura del proyecto

```
dojobot/
├── aikido-valdemoro-landing.html     # Landing page con widget integrado
├── integrar_chatbot_en_web.txt       # Guía de integración del widget
├── README.md
└── .gitignore
```

> ⚠️ Los workflows de n8n, documentación de la organización y system prompts no están incluidos por privacidad.

---

## ⚙️ Cómo funciona el RAG

1. **Ingesta**: la documentación interna se procesa y divide en chunks
2. **Embeddings**: cada chunk se convierte en vector con OpenAI Embeddings
3. **Almacenamiento**: los vectores se guardan en Supabase (pgvector)
4. **Consulta**: cuando el usuario pregunta, se buscan los chunks más relevantes por similitud semántica
5. **Generación**: el LLM genera una respuesta contextualizada con los chunks recuperados

---

## 🔔 Flujo de formulario automatizado

Cuando un usuario envía el formulario de contacto:

1. El webhook de n8n recibe los datos (nombre, email, consulta)
2. Se envía un email de respuesta automática personalizado al usuario
3. Se notifica al propietario vía Telegram con los datos del contacto
4. Todo queda registrado para trazabilidad

---

## 🚀 Integración del widget

```html
<link href="https://cdn.jsdelivr.net/npm/@n8n/chat/dist/style.css" rel="stylesheet" />

<script type="module">
import { createChat } from 'https://cdn.jsdelivr.net/npm/@n8n/chat/dist/chat.bundle.es.js';
createChat({
  webhookUrl: 'TU_WEBHOOK_URL',
  defaultLanguage: 'es',
  initialMessages: [
    'Hola! 👋',
    'Soy DojoBot – ¿En qué puedo ayudarte?'
  ],
});
</script>
```

---

## 📋 Requisitos para replicar

- Cuenta en **Supabase** con extensión pgvector activada
- API key de **OpenAI**
- Instancia de **n8n** (self-hosted o cloud)
- **Telegram Bot** para notificaciones

---

## 🔄 Estado del proyecto

- [x] Pipeline RAG con Supabase + OpenAI Embeddings
- [x] Flujo conversacional con memoria de sesión
- [x] Formulario de contacto con respuesta automática por email
- [x] Notificación al propietario vía Telegram
- [x] Widget embebido en landing page
- [ ] Panel de administración para actualizar la base de conocimiento
- [ ] Métricas de uso y calidad de respuestas

---

## 👤 Autor

**Mario Pérez Ramos** — AI Engineer  
[LinkedIn](https://linkedin.com/in/mario-pérez-ramos) · [GitHub](https://github.com/marioperezdata)
