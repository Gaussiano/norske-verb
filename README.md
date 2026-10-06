# Norske Verb 🇳🇴

Aplicación web para aprender la conjugación de los verbos noruegos (bokmål), con
explicaciones en español. Es una **página única autónoma** (`index.html`): todo
el HTML, CSS y JavaScript van embebidos en un solo archivo, sin dependencias
externas salvo las tipografías de Google Fonts. El progreso se guarda en el
navegador mediante `localStorage`.

## Características

- **297 verbos** en la base de datos, de los cuales **48** pertenecen a la lista
  estructurada del curso A1.
- Clasificación por grupos de conjugación: Grupo 1 (-et), Grupo 2 (-te),
  Grupo 3 (-de), Grupo 4 (-dde) e irregulares.
- Reconocimiento de **sinónimos**: si un verbo en español tiene varias
  traducciones noruegas, se aceptan todas como válidas. Ejemplos:
  - *trabajar* → `å jobbe` / `å arbeide`
  - *practicar* → `å øve` / `å praktisere` / `å drive med`
  - *repetir* → `å gjenta` / `å repetere`
  - *descansar* → `å hvile` / `å slappe av`

  El emparejamiento se hace por coincidencia en el campo de traducción
  española (separado por comas), de forma que añadir un sinónimo nuevo basta
  con incluir la palabra en español compartida.
- Tolerancia de entrada: acepta sustituciones ASCII (`ae`→æ, `oe`→ø, `aa`→å) y
  variantes conocidas (`ba`/`bad`, `ga`/`gav`, `sto`/`stod`, `dro`/`drog`).
- Diseño responsive de estética nórdica; funciona igual en móvil y escritorio.

## Los 4 módulos

1. **Aprendizaje progresivo** — Empieza con 3 verbos por grupo y va ampliando.
   Pide las tres formas (infinitivo, presente, pretérito) a partir del verbo en
   español, o cuatro si está activo el pretérito perfecto. Los verbos nuevos requieren 2 aciertos para dominarse; los de niveles
   anteriores, solo 1. Al dominar el nivel completo, ofrece avanzar al siguiente.
2. **Práctica aleatoria** — Todos los verbos en orden mezclado, mismo formato de
   pregunta que el módulo 1.
3. **Frases con huecos** — 73 frases de contexto (15 de ellas en pretérito
   perfecto, que solo aparecen con el toggle activo) donde hay que escribir la forma
   verbal correcta. La pista muestra solo el verbo en español; el tiempo verbal
   se revela al comprobar. Los fallos se marcan en rojo dentro de la frase.
4. **Diccionario** — Búsqueda en vivo y tres ordenaciones: por categoría,
   alfabético en noruego y alfabético en español.

## Ajustes y funciones

- **Toggle "Pedir å"** en la barra superior: si está activo, el infinitivo debe
  escribirse con `å` (p. ej. `å bo`); si no, basta la raíz (`bo`).
- **Toggle "Pretérito perfecto"** en la barra superior, apagado por
  defecto. Al activarlo:
  - Los módulos 1 y 2 piden una cuarta forma, `har` + participio (`har reist`).
    Para verbos de movimiento o cambio de estado (`komme`, `gå`, `reise`,
    `bli`…) también se acepta `er` (`er reist`, `er blitt`).
  - El módulo 3 incluye frases en perfecto (`Jeg har ___ til Norge`), donde se
    escribe solo el participio.
  - El diccionario muestra la columna *Perfektum* y los cuadros de corrección
    incluyen la forma `perf.`.

  Con el toggle apagado, el perfecto no se pregunta ni se muestra en ningún sitio.
- **Reportar error**: tras comprobar o revelar, un botón abre un cuadro con el
  contexto del fallo, que se puede copiar al portapapeles o descargar como `.txt`.
- **Enter** para avanzar a la siguiente pregunta una vez comprobada la actual.
- **Reiniciar progreso** desde el pie de página.

## Desbloqueo de niveles (módulo 1)

