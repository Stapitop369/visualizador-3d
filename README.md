# Visualizador de tejidos — Studio Tapitop

Visor interactivo que muestra los tejidos del estudio sobre un sofá, una butaca, una silla y un puf en 3D con giro de 360°, y que además aplica cualquier tejido de la colección sobre una **foto del mueble del cliente**.

Sigue el Brand Book (edición 1.0): paleta negro / taupe / gris / crema, Arual para titulares display, Generica para todo lo demás, lockup horizontal en la cabecera, sin esquinas redondeadas, y el kit de la puntada (hilván, etiqueta cosida y cinta métrica como indicador de carga).

Todo está en un único archivo: `index.html`. Las fuentes van incrustadas, así que no hace falta instalar nada.

## Publicar en studiotapitop.com

1. Copia la carpeta al hosting (por ejemplo en `/visualizador/`).
2. Enlaza desde el menú: `https://studiotapitop.com/visualizador/`.
3. Para incrustarlo dentro de una página existente:

```html
<iframe src="/visualizador/" style="width:100%;height:80vh;border:0" allowfullscreen loading="lazy"></iframe>
```

Three.js se carga desde jsDelivr. Si prefieres alojarlo todo en tu dominio, descarga `three@0.160.1` y cambia las dos URLs del bloque `importmap`.

## Modo "Tu foto"

El cliente sube una foto de su sofá o silla y el tejido elegido se aplica sobre la tapicería conservando la luz y las sombras de la foto.

1. **Varita**: al tocar la tela del mueble se selecciona la zona del mismo color. Se puede tocar varias veces; la tolerancia regula cuánto se extiende.
2. **Pincel / Borrar**: para completar o limpiar la selección a mano.
3. **Escala tejido**: ajusta el tamaño del dibujo del tejido respecto al mueble (la app supone que la foto abarca unos 3 m de ancho).
4. **Guardar** descarga la imagen resultante en JPG.

Consejos para una buena foto: luz uniforme, mueble de un solo color, fondo que contraste con la tela.

## Añadir o cambiar tejidos

Edita el array `FABRICS` al principio del `<script type="module">`. Cada tejido tiene:

| Campo | Qué es |
| --- | --- |
| `id` | Identificador para el enlace compartible (`#tela=noche`). |
| `name`, `type`, `desc` | Nombre comercial, familia y descripción corta que aparecen en la etiqueta. Si el nombre lleva tilde se muestra en Generica (Arual no tiene acentos). |
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

Si además dispones de un mapa de normales (Materialize, NormalMap Online), añade `normal: 'texturas/noche_n.jpg'`. Conviene que la imagen sea repetible sin costuras (Photoshop → Filtro → Desplazamiento + clonar bordes, o cualquier herramienta de texturas "seamless").

## Botón "Solicitar muestra"

Apunta a `CONTACT_URL` (por defecto `https://studiotapitop.com/contacto`) y añade `?tela=Nombre` para que el formulario sepa qué tejido interesa.

## Enlaces compartibles

El estado 3D va en la URL: `index.html#tela=caramelo&mueble=butaca`. Las fotos del cliente no se suben a ningún servidor: todo se procesa en su navegador.
