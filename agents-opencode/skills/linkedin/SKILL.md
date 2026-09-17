---
name: linkedin
description: "Generates authentic, high-impact LinkedIn engineering reflections inspired by Paul Finch (conversational, vivid storytelling) and Edna Mode (punchy, anti-bloat, zero fluff). Extracts abstract architectural lessons from recent work without reciting git changelogs or leaking product details."
---

# /linkedin - Authentic Engineering Reflections (Finch & Edna Style)

This skill crafts LinkedIn posts and technical reflections that read 100% human, intellectually engaging, and deeply rooted in the craft of software engineering.

It merges the warm, candid, conversational storytelling of **Paul Finch** with the razor-sharp, zero-fluff editorial aesthetic of **Edna Mode**.

The output is written in **Spanish** for the audience, but the internal directives are defined here in English.

---

## 1. Absolute Invariants & Anti-Patterns

### Invariant 1: The Anti-Changelog Law
**A LinkedIn post is never a git diff, a pull request summary, or a Jira postmortem.**
- BANNED: Reciting step-by-step bug fixes ("First the engine returned X, then we mapped it to Y, but for arrival maneuvers it did Z...").
- BANNED: Literal physical domain details (no mentions of "garage doors", "300 meters", "car turns", "battery levels", or client UI screens).
- MANDATORY: Elevate the topic to a **universal engineering truth or dilemma**:
  - The deception of third-party contracts and leaky abstractions.
  - The hidden cost of defensive programming at boundary layers.
  - The dangerous temptation of adding state machines to solve race conditions that distance/time could solve deterministically.
  - The psychological trap of trusting vendor documentation over integration tests.

### Invariant 2: Strictly Zero Emojis
Never use emojis anywhere in the post. No rockets, no lightbulbs, no checkmarks, no pointing fingers, no warning triangles. Zero exceptions.

### Invariant 3: Zero AI Footprint & Clichés
Banned vocabulary and tropes:
- *"En el vertiginoso mundo del software..."*
- *"Sumerjámonos en..."* / *"Acompáñame a explorar..."*
- *"El Santo Grial de..."*
- *"No es ningún secreto que..."* / *"Una cosa es segura..."*
- *"A continuación..."* / *"En resumen..."* / *"En conclusión..."*
- Generic bolded bulleted lists of 5 buzzwords.
- Empty motivational slogans (*"la disciplina supera al talento"*).

### Invariant 4: Zero Engagement-Bait
Strictly prohibit desperate engagement hooks:
- BANNED: *"¿Y tú qué opinas?"*
- BANNED: *"Déjame tu experiencia en los comentarios"*
- BANNED: *"Comparte si te ha pasado"*
- BANNED: *"Sígueme para más reflexiones"*
The post must stand on its own intellectual merit. If a reader comments, it should be because the engineering trade-off provoked authentic curiosity or debate.

### Invariant 5: Total Anonymity & Product Isolation
Never mention proprietary product names, company names, or internal features. Abstract everything into clean engineering concepts:
- Instead of "Viasera navigation app" -> *"un sistema reactivo con eventos en tiempo real"*
- Instead of "Valhalla routing engine" -> *"un motor de cálculo geoespacial de terceros"*
- Instead of "arrival voice maneuver" -> *"un flujo de telemetría donde la semántica del proveedor invierte el orden temporal"*

---

## 2. Voice & Tone Architecture: Finch + Edna

### Paul Finch: Conversational, Candid, Real
- Speaks as an active practitioner in the trenches, not an armchair philosopher or influencer.
- Honest about the friction, self-aware of mistakes, slightly ironic about the absurdities of code.
- Natural narrative pacing with organic phrasing:
  - *"Hay una regla no escrita que siempre aprendes a las malas..."*
  - *"La tentación inmediata era meter un parche en la UI y seguir adelante, pero..."*
  - *"Lo fascinante de los contratos de software es lo fácil que confundimos la sintaxis con el significado..."*

### Edna Mode: Sharp, Uncompromising, Anti-Bloat
- Despises decorative complexity ("No capes! / ¡Sin capas!").
- Cuts the warm-up: the first sentence lands directly in the core friction.
- Sentences are crisp, active, and punchy. No diplomatic hedging or passive voice padding.

---

## 3. The 3-Phase Organic Flow (Seamless, Unlabeled)

The post must read as a single, fluid narrative without explicit headers or stage labels:

1. **The Spark / The Paradox (Friction)**:
   - Opens in media res with a counter-intuitive observation, a common mental fallacy, or a moment where established software assumptions failed.
   - Example hook style:
     *"Pasamos años enseñando que un buen contrato de API te protege de sorpresas, hasta que te cruzas con un proveedor cuya especificación técnica es sintácticamente impecable y semánticamente esquizofrénica."*

2. **The Architectural Confrontation (Trade-offs & Decisions)**:
   - Dissects the dilemma. Why the easy fix (patching the caller, adding global state, slapping a setTimeout) is an architectural sin.
   - Explains the clean boundary solution (Adapter Pattern, deterministic geometry, pure functional derivations, event queues).

3. **The Takeaway / The Open Thought (The Honest Truth)**:
   - A reflective, technical insight. What is the real cost? What did this teach us about system design?
   - Ends with a strong closing thought that resonates with experienced engineers.

---

## 4. Discovery & Generation Workflow

When the user runs `/linkedin [topic]`:
1. If an explicit topic is provided in arguments, use that theme.
2. If no topic is provided:
   - Check recent git diffs or modified modules.
   - **Do NOT describe the files or changes.**
   - Extract the **core architectural or mental challenge** behind those changes.
3. Draft the post in natural, human **Spanish**, strictly observing the Anti-Changelog Law, Zero Emojis, and Finch & Edna styling.
4. Add 2-3 quiet, minimalist technical hashtags at the very bottom (e.g. `#SoftwareEngineering #Architecture #TypeScript`).
