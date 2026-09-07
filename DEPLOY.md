# DEPLOY — CAVALLARO FITNESS

## 1. Estado actual

Sitio estático sin build, sin dependencias y sin JavaScript. **Se publica en Vercel**
desde la rama `main`; el repositorio se sirve tal cual, sin paso de compilación.

Dominio propio **`marcoscavallaro.com`** (registrado el 02/09/2026 en dattatec.com /
DonWeb). El sitio vive en el ápice; `www` redirige ahí.

**El sitio es indexable.** Se abrió el 02/09/2026. `404.html` conserva su `noindex`,
como corresponde a una página de error.

Pendiente documental que no bloquea: la cesión de derechos del retrato M-02 sigue
sin resolverse (ver §9). El logotipo de Under Armour en cuadro quedó resuelto por
decisión del responsable del proyecto y no se modifica.

## 2. Dominio propio, hosting y apertura a indexación

**Hosting: Vercel.** El proyecto no tiene framework ni build: Vercel sirve los archivos
del repositorio. Toda la configuración vive en **`vercel.json`**, y son tres cosas:

- `"trailingSlash": true` — **no es cosmético.** El canonical de la tarjeta es
  `https://marcoscavallaro.com/tarjeta/` con barra final. Sin esta línea Vercel
  redirige a `/tarjeta` sin barra y la URL real deja de coincidir con el canonical.
- Cabeceras del `.vcf`: `text/vcard` y `Content-Disposition: attachment`. Es lo que
  hace que iOS ofrezca guardar el contacto en vez de mostrar el texto (ver §3).
- Cache larga e inmutable para `/assets/*`.

El dominio se conecta **desde el panel de Vercel** (Project → Settings → Domains). No
hay archivo `CNAME` ni `.nojekyll`: eran de GitHub Pages y se eliminaron.

**Los seis lugares donde vive la URL absoluta**, por si vuelve a mudarse:

1. `<link rel="canonical">` de `index.html` y de `tarjeta/index.html`
2. `og:url`, `og:image` y `twitter:image` de las dos páginas
3. `@id`, `url` y `contentUrl` del JSON-LD de las dos páginas
4. `sitemap.xml` (2 URLs + `lastmod`) y `robots.txt` (`Allow` + Sitemap)
5. `llms.txt` (URLs + sección Estado) y el campo `URL` de `marcos-cavallaro.vcf`
6. `404.html`: usa rutas absolutas de raíz (`/assets/...`). Si el sitio volviera a vivir
   en un subdirectorio, **el 404 se rompe en silencio** — sin hoja de estilo y sin
   enlaces.

Las dos páginas públicas no llevan `<meta name="robots">`: son indexables. El 404 sí
lo lleva.

Falta, y lo hace Hernán a mano: dar de alta el sitio en Google Search Console y enviar
el `sitemap.xml`.

## 3. Verificaciones obligatorias antes de publicar

| # | Verificar | Riesgo si falla |
|---|---|---|
| 1 | El link `wa.me/5493413357925` **en un teléfono real** | Se pierden todas las consultas sin que nadie se entere |
| 2 | `curl -I <url>/marcos-cavallaro.vcf` → qué `Content-Type` devuelve | Si vuelve `text/plain`, iOS muestra el texto en vez de ofrecer guardar el contacto |
| 3 | La tarjeta abierta **desde adentro de WhatsApp**, en iPhone y en Android | Es el navegador por el que va a entrar casi todo el mundo, y el más restrictivo |
| 4 | Cómo se ve la tarjeta OG al pegar el link en un chat | Es lo primero que ve el que recibe el link |

Si el `.vcf` falla en iOS: el respaldo es el atributo `download` (ya está puesto) y,
si tampoco, generarlo con JavaScript. Último recurso: los datos en texto seleccionable.

## 4. Migración a dominio propio — hecha

Ver §2. El sitio ya vive en `marcoscavallaro.com`. Queda como recomendación abierta
registrar `cavallarofitness.com` y `cavallarofitness.com.ar` de forma defensiva: el
primero coincide con la denominación registrada en el INPI y con el handle de
Instagram, y dejarlos libres es una exposición innecesaria (`02-ARQUITECTURA/G.10`).

## 5. Repositorio

El repositorio es `hernancapucci/cavallaro-fitness`. Como el hosting es Vercel y el
dominio es propio, el nombre del repositorio ya no aparece en ninguna URL pública y
no hay nada que reorganizar.

## 6. Dónde vive el número de teléfono

Si Marcos cambia de número, hay que tocarlo en **todos** estos lugares:

