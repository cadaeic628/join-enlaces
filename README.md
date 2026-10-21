# Página de enlaces de JOIN

La página que abre el link de la bio de [@proyectojoin.dcc](https://instagram.com/proyectojoin.dcc).
Un solo archivo, `index.html`, sin dependencias ni build: se abre con doble clic y funciona igual.

Sigue la estructura de [links.cadcc.cl](https://links.cadcc.cl), que corre LinkStack con el tema
galaxy: avatar circular, nombre, bio, fila de redes y una pila de botones con separadores de
sección. Lo que cambia es la identidad. El fondo es el tono `tinta` de `social/join-social.typ`,
las estrellas del tema galaxy son bowties del logo derivando muy despacio, el avatar es el ícono
de Instagram en negativo, disco azul y bowtie blanco, con el aire alrededor que pide
`instrucciones-forms-y-logo.txt` para que el símbolo aguante el recorte circular, sin anillo, y toda la página va en Inconsolata, la misma monoespaciada de la
palabra JOIN en las guías. Es la única tipografía que carga: no hay serif en ninguna parte.

El tema es único a propósito: el fondo oscuro se ve igual venga el teléfono en claro o en oscuro.

Al pie van los logos del CaDCC y del DCC, enlazados a `cadcc.cl` y `dcc.uchile.cl`. Los dos están
dibujados para fondo claro —sobre la tinta el morado del CaDCC y el gris del DCC no se leen— así
que van dentro de una placa crema con las esquinas redondeadas, que respeta los colores oficiales
sin recolorear la marca de nadie. Van incrustados en el HTML como data URI a 3x, unos 20 KB entre
los dos, así que el archivo sigue siendo uno solo y no depende de ninguna carpeta de imágenes.
Si hay que reemplazarlos, el script que los preparó recorta el aire transparente, escala a 3x del
alto de pantalla y aplana contra el crema de la placa.

## Cambiar los links

Todo lo editable está en un solo bloque, al final de `index.html`, bajo el comentario
`CONFIGURACIÓN`. Son cuatro constantes y no hace falta tocar nada más.

### `BIO`

El párrafo bajo el nombre. Un string.

### `SESIONES`

La agenda de próximas sesiones, sobre los botones. Es una lista: se muestran todas, en el orden
en que estén, separadas por un filete.

```js
const SESIONES = [
  {
    codigo:    "CC3102",
    ramo:      "Teoría de la Computación",
    dia:       "7",
    mes:       "sep",
    diaSemana: "lunes",
    hora:      "16:15 a 17:45",
    // lugar:  "sala B12",
    url:       "https://docs.google.com/forms/d/e/1FAIpQL.../viewform",
  },
];
```

`dia` y `mes` arman el bloque de fecha de la izquierda. El día se rellena solo a dos dígitos, así
que da lo mismo escribir `"7"` o `"07"`; el mes va en tres letras porque la columna es angosta.
`url` es el formulario de inscripción **de esa sesión**: con él la fila se vuelve pulsable y
aparece el «Inscribirse» subrayado. Si lo omites, la fila solo informa, que sirve para anunciar una sesión
cuyo formulario todavía no está listo.

`codigo`, `diaSemana`, `hora` y `lugar` son opcionales: los tres últimos se unen con comas en la
misma línea, y si no hay ninguno esa línea no aparece.

Para sumar una sesión, copia el bloque `{ ... }`. Cuando una sesión ya pasó, borra su bloque:
desaparece de la agenda junto con su link. Sin ninguna agendada, `const SESIONES = [];` hace
desaparecer el bloque entero sin dejar hueco.

El título «Próximas sesiones» está fijo en `pintarSesiones()`, un poco más abajo en el archivo.

### `REDES`

Los íconos redondos sobre el aviso.

```js
const REDES = [
  { icono: "instagram", nombre: "Instagram", url: "https://instagram.com/proyectojoin.dcc" },
];
```

### `BOTONES`

La pila principal, en el orden en que sale.

```js
const BOTONES = [
  {
    principal: true,                          // el relleno azul; deja solo uno
    icono:  "formulario",
    titulo: "Inscríbete a la próxima sesión",
    url:    "https://forms.gle/PEGA-AQUI-INSCRIPCION",
  },

  { seccion: "Redes" },                       // dibuja un separador con ese título

  { icono: "telegram", titulo: "Canal de Telegram", url: "https://t.me/PEGA-AQUI-TELEGRAM" },
];
```

- **Agregar un botón**: copia un bloque `{ ... }` y cámbiale los campos.
- **Sacar uno**: borra su bloque.
- **Reordenar**: mueve los bloques.
- **Agrupar**: mete un `{ seccion: "Título" }` donde quieras el separador. Puedes poner varios.
- `icono` acepta `formulario`, `guia`, `chat`, `telegram`, `discord`, `instagram` y `calendario`. Si lo
  omites, el botón sale sin ícono, que también es válido.

Todos los links ya son los de verdad.

El formulario de inscripción es por sesión (el actual es «Control 1 CC3102 Teoría de la
Computación»), y por eso vive dentro de `SESIONES` y no entre los botones: cada sesión trae el
suyo, y al borrar la sesión se va también su link. El de feedback es general y sirve para todas,
así que ese sí es un botón fijo.

### Agregar un ícono nuevo

Los íconos están dibujados a mano sobre una caja de 24 × 24, en el objeto `ICONOS`, justo bajo
la configuración. Para sumar uno, agrega una línea con el contenido del SVG, sin la etiqueta
`<svg>` (el trazo, el grosor y las puntas los pone el código):

```js
const ICONOS = {
  ...
  mapa: '<path d="M9 4 3 6.5v14L9 18l6 2.5 6-2.5v-14L15 6.5 9 4z"/><path d="M9 4v14M15 6.5v14"/>',
};
```

## Publicar en GitHub Pages

Ya está publicada, servida por GitHub Pages desde este mismo repo:

**https://cadaeic628.github.io/join-enlaces/**

Esa es la URL que va en la bio de Instagram (Editar perfil → Enlaces → Agregar enlace externo).

Pages sirve el `index.html` tal cual, sin build ni workflow de Actions. La configuración es
`Settings → Pages → Deploy from a branch`, rama `main`, carpeta `/ (root)`.

## Cómo actualizarla

Editas `index.html`, commiteas y pusheas. Pages republica solo en un par de minutos.

```sh
git add index.html
git commit -m "Actualiza la agenda de sesiones"
git push
```

Para un cambio chico, como borrar una sesión que ya pasó o agregar la siguiente, se puede editar
`index.html` directamente en la web de GitHub con el lápiz y confirmar los cambios ahí mismo. No
hace falta clonar nada.

### Una advertencia sobre la cuenta

El repo está en una cuenta personal. Vale la pena moverlo a la organización del CaDCC
(`Settings → General → Transfer ownership`) para que la página siga siendo editable cuando quien
la creó ya no esté en el centro. Al transferirlo cambia la URL, así que conviene hacerlo antes de
publicar el link en Instagram, o dejar un dominio propio de por medio.

### Si quieren un dominio más corto

Con un dominio propio, por ejemplo `join.cadcc.cl`, en **Settings → Pages → Custom domain** se
pone el dominio y se agrega un `CNAME` en el DNS apuntando a `<cuenta>.github.io`. GitHub crea
el archivo `CNAME` en el repo solo.

## Revisar cambios antes de subir

```sh
xdg-open index.html
```

Vale la pena mirarla en el teléfono además del computador: casi todo el tráfico llega desde
Instagram, o sea desde un celular.
