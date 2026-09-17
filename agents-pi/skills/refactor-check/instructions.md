# Refactor Check Procedure

Protocolo determinista para verificar cualquier refactor antes de emitir cambios:

1. **Límites de archivo**: Comprobar que ningún archivo modificado o nuevo supere 250 LOC.
2. **Higiene de imports**: Eliminar imports sin uso y referencias obsoletas.
3. **Contrato de Result Pattern**: Verificar que los servicios no lancen excepciones (`throw`) y retornen `{ success, data } | { success, error }`.
4. **Verificación de Biome y TypeScript**:
   ```bash
   bun run biome:check && bun run check
   ```
5. **Comprobación de Tests**:
   ```bash
   bun test
   ```
