```
## 📋 INSTRUCCIONES TÉCNICAS

[AQUÍ PEGAS TODO EL CONTENIDO DESDE "# Technical Project Initialization Prompt" HASTA EL FINAL]
```

---

## 🎯 RESULTADO ESPERADO

La IA generará automáticamente:

✅ Sistema de variables CSS en `src/styles/variables.css`:
```css
:root {
  --color-primary: #FF6B35;
  --color-primary-light: #ff8a5c;     /* Generado automáticamente 20-30% más claro */
  --color-primary-dark: #e65420;       /* Generado automáticamente 20-30% más oscuro */
  
  --color-secondary: #004E89;
  --color-secondary-light: #2670a6;
  --color-secondary-dark: #003a66;
  
  --color-tertiary: #FFB563;
  --color-tertiary-light: #ffc88a;
  --color-tertiary-dark: #e69c46;
  
  /* Escala de grises neutra */
  --color-gray-1: #ffffff;
  --color-gray-2: #f5f5f5;
  --color-gray-3: #e0e0e0;
  --color-gray-4: #9e9e9e;
  --color-gray-5: #424242;
  --color-gray-6: #1a1a1a;
  
  /* etc... */
}
```

✅ Todos los componentes usarán estas variables genéricas
✅ Soporte para light/dark mode automático
✅ Para cambiar de branding solo cambias los hex en un solo archivo

---

📌 **RECUERDA:** 
- Si no especificas colores secundarios/terciarios, la IA usará colores complementarios apropiados
- Si no especificas colores de estado, usará los defaults (verde, rojo, amarillo, azul)
- La IA generará automáticamente las variantes light/dark de tus colores

---
```