- `index.html`: 5 enlaces `wa.me` (hero, presencial, online, marcos, contacto, barra fija, pie)
- `tarjeta/index.html`: enlace `wa.me` y enlace `tel:`
- `marcos-cavallaro.vcf`: campo `TEL`
- `index.html`: campo `telephone` del JSON-LD (dos páginas)
- `llms.txt`

## 7. Medición

No hay analytics ni cookies, y es a propósito. Pero ya se puede medir:

- **Cada enlace de acción tiene un atributo `data-cta` estable** (`hero`, `presencial`,
  `online`, `marcos`, `contacto`, `barra-fija`, `tarjeta`, `vcard`, `instagram`...).
  Cuando se agregue medición, se enganchan eventos sin tocar la estructura.
- **El mensaje prearmado de WhatsApp cambia según el bloque del que salió el clic.**
  Marcos ve en el propio mensaje si la consulta vino de presencial, de online o de la
  tarjeta. Eso ya es medición, hoy, sin instalar nada.
- El link de la biografía de Instagram debe llevar UTM desde el día uno.

---

## 8. V2 · Construcción sobre el sistema visual aprobado (2026-08-31)

Reconstrucción completa sobre la **dirección D** aprobada, el **territorio ámbar** y la
**dosis cromática** cerrada. Reemplaza la V1 (Barlow Condensed + rojo), que era anterior
a la fase de identidad y de sistema visual.

**Reglas del sistema que están codificadas en el HTML/CSS y que no hay que romper:**

| Regla | Dónde vive |
|---|---|
| La gramática **fotografía + recorte + nota** aparece **dos veces** en la home: hero y resultados | `index.html`, clases `.gram` / `.recorte` / `.nota` |
| Numeración **por sección**: cada bloque vuelve a `01` | los dos `.nota` dicen `01` |
| El **ámbar aparece dos veces**: CTA del hero y CTA de contacto (más el número/filete y el borde del recorte del hero) | `--ambar` en `site.css` |
| El **ámbar no entra en territorio de papel** (2,81:1) | `.papel .nota .n` y `.papel .recorte .n` pasan a negro |
| El **papel queda reservado a la evidencia**: el único bloque claro es Resultados | `.sec.papel` sólo en `#resultados` |
| **Nada se dibuja encima de la fotografía**: el recorte es un archivo aparte | `assets/img/recorte-*.webp` |
| El tratamiento fotográfico está **horneado en el archivo** (grayscale · contraste 1,30 · brillo 0,90), no aplicado por CSS | `06-IMPLEMENTACION/assets.py` en el workspace |
| **Archivo, familia única**, variable, 34 KB | `assets/fonts/archivo-var.woff2` |

**Si desaparece el bloque de Resultados, la página queda entera en territorio oscuro y se
pierde la alternancia.** La regla cromática aprobada exige que exista al menos un bloque
de evidencia. No borrar ese bloque sin decidir qué pasa con el color.

**Los assets se regeneran** con `~/CAVALLARO FITNESS/06-IMPLEMENTACION/assets.py`, que los
emite desde la geometría maestra (`04-IDENTIDAD/maestra`) y desde los derivados
fotográficos (`05-SISTEMA-VISUAL/inventario/derivados`). No editar los SVG a mano.

---

## 9. V3.2 · Un único campo negro (2026-09-01)

Corrección del lenguaje visual después de comparar V1, V2 y las pruebas V3 / V3.1 / V3.2.
La portada de V2 competía consigo misma: dos ejes de lectura, la gramática recorte/nota
peleando con el mensaje, el CTA fuera del recorrido y una fotografía de 720 px estirada a
pantalla completa. V3.2 quedó aprobada como dirección y este commit la extiende al sitio.

**Reglas que están codificadas y que no hay que romper:**

| Regla | Dónde vive |
|---|---|
| El sitio es **un único campo negro**. La única excepción es el territorio de papel, reservado a evidencia | `.sec.papel` sólo en `#resultados` |
| **Ninguna fotografía se muestra por encima de su resolución nativa** | ver la tabla de escalas más abajo |
| La fotografía que **pertenece al espacio** se integra con `.integra`: punto de negro igualado (`contrast(1.104)`) y cantos disueltos por máscara | `site.css`, clase `.integra` |
| La fotografía con **función documental conserva su límite** | `.papel .recorte` mantiene su filete |
| **Ningún fondo detrás de una foto más oscuro que la página** | `.foto` y `.recorte` usan `var(--fondo)`, ya no `#000` |
| La gramática **plano + recorte + nota** aparece **una sola vez**, en Resultados, donde explica algo | `index.html`, `.evid-gram` |
| El **ámbar sigue apareciendo dos veces**: CTA del hero y CTA de contacto | `--ambar` en `site.css` |
| El hero tiene **un solo eje**: texto y botones arrancan en el mismo x que el H1 | `.hero .w` |
| **Nada escrito encima de una fotografía**, en ninguna sección | — |

