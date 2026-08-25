No guardé todo el análisis porque la memoria no me sirve como copia del repositorio; me sirve como índice corto de decisiones, convenciones y atajos que conviene recordar.

# Uso de memoria: qué guardo, cuándo y por qué

Este documento explica el comportamiento observado al guardar memoria en el sistema de memoria persistente del agente. La intención es que puedas usarlo como referencia para tu propio gestor de memoria o para decidir qué tipo de información merece persistencia.

## Qué pasó en este caso

Después de analizar la lógica de Gentle-AI y escribir la guía de personalidad operativa, guardé una nota corta en memoria de repositorio. No guardé el texto completo del análisis; guardé solo los rasgos que era razonable reutilizar en futuras tareas.

La memoria creada fue esta:

- Gentle-AI's core agent logic: ask for the outcome, choose the smallest useful route, and keep delivery policy separate from review.
- Key rules: direct inline for 1-3 files or one understood mechanical file; delegated direct for 4+ files or multi-file writes; SDD only for substantial ambiguity.
- Anti-hallucination: distinguish facts, inferences, and assumptions; ask only when scope/risk/permissions/side effects/residual risk change.
- Token economy: start from a concrete anchor, do not reread without a new hypothesis, use fresh context for review/long sessions, and pass exact SKILL.md paths rather than summaries.

## En qué me basé para guardarlo

Me basé en dos señales:

1. El contenido era estable y reutilizable.
2. El contenido resumía una decisión operativa del proyecto, no una anécdota pasajera.

La información venía directamente de archivos fuente del repositorio, principalmente README, documentación de arquitectura, reglas de disparo, modelo mental y el renderer de routing. Por eso la nota no es una opinión ni una inferencia libre: es una síntesis de reglas reales del proyecto.

## Qué criterio uso para decidir si algo merece memoria

Guardo memoria cuando encuentro información que probablemente volverá a importar en otra sesión. En la práctica, eso suele ser una de estas cosas:

- una convención del repositorio;
- una regla de flujo de trabajo que no conviene releer desde cero cada vez;
- una limitación o borde importante del sistema;
- una preferencia de estilo del usuario o del proyecto;
- un comando o patrón que ahorra tiempo y evita repetir búsqueda;
- una corrección útil sobre algo que yo podría olvidar o interpretar mal.

No guardo memoria solo porque algo sea interesante. La memoria tiene que reducir trabajo futuro o evitar repetir errores.

## Qué no guardo

No guardo, por defecto:

- transcripciones completas de análisis largos;
- texto redundante que ya vive bien en un archivo fuente;
- detalles efímeros de una sesión que no cambian decisiones futuras;
- ruido conversacional;
- supuestos sin confirmar;
- material sensible o secreto que no debería persistir sin necesidad.

Si la información ya existe mejor en un documento del repositorio, prefiero no duplicarla en memoria salvo que necesite un recordatorio muy corto.

## Por qué la guardo de esa manera

La memoria persistente funciona mejor como índice compacto de decisiones, no como copia total del razonamiento.

Eso tiene tres ventajas:

- reduce tokens al volver a consultar un tema;
- evita duplicar fuente de verdad;
- hace más fácil detectar qué es una regla y qué es solo contexto de una sesión.

En otras palabras: la memoria debe ser un recordatorio de alto valor, no un archivo espejo del repositorio.

## Cómo decido el texto exacto que guardo

Cuando escribo una memoria, reduzco el contenido a una versión que cumpla estas condiciones:

- sea corta;
- sea accionable;
- sea estable;
- sea fácil de buscar;
- conserve la regla esencial;
- elimine adornos y repeticiones.

Si la regla se puede expresar en una sola línea, normalmente prefiero una sola línea. Si necesita algo más, uso viñetas breves.

## Qué tipo de texto sí termina en memoria

Normalmente guardo:

- reglas de routing o flujo de trabajo;
- decisiones repetibles;
- convenciones de un repositorio;
- nombres de archivos o rutas clave;
- patrones de validación que funcionaron;
- límites o trampas que conviene recordar.

En este caso, el texto guardado capturó justamente eso: la lógica de Gentle-AI para orquestar trabajo con rutas mínimas, foco y evidencia.

## Qué tipo de texto no termina en memoria

No suelo guardar:

- explicaciones extensas ya presentes en un documento;
- narraciones de por qué elegí algo si la decisión ya quedó resumida;
- listas largas de referencias;
- contenido que cambia a cada sesión;
- diagnósticos no confirmados.

## Cuándo creo una memoria

Creo memoria cuando noto una de estas situaciones:

- acabo de descubrir una regla del proyecto que es importante no perder;
- el trabajo produjo un patrón que probablemente volveré a aplicar;
- el usuario mostró una preferencia útil para futuras respuestas;
- hay un criterio de trabajo que conviene recordar de forma compacta;
- una tarea generó un aprendizaje que vale más como regla que como historia.

No espero a que el usuario lo pida si la información ya es claramente útil para el futuro.

## Por qué lo hice en este caso

Lo hice porque la tarea produjo una síntesis de valor duradero:

- el proyecto prioriza rutas mínimas;
- la revisión está separada de la entrega;
- la ambigüedad se resuelve con preguntas puntuales, no con exploración excesiva;
- la memoria y el skill loading son parte del ahorro de tokens.

Esas ideas no son solo útiles para esta conversación. También sirven para futuras tareas sobre Gentle-AI y para cualquier agente que quieras entrenar con una personalidad operativa parecida.

## Cómo leer una memoria de este tipo

Una memoria no es la verdad completa del proyecto. Es una pista compacta.

Úsala así:

1. como recordatorio rápido;
2. como puntero hacia la regla o convención;
3. como contexto para no empezar desde cero;
4. como disparador para volver al archivo fuente cuando haga falta detalle.

No la uses como sustituto de la documentación original si necesitas precisión completa.

## Cómo puedes usar esto en tu gestor de memoria

Si vas a diseñar tu propio sistema, una regla práctica sería:

- guarda solo lo que reduzca trabajo futuro;
- resume a la mínima forma que preserve la decisión;
- separa memoria estable de notas de sesión;
- no copies fuentes largas si puedes guardar una referencia corta;
- revisa periódicamente si la memoria sigue siendo verdadera o útil.

## Regla corta

Si una pieza de información no cambia una decisión futura, probablemente no merece memoria.

Si cambia una decisión futura, pero puede guardarse en una frase corta, sí merece memoria.
