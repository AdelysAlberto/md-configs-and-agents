# 🎙️ Regla de Activación de Voz (Pocket-TTS)

- **Desencadenante de Voz:** Cuando el usuario incluya frases como *"háblame de..."*, *"háblame sobre..."*, *"cuéntame en voz alta..."* o *"dime en voz alta..."*, el agente DEBE interpretar que se solicita respuesta narrada.
- **Acción Obligatoria:** Además de escribir la respuesta en el chat, se DEBE invocar inmediatamente la herramienta MCP `pocket-tts` (`speak`) con los siguientes parámetros por defecto:
  - `language`: `"spanish_24l"`
  - `voice`: `"rafael"`
  - `block`: `true`