**Escalas de las fotografías (mostrada ÷ nativa; nunca > 1):**

| Archivo | Nativa | Home 1440 | Home 390 | Tarjeta 390 |
|---|---|---|---|---|
| `retrato-m02.webp` | 900 × 900 | 0,49 | 0,39 | — |
| `marcos-retrato.webp` | 600 × 800 | — | — | 0,50 |
| `recorte-disco.webp` | 180 × 180 | 0,92 | 0,64 | — |

### M-02 · estado de los dos puntos abiertos (actualizado 2026-09-02)

El sitio usa **M-02** (retrato de estudio) en el hero, en la tarjeta y en la imagen de
compartir. Los dos puntos que estaban registrados como impedimento quedaron así:

1. **Logotipo de Under Armour visible en cuadro.** *Resuelto por decisión.* El responsable
   del proyecto resolvió el 2026-09-02 que la marca ajena en cuadro **no constituye un
   bloqueo de publicación**. El logotipo **no se modifica, no se oculta y no se retoca**.
   La observación de `04-IDENTIDAD/01-CIERRE-IDENTIDAD-BASE.md` §7 queda como criterio de
   preferencia para la futura sesión propia, no como condición de publicación.
2. **Origen y cesión de derechos de la fotografía.** *Pendiente, y sigue pendiente.*
   Retrato de estudio, fotógrafo desconocido. Ítem 44 de `01-ADENDA-MATERIAL-ORIGINAL.md`.
   Por decisión del responsable del proyecto **no detiene la implementación ni el
   despliegue**, pero **no se da por saldado**: conviene cerrarlo antes de que el sitio
   tenga difusión sostenida, y se cierra definitivamente con la sesión de fotos propia
   que pide `05-SISTEMA-VISUAL/01-BRIEF-FOTOGRAFICO.md`.

Ninguno de los dos se disimuló en el sitio. **No hay bloqueo técnico para desplegar.**

### Qué se sacó, y qué se pierde con eso

Salieron del sitio los derivados de M-03 (`escena`, `marcos`, `recorte-barra`,
`recorte-mirada`): eran 720p tratados en blanco y negro y se mostraban ampliados. Con eso
**el sitio ya no muestra a Marcos trabajando**, que era el mejor argumento visual del lote.
Eso no se recupera con CSS: se recupera con la sesión de fotos del brief.

---

## 10. La estrategia nutricional entra al acompañamiento (2026-09-07)

Decisión del **titular del servicio**, tomada con conocimiento de las observaciones que constan
en `~/CAVALLARO FITNESS/00-EXPEDIENTE-INVESTIGACION.md` §24, que le fueron comunicadas antes.
El sitio deja de comunicar que la nutrición está excluida y pasa a comunicarla como parte del
acompañamiento.

### Qué se aprobó

La estrategia nutricional es **uno de los cinco componentes del acompañamiento**, en las dos
modalidades y sin costo aparte:

> evaluación inicial · plan mensual con progresión · corrección de la ejecución ·
> medición y ajuste mensual · **estrategia nutricional aplicada al objetivo**

Esa enumeración es **la descripción canónica del servicio** y aparece idéntica en las cinco
superficies: contenido visible, `meta description`, Open Graph/Twitter, JSON-LD y `llms.txt`.
Si cambia, cambia en las cinco.

### La fuente del contenido

Todo lo que el sitio afirma sobre alimentación sale de declaraciones textuales de Marcos en
`evidencia/entrevista-marcos-2026-08-27.md`, y **de ninguna otra parte**:

| Textual | Dónde se usa |
|---|---|
| P6 · *"Orientación nutricional general"* / *"Estrategia nutricional más individualizada"* | El componente y su nombre. **"Estrategia nutricional" es palabra suya**, no nuestra |
| P2 · *"todo tipo de dietas, normal, vegetariana, vegana, volumen, definición"* | La línea de §3 sobre vegetariano y vegano |
| P6 · *"Seguimiento de peso/medidas/fotos"* | Que el ajuste nutricional ocurra en la medición mensual ya existente |

**No se agregó ninguna prestación que Marcos no haya declarado.**

### La regla de atribución — es la que no se puede romper

