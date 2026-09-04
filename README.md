# Visualizador 3D de tejidos — Studio Tapitop

Visor interactivo en 3D (Three.js) que muestra los tejidos del estudio sobre un sofá, una butaca, una silla y un puf, con giro de 360°, vista de detalle del tejido, fondo claro/oscuro y enlace compartible.

Todo está en un único archivo: `index.html`. No necesita build ni servidor especial.

## Publicar en studiotapitop.com

1. Copia la carpeta `visualizador-3d` al hosting (por ejemplo en `/visualizador/`).
2. Enlaza desde el menú: `https://studiotapitop.com/visualizador/`.
3. Para incrustarlo dentro de una página existente:

```html
<iframe src="/visualizador/" style="width:100%;height:80vh;border:0" allowfullscreen loading="lazy"></iframe>
```

Three.js se carga desde jsDelivr y las fuentes desde Google Fonts. Si prefieres alojarlo todo en tu dominio, descarga `three@0.160.1` y cambia las dos URLs del bloque `importmap`.

## Añadir o cambiar tejidos

Edita el array `FABRICS` al principio del `<script type="module">`. Cada tejido tiene:

| Campo | Qué es |
| --- | --- |
| `id` | Identificador para el enlace compartible (`#tela=noche`). |
| `name`, `type`, `desc` | Nombre comercial, familia y descripción corta que aparecen en la ficha. |
| `width`, `comp`, `mart` | Ancho, composición y resistencia (Martindale). Los valores actuales son de ejemplo. |
| `color` | Color base en hex. Se usa para el brillo (sheen) y para las texturas procedurales. |
| `gen` | Generador procedural: `boucle`, `chenille`, `chenille2`, `velvet`, `linen`, `basket`, `stripe`. |
| `map`, `normal` | Alternativa a `gen`: rutas a fotos planas y repetibles del tejido (ver abajo). |
| `tile` | Metros reales que cubre una repetición de la textura (0.2–0.4 m es lo habitual). |
| `sheen`, `rough`, `bump` | Brillo de fibra (0–1), rugosidad (0–1) y relieve del mapa de normales. |

### Usar fotos reales de los tejidos

Las fotos de catálogo con pliegues y sombras no sirven como textura: se repetirían los pliegues por todo el mueble. Lo que funciona es un **escaneo o foto cenital, plana y con luz uniforme** de un trozo de unos 20–30 cm, recortada a un cuadrado y guardada a 2048 px en JPG. Después:

```js
{ id: 'noche', name: 'Noche', type: 'Bouclé', color: '#151210',
  map: 'texturas/noche.jpg', tile: 0.30, sheen: 0.5, rough: 0.95, ... }
```

Si además dispones de un mapa de normales (se puede generar con herramientas como Materialize o NormalMap Online), añade `normal: 'texturas/noche_n.jpg'`. Para que la repetición no se note, conviene que la imagen sea "seamless" (Photoshop → Filtro → Desplazamiento + clonar bordes, o cualquier herramienta de texturas repetibles).

El botón **Tu tela** de la app permite probar cualquier foto al momento sin tocar código: es útil para comprobar cómo queda un escaneo antes de añadirlo al catálogo.

## Botón "Solicitar muestra"

Apunta a `CONTACT_URL` (por defecto `https://studiotapitop.com/contacto`) y añade `?tela=Nombre` para que el formulario sepa qué tejido interesa.

## Enlaces compartibles

El estado va en la URL: `index.html#tela=caramelo&mueble=butaca`. Sirve para enlazar un tejido concreto desde la ficha de producto o desde un correo.