Se puede saltar a un nivel concreto mediante contraseña (desbloquea también todos
los inferiores). Botón "🔓 Desbloquear nivel" en el banner del módulo 1.

| Nivel | Contraseña      |
|-------|-----------------|
| 2     | `nordlys`       |
| 3     | `fjordvei`      |
| 4     | `soloppgang`    |
| 5     | `bjorkeskog`    |
| 6     | `vintergata`    |
| 7     | `isbjorn`       |
| 8     | `midnattssol`   |
| 9     | `grantreet`     |
| 10    | `trollstigen`   |

## Uso local

Al ser un único archivo estático, basta con abrir `index.html` en cualquier
navegador. No requiere servidor ni instalación.

## Despliegue

### Netlify (con redespliegue automático)
1. *Add new site → Import from Git* y conecta este repositorio.
2. No hace falta build command; el directorio de publicación es la raíz.
3. Cada `git push` a `main` redesplegará la web automáticamente.

Alternativa rápida sin Git: arrastra la carpeta con `index.html` a la zona de
*drag and drop* de Netlify.

### GitHub Pages
*Settings → Pages → Source: rama `main` / carpeta raíz*. La web quedará publicada
en `https://TU_USUARIO.github.io/norske-verb/`.

## Estructura técnica

- **Estado y persistencia**: objeto `state` en memoria; se serializa a
  `localStorage` (clave `norske_verb_progress_v1`) el nivel, las estadísticas, el
  nivel desbloqueado, los verbos dominados y los ajustes de `å` y del pretérito
  perfecto.
- **Base de datos de verbos**: array `VERBS`, cada entrada con `inf`, `pres`,
  `pret`, `perf`, `group`, `es` (español) y, opcionalmente, `noter`/`noterOrder`
  (pertenencia y orden en la lista del curso A1) y `note` (excepciones).
- **Frases**: array de objetos con `no` (frase noruega con `___`), `es`
  (traducción) y `blanks` (verbo y tiempo de cada hueco: `pres`, `pret`, `perf`
  o `inf`).

## Versionado

La versión se muestra en el pie de `index.html` y debe coincidir con la
entrada más reciente del historial de abajo. Sigue **SemVer** (`MAJOR.MINOR.PATCH`):

| Dígito | Cuándo se bumpea |
|--------|------------------|
| **PATCH** (`v1.1.→1`) | Cada petición de cambio: ajustes de UI, conjugaciones, traducciones, sinónimos puntuales, documentación. Es el caso por defecto. |
| **MINOR** (`v1.→2.0`) | Cambios grandes: nuevo módulo, lote amplio de verbos o frases, refactor visible. Se acuerda con el usuario antes de subirlo. |
| **MAJOR** (`→2.0.0`) | Cambio incompatible: estructura de `localStorage` o de la base de verbos que rompa el progreso guardado (requiere migración). |

## Historial de versiones

- **v2.0.0** — Pretérito perfecto (*perfektum*, `har` + participio) como
  nuevo tiempo opcional, controlado por un toggle en la barra superior
  (apagado por defecto). Cuarto campo en los módulos 1 y 2, 15 frases nuevas
  en el módulo 3 y columna *Perfektum* en el diccionario, todo condicionado al
  toggle. La clave de `localStorage` no cambia, así que el progreso guardado se
  conserva. Major bump a petición del usuario por el alcance del cambio.

- **v1.1.1** — Sección de sinónimos en el README ampliada con ejemplos
  concretos; añadido este control de versiones y changelog.
- **v1.1.0** — Nuevos verbos noruegos sinónimos: `å praktisere` (grupo 2) y
  `å drive med` (irregular) para *practicar*; ampliada la traducción de
  `å slappe av` para incluir *descansar* y enlazarlo así con `å hvile`.
- **v1.0.0** — Primera versión publicada: 295 verbos, 4 módulos
  (aprendizaje progresivo, práctica aleatoria, frases con huecos,
  diccionario), 58 frases, sistema de niveles con contraseñas y
  persistencia en `localStorage`.

## Licencia

Uso personal y educativo.
