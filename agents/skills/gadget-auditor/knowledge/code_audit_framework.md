# Code Hygiene & Dead Code Audit Framework - Inspector Gadget

This document details the cross-reference code auditing standards and static inspection rules enforced by **Inspector Gadget**.

---

## 1. Inspector Gadget's Audit Directives

1. **Unused API & Endpoint Detection**:
   - Escanear todas las definiciones de servicios HTTP y clientes de API.
   - Realizar búsquedas cruzadas (grep/AST) en todo el codebase (UI, hooks, componentes).
   - Reportar cualquier endpoint exportado o método que tenga 0 invocaciones en los portales públicos/privados.
2. **Semantic & HTTP Verb Discrepancies**:
   - Detectar contradicciones entre el nombre de la función y la acción real (ej. `deleteInvoicingGroup` invocando `privateApi.editInvoicingGroup`).
   - Identificar uso incorrecto de métodos HTTP (ej. `POST` o `DELETE` llamando a endpoints de edición/lectura).
3. **Dead Code & Unused Artifacts**:
   - Detectar tipos TypeScript exportados, utilidades, hooks o componentes que genuinamente jamás se utilicen.
   - Detectar endpoints sensibles expuestos (ej. `/refresh-token`) sin consumo en cliente para reducir la superficie de ataque.
4. **Pragmatic Risk Categorization**:
   - **CRITICAL**: Endpoints que ejecutan acciones destructivas con verbos incorrectos o exposición grave.
   - **WARNING**: Endpoints/servicios o funciones huérfanas sin ningún llamado en la UI.
   - **INFO**: Tipos u opciones obsoletas a limpiar.

---

## 2. Deliverable Artifact Structure

- `artifacts/code_audit.md`: Informe completo de salud de código, inventario de endpoints sin uso, contradicciones semánticas y recomendaciones de limpieza.
