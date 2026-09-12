# VoxTi Labs — Sistema de diseño Pulso

Dirección definitiva de VoxTi Labs. Moderna e informal, con modo día y modo noche.
Versión 1.1 · septiembre 2026 · incorpora el isotipo y los archivos de marca

---

## Índice

1. [Principio](#1-principio)
2. [Tokens](#2-tokens)
3. [Tipografía](#3-tipografía)
4. [Espaciado, radios y layout](#4-espaciado-radios-y-layout)
5. [Componentes](#5-componentes)
6. [El dispositivo de marca](#6-el-dispositivo-de-marca)
7. [Marca: isotipo, wordmark y lockup](#7-marca-isotipo-wordmark-y-lockup)
8. [Voz](#8-voz)
9. [Accesibilidad](#9-accesibilidad)
10. [Antipatrones](#10-antipatrones)
11. [Instalación](#11-instalación)

---

## 1. Principio

**Ningún componente sabe en qué modo está.** Todos piden `--text`, `--bg-raised`, `--action`. El modo decide qué valor entregar. Por eso el mismo HTML funciona en día y en noche sin una sola clase condicional.

Esto obliga a una disciplina: **nunca escribas un hex suelto en un componente.** Si un color no existe como token, o falta el token o el componente está mal pensado.

Dos reglas que hacen que el modo noche no sea una inversión ingenua:

- **La tarjeta siempre se separa del fondo.** En día baja un paso (`#FFFFFF` → `#F7F8FD`), en noche sube uno (`#0B0D14` → `#141826`). Misma intención, resuelta al revés.
- **Los colores saturados se recalibran, los rellenos no.** El azul de botón `#3D5AFE` es idéntico en los dos modos. El azul de *texto* cambia de `#2739D6` a `#93A2FF`, porque un azul oscuro sobre fondo negro no se lee y un azul saturado como texto vibra.

---

## 2. Tokens

### Modo día

```css
:root, [data-mode="dia"] {
  --bg:#FFFFFF;            /* fondo de página */
  --bg-raised:#F7F8FD;     /* tarjetas, tablas, superficies elevadas */
  --bg-rest:#E6E9F2;       /* zona de descanso, cifras, estado "antes" */
  --border:#E6E9F2;        /* filete estándar */
  --border-strong:#CBD0DE; /* borde de campo y de botón secundario */

  --text:#12141C;          /* titulares y datos */
  --text-body:#414759;     /* texto corrido */
  --text-muted:#6E7488;    /* secundario, metadatos, rótulos */
  --text-faint:#A3A9BC;    /* deshabilitado, placeholder */

  --action:#3D5AFE;        /* relleno de botón primario */
  --action-hover:#2739D6;
  --action-text:#2739D6;   /* enlaces y texto azul */
  --action-soft:#EEF1FF;   /* fondo de aviso y etiqueta */
  --action-soft-br:#DDE2FF;

  --warn:#FFA51F;          /* ámbar de energía y atención */
  --warn-text:#96560A;
  --warn-soft:#FFF6E6;
  --warn-soft-br:#FFDFA8;

  --good-text:#0F7A52;
  --good-soft:#E7FAF3;
  --good-soft-br:#C6F0DF;

  --bad:#E23D5C;           /* relleno de botón destructivo */
  --bad-text:#A6203C;
  --bad-soft:#FFEEF1;
  --bad-soft-br:#FCD6DE;

  --field-bg:#FFFFFF;
}
```

### Modo noche

```css
[data-mode="noche"] {
  --bg:#0B0D14;
  --bg-raised:#141826;
  --bg-rest:#1D2233;
  --border:#262C40;
  --border-strong:#333A52;

  --text:#EDEFF7;          /* nunca blanco puro: produce halo */
  --text-body:#B9C0D4;
  --text-muted:#8189A0;
  --text-faint:#5A6178;

  --action:#3D5AFE;        /* idéntico al día */
  --action-hover:#5570FF;
  --action-text:#93A2FF;   /* recalibrado para leerse sobre oscuro */
  --action-soft:#171E3D;
  --action-soft-br:#2A3566;

  --warn:#FFB84D;          /* subido para contraste */
  --warn-text:#FFB84D;
  --warn-soft:#2A2012;
  --warn-soft-br:#4A3A1C;

  --good-text:#4FD9A0;
  --good-soft:#10281F;
  --good-soft-br:#1E4536;

  --bad:#D9304F;           /* bajado para que el blanco encima pase AA */
  --bad-text:#FF8098;
  --bad-soft:#2A141B;
  --bad-soft-br:#4D2130;

  --field-bg:#0F1220;      /* más oscuro que la tarjeta, no igual */
}
```

### Qué significa cada color

| Token | Significado | Nunca |
|---|---|---|
| `--action` | Acción principal, enlaces, estado activo | Decoración |
| `--warn` | Requiere atención pronto, vencimientos | Destacar algo lindo |
| `--good-text` | Confirmación, estado saludable | Botones |
| `--bad` | Error y acción destructiva | Énfasis |

Si un color aparece porque "queda bien", está mal usado. En cuanto el ámbar se usa para decorar, deja de servir para avisar.

---

## 3. Tipografía

| Rol | Familia | Peso | Tamaño | Interletra |
|---|---|---|---|---|
| Display | Outfit | 800 | 46–68 px | −4,5% |
| H2 | Outfit | 700 | 34 px | −3,5% |
| H3 | Outfit | 700 | 17 px | −2% |
| Cuerpo | Inter | 400 | 16 px / línea 1.65 | 0 |
| Menor | Inter | 400 | 14 px | 0 |
| Dato | JetBrains Mono | 400 | 13 px | 0 |
| Rótulo | JetBrains Mono | 400 | 11 px mayúsculas | +8% |

Las tres son de Google Fonts, licencia OFL, uso comercial libre, embebibles en PDF.

**Outfit nunca en párrafos** — cansa a partir de tres líneas. **Mono nunca en prosa.**

**El wordmark no se escribe en Outfit.** Es un archivo en curvas con letras propias, una geométrica de terminaciones redondeadas que no es ninguna de las tres familias de arriba. Outfit es la tipografía de la interfaz; el nombre de la marca es un asset. Ver sección 7.

La mono es obligatoria en: montos, UF, RUT, fechas, plazos, códigos de propuesta, identificadores. Alinea en columna y evita confundir el 0 con la O, que en un documento comercial no es detalle menor.

En cifras destacadas, el número va en Outfit 700 y la unidad en Inter 400 con `--text-muted`, para que domine el número.

---

## 4. Espaciado, radios y layout

```
Escala de espaciado   8 · 16 · 24 · 32 · 48 · 64 · 96      (nada intermedio)
Radios                botón 999px · campo 14 · tarjeta 22 · bloque grande 28
Alto de control       46 px mínimo · área táctil 44 px mínimo
Ancho de contenido    1000 px máximo
Medida de texto       66 caracteres máximo
Separación secciones  96 px escritorio · 56 px celular
Grilla                12 columnas, canaleta 24
```

Solo hay **tres niveles de superficie**: `--bg`, `--bg-raised`, `--bg-rest`. Si necesitas un cuarto, estás anidando de más.

Alineación a la izquierda por defecto. Centrado solo en hero y estados vacíos.

---

## 5. Componentes

### Botón

```
padding 15px 30px · radio 999px · Outfit 700 · 16px
primario     fondo --action, texto blanco, hover --action-hover
secundario   fondo transparente, borde 1px --border-strong, texto --text
fantasma     sin fondo, texto --text-muted
destructivo  fondo --bad, texto blanco
deshabilitado fondo --bg-rest, texto --text-faint, cursor not-allowed
pequeño      14px, padding 10px 20px
foco         outline 2px --action, offset 2px
```

**Un solo botón primario por vista.** Si hay dos, el usuario no sabe cuál es la acción principal.

### Campo

```
alto 46px · padding 0 16px · radio 14px
fondo --field-bg · borde 1px --border-strong
foco  borde --action + box-shadow 0 0 0 3px --action-soft
error borde --bad, mensaje en --bad-text
label arriba, Inter 500 14px, color --text
ayuda debajo, 13px, color --text-muted
```

En modo noche el campo usa `--field-bg`, más oscuro que la tarjeta. Un input del mismo color que su contenedor desaparece.

### Etiqueta

```
padding 6px 14px · radio 999px · 12.5px peso 500
fondo --{rol}-soft · texto --{rol}-text
```

Nunca texto negro sobre color, nunca color sobre color saturado.

### Tarjeta

```
fondo --bg-raised · borde 1px --border · radio 22px · padding 22px
sin sombra
```

### Tabla

```
encabezado  mono 11px mayúsculas, color --text-muted, fondo --bg-rest
celdas      padding 15px 18px, borde inferior 1px --border
cifras      mono 13.5px, color --text, alineadas a la derecha
```

### Aviso

```
radio 14px · padding 15px 20px
fondo --{rol}-soft · borde 1px --{rol}-soft-br · texto --{rol}-text
primera frase en peso 500
```

Los errores dicen qué pasó y qué hacer, en una frase. Sin "Error:", sin disculpas, sin el mensaje crudo del sistema.

### Estado vacío

Dice qué va a aparecer ahí y ofrece la acción que lo provoca. Nunca "no hay datos".

---

## 6. El dispositivo de marca

Dos bloques lado a lado. Es el único elemento visual propio *del contenido* —el isotipo de la sección 7 pertenece a la marca, no a la página— y no es decorativo: dice literalmente lo que VoxTi vende. Reemplaza al hero genérico y a las grillas de features con íconos.

El isotipo es la versión comprimida de esta misma idea: dos bloques, un paso entre ellos. Por eso no se usan los dos como protagonistas en la misma pantalla; el isotipo vive en el encabezado y el dispositivo en el cuerpo.

```
Antes    fondo --bg-rest · borde 1px --border-strong · texto --text-body
Después  fondo --bg-raised · borde 1px --action-soft-br · texto --text
Rótulo   mono 11px mayúsculas · antes en --text-muted, después en --action-text
```

El "antes" describe el trabajo manual actual, concreto y con frecuencia: *"Alguien revisa el correo, copia al Excel y avisa por WhatsApp. Tres veces al día."*
El "después" describe el mismo proceso ya resuelto.

Se usa en landings, propuestas y piezas de Instagram.

---

## 7. Marca: isotipo, wordmark y lockup

Son dos piezas. El **isotipo** es la forma azul; el **wordmark** es el nombre dibujado. Juntos forman el **lockup**, que es lo que se usa casi siempre.

### Isotipo

Dos bloques de esquinas redondeadas desplazados en diagonal, unidos por un escalón. Es el dispositivo de la sección 6 dicho en una sola forma: un paso que entrega al siguiente.

```
proporción   209 × 189 (1,11:1)
color        --action, idéntico en día y en noche
altura       igual a la altura de mayúscula del wordmark
alineación   su base se apoya en la línea de base del wordmark
```

El isotipo es la única excepción a la regla de que el azul no decora. Para que no compita con la acción, aparece **una sola vez por pieza** y no se repite como ícono ni como viñeta.

### Wordmark

Se escribe **VoxTi Labs**, con V y T mayúsculas. La T interior levanta la palabra en el medio; ese quiebre se respeta, no se disimula.

```
"VoxTi"  peso alto    · color --text
"Labs"   peso liviano · color --text-muted
```

Los dos colores son tokens, así que el mismo dibujo funciona en los dos modos sin recolorearse a mano.

**El wordmark está en curvas, no es texto.** Sus letras son una geométrica redondeada, distinta de Outfit. No se reproduce escribiendo "VoxTi Labs" en Outfit junto al isotipo: no calza y se nota. Si necesitas el nombre en un lugar donde no puedes poner el archivo, escríbelo como texto normal, en la tipografía que corresponda al contexto, y no lo trates como logo.

### Lockup

```
completo   1389 × 191  (7,3:1)   isotipo + VoxTi Labs
corto       875 × 190  (4,6:1)   isotipo + VoxTi
isotipo     209 × 189            la forma sola

separación  isotipo→wordmark 68 u · "VoxTi"→"Labs" 60 u
aire libre  media altura de isotipo por lado, sin texto, sin borde, sin foto
```

Las separaciones ya vienen resueltas en el archivo. No se reajustan, no se aprieta el lockup para que quepa: si no cabe, se baja de versión.

### Tamaños mínimos

```
lockup completo   190 px de ancho · 45 mm impreso
lockup corto      120 px de ancho
isotipo solo       20 px · 16 px únicamente como favicon
```

Bajo 190 px usa el corto. Bajo 120 px usa el isotipo solo. Nunca encojas el completo más allá del mínimo: "Labs" se convierte en una mancha gris antes que en una palabra.

### Archivos

| Archivo | Cuándo |
|---|---|
| `voxti-tokens.svg` | **Por defecto en la web.** Pégalo inline en el HTML: usa `var(--text)`, `var(--text-muted)` y `var(--action)`, y cambia solo con el modo |
| `voxti-tinta.svg` | Fondo claro, cuando no puedes pegar inline (correo, PDF, terceros) |
| `voxti-claro.svg` | Fondo oscuro, mismo caso |
| `voxti-mono.svg` | Un solo color, hereda `currentColor`. Para grabados, sellos, fondos de color |
| `voxti-corto-tinta.svg` · `voxti-corto-claro.svg` | Espacios reducidos, barras compactas, celular |
| `voxti-isotipo.svg` | Avatar, marca de agua, viñeta de cierre de documento |
| `voxti-isotipo-cuadrado.svg` | Favicon y fotos de perfil, con el aire ya incluido |

Todos con fondo transparente y contrapunzones huecos —los de la o, la a y la b son agujeros reales, no parches blancos— así que el logo se puede poner sobre `--bg-raised`, sobre color o sobre una foto sin que aparezca un rectángulo detrás.

### Nunca

- Aplicar `--action` al wordmark. El azul es del isotipo.
- Recolorear el isotipo: ni ámbar, ni verde, ni degradado, ni blanco calado sobre un cuadrado azul.
- Separar el isotipo del wordmark y mandarlos a esquinas distintas de la pieza.
- Rearmar el lockup escribiendo el nombre en Outfit al lado del isotipo.
- Estirarlo, inclinarlo, rotarlo, ponerle sombra, contorno o brillo.
- Usar el isotipo como bullet, ícono de lista, patrón de fondo o adorno de sección.
- Encerrarlo en un círculo, un cuadrado con borde o una pastilla.
- Abreviar a "VTL" o a cualquier sigla.
- Ponerlo sobre un recuadro blanco dentro de una pieza oscura. Para eso está `voxti-claro.svg`.

---

## 8. Voz

- Titulares de **menos de ocho palabras**. Si no cabe, no era el titular.
- **Tuteo directo.** "Tu equipo", no "las organizaciones".
- El problema del cliente antes que la tecnología. "Deja de copiar datos a mano", no "sistema RAG con base vectorial".
- El botón dice qué pasa: "Enviar propuesta", no "Aceptar". Y mantiene su nombre en todo el flujo: si dice "Enviar", el aviso dice "Propuesta enviada".
- Una cifra concreta y verificable por sección. **Si no tienes el dato real, escribe el rótulo sin número.**

---

## 9. Accesibilidad

Contrastes verificados en los dos modos:

| Combinación | Día | Noche |
|---|---|---|
| Texto titular sobre fondo | 17,4:1 | 16,9:1 |
| Texto cuerpo sobre fondo | 9,7:1 | 10,7:1 |
| Texto secundario sobre fondo | 5,3:1 | 5,6:1 |
| Enlace azul sobre fondo | 7,1:1 | 8,2:1 |
| Blanco sobre botón primario | 5,1:1 | 5,1:1 |
| Blanco sobre botón destructivo | 4,6:1 | 4,7:1 |
| Texto de aviso sobre su fondo | 4,9–7,1:1 | 6,9–9,3:1 |

Todo pasa AA, casi todo AAA.

Requisitos no negociables:

- Foco visible en todo control: `outline: 2px solid var(--action); outline-offset: 2px`.
- Todo operable con teclado, en orden lógico.
- `--text-faint` solo para deshabilitado y placeholder, nunca para contenido.
- Respetar `prefers-reduced-motion`.
- El modo inicial sigue a `prefers-color-scheme`, y el usuario puede cambiarlo.
- El color nunca es el único portador de significado: la etiqueta lleva texto además del color.

---

## 10. Antipatrones

Prohibido, sin excepciones:

- **Gradientes.** Ninguno, en ningún elemento.
- **Sombras.** Salvo el anillo de foco.
- **Morado y violeta.**
- **Verde como color principal** — está tomado por GoDevs y por Flow.
- **Blanco puro como color de texto en modo noche.**
- **Negro puro** como fondo.
- Glassmorphism, blur de fondo, bordes con brillo, neón.
- **Emoji** en titulares, botones, encabezados o etiquetas. Informal no es desordenado; Flow tampoco los usa, y el emoji es justo lo que delata un sitio generado.
- Iconos de cohete, rayo, cerebro o bombilla.
- **El isotipo repetido** como viñeta, ícono de feature, patrón de fondo o marca de agua gigante detrás del texto.
- El logo con un recuadro blanco debajo en una pieza oscura.
- Fotos de stock de gente dándose la mano en oficinas.
- La grilla de tres columnas con ícono, título y dos líneas.
- Hero con número gigante y acento en gradiente.
- Contadores animados, partículas, parallax.
- Jerga en material comercial: RAG, embeddings, orquestación.
- Copy de relleno: "eleva tu negocio", "impulsa", "potencia", "de vanguardia", "solución integral", "100% garantizado".
- **Métricas inventadas.**

---

## 11. Instalación

### CSS

Copia los dos bloques de la sección 2 a `pulso-tokens.css` e impórtalo una vez en la raíz del proyecto. El modo se controla con un atributo en `<html>`:

```html
<html data-mode="dia">   <!-- o data-mode="noche" -->
```

Detección y cambio:

```js
if (window.matchMedia('(prefers-color-scheme: dark)').matches) {
  document.documentElement.dataset.mode = 'noche';
}
// para cambiarlo:
document.documentElement.dataset.mode = 'noche';
```

### Tailwind

En `tailwind.config.js`, apunta los colores a las variables y activa el modo por atributo:

```js
module.exports = {
  darkMode: ['selector', '[data-mode="noche"]'],
  theme: { extend: { colors: {
    bg:      'var(--bg)',
    raised:  'var(--bg-raised)',
    rest:    'var(--bg-rest)',
    line:    'var(--border)',
    ink:     'var(--text)',
    body:    'var(--text-body)',
    muted:   'var(--text-muted)',
    action:  'var(--action)',
  }}}
}
```

Así las clases (`bg-raised`, `text-muted`) funcionan igual en los dos modos sin variantes `dark:`.

### Fuentes

```html
<link href="https://fonts.googleapis.com/css2?family=Outfit:wght@400;700;800&family=Inter:wght@400;500&family=JetBrains+Mono:wght@400;500&display=swap" rel="stylesheet">
```

### Marca

Los archivos viven en `/marca/`. En la web, pega el contenido de `voxti-tokens.svg` directamente en el HTML: al estar inline, el SVG lee las variables de la página y cambia de modo solo, sin duplicar archivos ni escuchar el cambio por JS.

```html
<a href="/" class="marca" aria-label="VoxTi Labs, inicio">
  <!-- contenido de /marca/voxti-tokens.svg -->
  <svg viewBox="0 0 1389 191" fill="none" role="img" aria-label="VoxTi Labs">…</svg>
</a>
```

```css
.marca { display:inline-block; line-height:0; }
.marca svg { height:30px; width:auto; display:block; }   /* 218 px de ancho */
.marca:focus-visible { outline:2px solid var(--action); outline-offset:2px; border-radius:4px; }
```

A 30 px de alto el lockup mide 218 px de ancho, sobre el mínimo. Si un contexto no da esos 190 px, pega los dos SVG —completo y corto— y esconde uno con la media query. No achiques el completo.

Un `<img src="voxti-tokens.svg">` **no** funciona: el SVG cargado como imagen está aislado y no ve las variables. Ahí usa `voxti-tinta.svg` o `voxti-claro.svg` según el fondo.

Favicon:

```html
<link rel="icon" href="/marca/voxti-isotipo-cuadrado.svg" type="image/svg+xml">
```

### Antes de entregar cualquier pantalla

1. ¿Hay exactamente un botón primario?
2. ¿Algún hex suelto en el código? Búscalo.
3. ¿Se ve bien en los dos modos? Cambia y revisa.
4. ¿Se recorre con el tabulador y se ve el foco?
5. ¿Los montos, RUT y fechas están en mono?
6. ¿Funciona a 360 px sin scroll horizontal?
7. ¿El logo es el archivo correcto —inline con tokens, o la variante del fondo— y por sobre su tamaño mínimo?
8. ¿Se coló algún antipatrón de la sección 10?