**Marcos Cavallaro es entrenador.** El componente nutricional pertenece al **acompañamiento de
Cavallaro Fitness**, no a una credencial personal suya. En ninguna superficie puede escribirse
ni insinuarse que sea nutricionista o licenciado en nutrición, ni atribuírsele matrícula o
título que no tiene.

Esa separación está codificada, y así es como está hecha:

| Mecanismo | Dónde vive |
|---|---|
| **El sujeto gramatical.** El entrenamiento va en primera persona ("armo tu plan", "corrijo", "comparo"); lo nutricional, no ("se define la estrategia nutricional del mes") | `index.html` §2, y la frase que instala la arquitectura está en el `.intro` de "Cómo trabajo" |
| **El nodo `Person` no toca nutrición.** `jobTitle` sigue siendo "Entrenador", `description` es sólo de entrenamiento, `knowsAbout` no incluye ningún término nutricional y **no hay `hasCredential`** | JSON-LD de `index.html` |
| **Lo nutricional vive en `Service`**, como ítem del `hasOfferCatalog` de cada modalidad: es algo que el servicio incluye, no una propiedad de la persona | JSON-LD, nodos `#presencial` y `#online` |
| **La regla escrita para modelos y buscadores** | `llms.txt`, sección Identidad |
| **La bio conserva el ancla de atribución**: *"Su formación declarada es de instructor de musculación"* | `index.html` §6 |

El sitio **no lleva disclaimers** sobre esto, y es deliberado: la separación se sostiene con
lenguaje normal y con el modelado semántico, no con advertencias.

### Qué quedó expresamente afuera

- **Patologías y condiciones clínicas.** Diabetes, colon irritable, FODMAP, cetogénica y
  cualquier otra condición de salud declarada en P2. No entran, y no se agregó ninguna FAQ
  sanitaria: el sitio no abre capa clínica.
- **Esteroides y fármacos** (P6). Fuera, sin discusión.
- **Suplementación.** Ver el bloque de abajo.
- **Dietas, menús, gramajes y planes alimentarios publicados.** El sitio no publica ninguno.
- **El descriptor gráfico "Entrenador personal - Nutrición".** La identidad
  CAVALLAROFITNESS / ENTRENAMIENTO PERSONALIZADO **no cambió** y no vuelve automáticamente por
  esta decisión. Requiere instrucción propia.

### Suplementación — decisión abierta, no resuelta

Marcos declaró en P6 una *"Guía de suplementación natural"* de ocho ítems que incluye
*"Dosis y forma de consumo"*. **Esta aprobación no la cubre y no se reincorporó por
inferencia.**

El sitio hoy **no afirma nada sobre si el servicio la ofrece o no**. Lo único que dice es una
frase verificable sobre sí mismo: *"el sitio no publica pautas, dosis ni protocolos de
suplementación"* (`llms.txt`). Se evitó a propósito la afirmación absoluta anterior —"no ofrece
suplementación"—, que podría dejar de ser verdadera.

**Queda como decisión pendiente del titular.** Si se aprueba, es otra intervención.

### Licenciada en Nutrición — pendiente, y no se anticipa nada

Se informó que existe una Licenciada en Nutrición que autorizó la incorporación del componente.
**El sitio no la menciona, y no debe mencionarla mientras no estén los datos de abajo.** Nada
de lo publicado depende de ella: la implementación actual se sostiene sola.

No se inventó ni se anticipó nombre, matrícula, modalidad de intervención ni relación
contractual. **Tampoco se dejó predeterminado cómo se la modelaría en el JSON-LD**: no se
asume `provider`, ni `employee`, ni `contributor`, ni ninguna otra relación con los nodos
`Service`. **El modelado semántico de su vínculo queda pendiente de determinar**, y sólo puede
decidirse una vez que se conozca su intervención profesional concreta.

Datos necesarios antes de cualquier afirmación pública:

1. Nombre y apellido completos.
2. Número de matrícula y jurisdicción que la otorgó.
3. **Autorización escrita de ella misma** para ser nombrada en el sitio. La que hay hoy fue
   transmitida por terceros y no alcanza.
4. **Cuál es su intervención real**, en una frase que ella suscriba: elaborar, revisar o
   recibir derivaciones son tres cosas distintas y se escriben distinto — y cada una implica
   un modelado semántico diferente.
5. Si el alumno tiene contacto directo con ella.
6. Si está incluida en el precio del acompañamiento o se contrata aparte.
7. Perfil público o sitio, si lo tiene, para el `sameAs`.

Con los puntos 1 a 4 resueltos recién puede decidirse **qué** afirma el sitio y **cómo** se
modela. Antes de eso, no se escribe.
