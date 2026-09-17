# ScreenLayout & Layout Wrapper Patterns

Usa siempre la composición por children:

```tsx
<ScreenLayout hasBack hasGradient title="...">
  {children}
</ScreenLayout>
```

Evita layouts manuales con `StyleSheet` repetidos en cada vista.

## Invariantes de ScreenLayout
1. **Composición declarativa**: Todas las pantallas deben recibir sus slots estructurales a través de `ScreenLayout`.
2. **Cero duplicación**: Prohibido instanciar `LinearGradient`, headers personalizados o botones de retroceso directamente dentro del cuerpo de la pantalla.
3. **Control de líneas**: Si una pantalla se aproxima a 200 LOC, extrae sub-componentes o hooks (`useScreenData`) antes de agregar más JSX.
