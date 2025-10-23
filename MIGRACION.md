# Migración de signature_pad de v2.3.2 a v5.1.1

## Cambios Realizados

### 1. Actualización de package.json
- Cambiado `"signature_pad": "^2.3.2"` por `"signature_pad": "5.1.1"`

### 2. Cambios en la Importación

**Antes (v2.3.2):**
```typescript
import * as SignaturePadNative from 'signature_pad';
// o
const sp = require('signature_pad').default;
```

**Después (v5.1.1):**
```typescript
import SignaturePadLibrary from 'signature_pad';
```

### 3. Cambios en la Inicialización

**Antes (v2.3.2):**
```typescript
this.signaturePad = new SignaturePadNative.default(canvas, this.options);
this.signaturePad.onBegin = this.onBegin.bind(this);
this.signaturePad.onEnd = this.onEnd.bind(this);
```

**Después (v5.1.1):**
```typescript
this.signaturePad = new SignaturePadLibrary(canvas, this.options);

// Los eventos onBegin y onEnd ahora son eventos del DOM
this.signaturePad.addEventListener('beginStroke', () => this.onBegin());
this.signaturePad.addEventListener('endStroke', () => this.onEnd());
```

### 4. Cambios en Propiedades del Canvas

**Antes (v2.3.2):**
```typescript
const canvas = this.signaturePad._canvas; // Propiedad privada
```

**Después (v5.1.1):**
```typescript
const canvas = this.signaturePad.canvas; // Propiedad pública
```

## Nuevas Características en v5.1.1

### Nuevos Métodos
- `toSVG()`: Genera imagen SVG sin conversión a base64
- `redraw()`: Redibuja el canvas
- Soporte mejorado para `toSVG()` con opciones

### Nuevos Eventos
- `beginStroke`: Se dispara antes de comenzar un trazo (puede cancelarse con event.preventDefault())
- `endStroke`: Se dispara después de terminar un trazo
- `beforeUpdateStroke`: Se dispara antes de actualizar un trazo
- `afterUpdateStroke`: Se dispara después de actualizar un trazo

### Opciones Mejoradas
- `canvasContextOptions`: Opciones para el contexto 2D del canvas

## Testing

Para probar la nueva versión, abre el archivo `test-signature.html` en tu navegador. Este archivo incluye:
- Canvas para firmar
- Botones para limpiar, guardar PNG/SVG
- Verificación de estado vacío
- Eventos de inicio y fin de trazo

## Migración para Otros Componentes

Si tienes otros componentes que usan signature_pad, aplica estos cambios:

1. Actualiza la importación
2. Cambia `onBegin`/`onEnd` por eventos `addEventListener`
3. Usa `canvas` en lugar de `_canvas`
4. Verifica la compatibilidad con las nuevas características

## Compatibilidad

Esta migración mantiene toda la funcionalidad anterior y añade nuevas características. El comportamiento del usuario final permanece igual, pero ahora tienes acceso a:

- Mejor rendimiento
- Soporte SVG mejorado
- Más eventos para personalización
- API más moderna y consistente

## Notas Importantes

- La v5.1.1 tiene mejor soporte para dispositivos móviles
- Se mejoró la detección de gestos multi-touch
- Mejor manejo de eventos de puntero (pointer events)
- Soporte mejorado para dibujar fuera del canvas (smoothing)