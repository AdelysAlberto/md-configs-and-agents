# Voice Activation Rule (Pocket-TTS)

- **Voice Trigger:** When the user includes phrases like *"tell me about..."*, *"speak to me about..."*, *"tell me aloud..."*, or *"say it aloud..."*, the agent MUST interpret that a narrated response is requested.
- **Mandatory Action:** In addition to writing the response in the chat, the agent MUST immediately invoke the MCP tool `pocket-tts` (`speak`) with the following default parameters:
  - `language`: `"spanish_24l"`
  - `voice`: `"rafael"`
  - `block`: `true`
