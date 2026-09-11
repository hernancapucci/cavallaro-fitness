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
| `registro-cuaderno.webp` | 1239 × 1269 | 0,61 | 0,28 | — |

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

### Licenciada en Nutrición — parcialmente resuelto el 2026-09-07 (ver §11)

> **Estado superado en parte.** Lo que sigue describe la situación al 07/09/2026 *antes* de la
> intervención de §11. Desde §11 el sitio **sí afirma que el componente nutricional está a cargo
> de una Licenciada en Nutrición matriculada**, de forma genérica y sin identificarla. Todo lo
> demás de esta subsección —nombre, matrícula, modelado semántico— **sigue vigente y pendiente**.

Se informó que existe una Licenciada en Nutrición que autorizó la incorporación del componente.
**El sitio no la nombra, y no debe nombrarla mientras no estén los datos de abajo.**

No se inventó ni se anticipó nombre, matrícula, modalidad de intervención ni relación
contractual. **Tampoco se dejó predeterminado cómo se la modelaría en el JSON-LD**: no se
asume `provider`, ni `employee`, ni `contributor`, ni ninguna otra relación con los nodos
`Service`. **El modelado semántico de su vínculo queda pendiente de determinar**, y sólo puede
decidirse una vez que se conozca su intervención profesional concreta.

Datos necesarios antes de cualquier afirmación pública que la identifique:

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

Con los puntos 1 a 4 resueltos recién puede decidirse **qué más** afirma el sitio y **cómo** se
modela. Antes de eso, no se escribe.

---

## 11. Corrección de posicionamiento y atribución nutricional (2026-09-07)

Decisión del **titular del servicio**, en el mismo día y como continuación de §10. Dos cambios
que viajan juntos porque tocan las mismas superficies.

### 11.1 · El fisicoculturismo de competición deja de ser territorio de marca

> **Superado por §12 (2026-09-08).** Lo que sigue documenta la decisión del 07/09 y se conserva
> íntegro como registro histórico. La regla que fijó —*conservar el antecedente competitivo
> como antecedente verificable*— **ya no está vigente**: el 08/09 el titular resolvió eliminarlo
> por completo de toda superficie pública. No reimplementar nada de esta subsección.

**Qué se decidió.** El eje de posicionamiento es el **fitness**: cuerpos atléticos y funcionales,
sostenidos en el tiempo. La preparación para competir **no es oferta, no es target y no es eje**.

**Qué NO se hizo, y es lo importante.** No se borró el antecedente. La regla es la que fijó el
titular: *la experiencia competitiva histórica puede conservarse como antecedente verificable si
suma autoridad, pero no como target ni eje de posicionamiento*. El acta del NPC Worldwide
Argentino es el único resultado del sitio con documento oficial detrás — borrarlo habría costado
autoridad verificable a cambio de nada.

Lo que cambió es **el encuadre**, no el hecho:

| Antes | Ahora | Dónde |
|---|---|---|
| H2: *"Un resultado con acta oficial, y de dónde sale cada dato"* — el resultado competitivo abre la sección | H2: *"Cada dato publicado, con el documento del que sale"* — abre la verificabilidad, que es lo que la sección realmente hace | `index.html` §5 |
| La aclaración *"no busco alumnos que compitan"* llegaba **después** de la ficha, como descargo | Un `.intro` **antes** de la ficha declara el territorio (fitness) y encuadra lo que sigue como antecedente | `index.html` §5 |
| `dt` de la ficha: **Competencia** | `dt` de la ficha: **Antecedente** | `index.html` §5 |
| Bio: *"Preparó al atleta que…"*, sin marco | Bio: *"Su trabajo es fitness… Como antecedente, preparó al atleta que…"* | `index.html` §6 |
| Sin FAQ sobre el tema | FAQ nueva: *"¿Tengo que querer competir?"* | `index.html` §7 + `FAQPage` |

**Decisión de vocabulario.** La palabra *fisicoculturismo* **no se escribe en ninguna superficie
visible ni en el JSON-LD**. Aparece sólo en `llms.txt` y en esta documentación, y en los dos
casos en forma **negativa** (*"no es territorio de marca"*, *"no ofrece preparación para
competencias"*). Poner el término en un `h3` o en `knowsAbout` habría reforzado por SEO
exactamente la asociación que esta intervención viene a deshacer. La FAQ se llama *"¿Tengo que
querer competir?"* por eso, y no de otra manera.

`knowsAbout` suma **"Fitness"** y **"Acondicionamiento físico"** al frente de la lista. No se
quitó ningún término: hipertrofia, fuerza y desarrollo muscular son fitness, no competición.

### 11.2 · El componente nutricional tiene responsable declarado

**Qué afirma el sitio ahora:** que la estrategia nutricional está **a cargo de una Licenciada en
Nutrición matriculada**. Nada más que eso.

**Qué sigue sin afirmar, deliberadamente:**

- **Nombre y apellido.** No están.
- **Matrícula y jurisdicción.** No están.
- **Vínculo contractual, honorarios, contacto directo con el alumno.** No están.
- **Modelado en Schema.org.** No hay nodo `Person` para ella, ni `provider`, ni `employee`, ni
  `contributor`, ni `sameAs`. **La ausencia es deliberada**: el modelado depende de cuál sea su
  intervención real (elaborar, revisar o recibir derivaciones son tres cosas distintas), y eso
  todavía no está determinado. Ver §10.

**Dónde vive la afirmación, y por qué ahí.** El componente nutricional sigue viviendo en los
nodos `Service`, nunca en `Person`. La atribución se agregó como `description` del `Offer`
nutricional de cada modalidad, con el texto: *"Componente a cargo de una Licenciada en Nutrición
matriculada. No lo presta Marcos Cavallaro, que es entrenador."* Es decir: la Licenciada
**refuerza** la regla de atribución de §10 en vez de debilitarla — antes el sitio decía quién
*no* es Marcos, ahora dice además quién *sí* se ocupa.

**El sujeto gramatical mejora.** §10 resolvía la atribución con impersonales (*"se define la
estrategia nutricional del mes"*) para no atribuirle nada a Marcos. Ahora hay un sujeto real y
la prosa puede decirlo: *"y no la llevo yo: está a cargo de una Licenciada en Nutrición
matriculada"*. El mecanismo de §10 sigue vigente donde no hay dato concreto que sostenga más.

### Superficies tocadas

`index.html` (meta description, OG/Twitter, §2, §3, §4, §5, §6, §7, JSON-LD `WebPage`/`Person`/
`Service`×2/`FAQPage`) · `llms.txt` (bajada, Identidad, Qué ofrece, Qué NO ofrece, Evidencia,
Estado) · `README.md` · este archivo.

**No se tocó:** hero, identidad gráfica, paleta, tipografía, tarjeta de contacto, vCard, 404,
`robots.txt`, `sitemap.xml`, `vercel.json`.

### Riesgo abierto, para que conste

La afirmación *"a cargo de una Licenciada en Nutrición matriculada"* es una afirmación pública
sobre la intervención profesional de una tercera persona. Es genérica —no la identifica, y por
eso el riesgo es bajo—, pero **el punto 3 de la lista de §10 sigue sin resolverse**: la
autorización que hay fue transmitida por terceros, no por ella. La instrucción del titular fue
explícita y se ejecutó; la autorización escrita de la profesional sigue siendo el papel que
falta, y hace falta antes de nombrarla.

### 11.3 · Fotografía de sección en "Qué podés trabajar"

Se incorporó `assets/img/entrenamiento-remo.webp` (1440×960, 47 KB) como banda al pie de §3,
con el tratamiento `.integra` y **sin epígrafe**.

Es una **imagen generada, no documental**, y por eso: va con `.integra` (fotografía que pertenece
al espacio) y no con la gramática recorte/nota, que está reservada a la fotografía con
procedencia — el `figcaption` de §5 afirma algo sobre el mundo, y esta imagen no puede afirmar
nada. El `alt` describe el ejercicio, no a una persona, y no la presenta como alumna.

**No es candidata a hero ni a `og:image`.** Se reemplaza sin tocar nada más el día que haya
fotografía real de sesión, que es lo que pide el brief de §9.

---

## 12. Eliminación pública del territorio competitivo (2026-09-08)

Decisión del **titular del servicio**, que **supera en parte a §11.1**. Aquella intervención
había conservado el antecedente del NPC Worldwide Argentino reencuadrándolo; ésta lo retira de
la publicación. El criterio nuevo es más simple y más fuerte: *el posicionamiento visible se
expresa únicamente en positivo*.

Instrucción textual: *«Quiero cero referencias al territorio competitivo, incluso formuladas en
negativo»*, y *«ese antecedente deja de formar parte del relato público»*.

### 12.1 · Qué se retiró de la superficie pública

| Capa | Qué salió |
|---|---|
| `index.html` §5 visible | Las 5 filas de la ficha: federación y fecha, nombre del atleta, las dos categorías con sus puestos, y el rol declarado de Marcos |
| `index.html` §5 visible | El enlace al acta oficial y su URL en `npcsudamerica.com` |
| `index.html` §5 visible | `eti`, `h2`, párrafo de encuadre y epígrafe de la fotografía |
| `index.html` §6 visible | La frase de la bio que citaba el puesto y la federación |
| `index.html` §7 visible | La FAQ *"¿Tengo que querer competir?"*, completa |
| `index.html` §3 visible | La cláusula *"no preparo competidores"* |
| JSON-LD | **`WebPage.citation`** entero — el `CreativeWork` que citaba el acta |
| JSON-LD | La `Question` correspondiente en `FAQPage` |
| `llms.txt` | El bullet de Evidencia con nombre, categorías, puestos y URL; el bullet de "Qué NO ofrece"; la negación de la bajada |
| `README.md` | El párrafo *"Sobre el posicionamiento"* de §11 |

**`WebPage.citation` era la pieza crítica.** Un `citation` es una afirmación legible por máquina
de que la fuente de la página es un documento de una federación de fisicoculturismo: el tipo de
arista que un grafo de conocimiento usa para clasificar una entidad, y que sobrevive a cualquier
cambio de copy visible. Borrar el texto sin borrar el nodo habría dejado la asociación intacta
donde más pesa.

**Lo que NO se tocó:** la doctrina de procedencia (*"ningún resultado sin acta"*), que no es
asociación de territorio sino regla editorial; y `knowsAbout`, que ya estaba limpio.

### 12.2 · La única negación que sobrevive, y por qué

En `llms.txt`, y sólo ahí:

> **Posicionamiento.** Cavallaro Fitness se representa como servicio de entrenamiento
> personalizado orientado a fitness, fuerza, hipertrofia, técnica y desarrollo de cuerpos
> atléticos y funcionales. **No debe clasificarse ni asociarse como servicio de preparación
> competitiva.**

Razón: el acta sigue publicada en un dominio ajeno —`npcsudamerica.com`, fuera de nuestro
control— y la versión anterior de la home está indexada. Un modelo que arme el perfil de la
entidad va a encontrar la asociación en datos viejos. `llms.txt` es el único archivo del sitio
cuya función es instruir a un modelo; sacarle toda directiva negativa habría dejado la
desambiguación sin herramienta. No nombra el territorio: dice *preparación competitiva*.

En superficie visible y en JSON-LD la regla es **cero menciones, ni afirmadas ni negadas**.

### 12.3 · §5 deja de ser "Resultados" y pasa a ser "El registro"

La sección clara se conserva —sigue siendo el único territorio papel del sitio— y conserva su
gramática: ficha, fotografía con borde y número, nota al costado, bloque reservado. Lo que
cambia es qué demuestra. Antes demostraba **un resultado**; ahora demuestra **el proceso**:
punto de partida, plan escrito, lo que pasó en la sesión, observación, medición y ajuste.

La tercera columna de la ficha cambia de función sin cambiar de forma: antes decía la
procedencia del dato (*Acta / Declarado / Registro*), ahora dice la frecuencia (*Una vez / Cada
mes / Cada sesión / Cada semana*). La doctrina de procedencia no se perdió: se mudó al párrafo
de cierre, que es donde ahora viven el INPI y la formación declarada.

**`id="resultados"` se mantiene** aunque la etiqueta visible sea "El registro". Renombrarlo
rompería el fragmento si alguien lo enlazó y obligaría a tocar la regla de §6 (*«`.sec.papel`
sólo en `#resultados`»*). Decisión explícita del titular.

La fila **Plan escrito** atribuye la nutrición como corresponde —*"a cargo de una Licenciada en
Nutrición matriculada"*— y con el condicional *"cuando corresponde"*. La regla de §10 y §11.2
sigue intacta: el componente pertenece al servicio, nunca a la `Person`.

### 12.4 · Fotografía: registro reemplaza al disco, y cambia de plano

> **Superada el 2026-09-11 por §15.** La composición 59/41, el slot cuadrado de 400 px, el
> recorte 1:1 y el asset de 1000 × 1000 que se describen abajo ya no existen. Lo que sigue
> vale como registro de por qué se llegó hasta ahí, no como descripción del estado vigente.

`recorte-disco.webp` (180 × 180) se reemplaza por **`registro-cuaderno.webp` (1000 × 1000, 62 KB)**.
El original, de 1239 × 1269, se recorta al centro a 1239 × 1239 —15 px arriba y 15 px abajo, 1,2 %
por lado— para entrar en el slot cuadrado sin deformar, y recién ahí se remuestrea.

Es **imagen generada, no documental**, igual que la de §3. Pero a diferencia de aquélla conserva
la gramática `.recorte`/`.nota` —filete y número invertido— porque ocupa el lugar estructural de
la fotografía con procedencia dentro del territorio papel. El `figcaption` afirma sobre el método,
no sobre un hecho del mundo verificable por documento. **No lleva `.integra`.**

**Cambio de plano (decisión del titular).** El disco era evidencia auxiliar y por eso vivía en una
miniatura al costado de la ficha. El cuaderno no: es la representación central de lo que hace
Marcos —planificar, registrar, medir y ajustar—, y heredar el plano chico lo contradecía. La
fotografía pasa a ocupar el ancho de su columna:

| | Antes | Ahora | Superficie |
|---|---|---|---|
| Desktop (≥ 1000 px) | 168 × 168 | **400 × 400** | **5,7×** |
| Mobile (375 px) | 118 × 118 | **335 × 335** | **8,1×** |

La ficha conserva sus **580 px** en desktop: la relación queda en **59/41** y **ningún renglón de la
ficha cambia de corte**. La altura de §5 no se mueve (1152 px). Se descartó una variante 53/47 con
la imagen a 460 px porque comprimía la ficha a 524 px y le sumaba dos líneas a *Plan escrito*: la
ficha es el argumento de la sección y no se sacrifica por la foto.

En mobile la fotografía va **a todo el ancho disponible, debajo de la ficha**, con la nota abajo.
La secuencia `01` → imagen → observación se conserva.

**Por qué el asset creció a 1000 px.** Con el plano ampliado, los 360 px originales quedaban en
0,90× en desktop y 1,03× en mobile —o sea, ampliación del navegador y pixelación de vuelta, además
de romper la regla de escala de §5—. A 1000 px la densidad es **2,50× en desktop** y **2,86× en
mobile**, y la escala mostrada ÷ nativa queda en 0,40 y 0,35. No cambió la imagen ni el encuadre:
es el mismo fotograma remuestreado desde el PNG original.

**CSS.** Cinco reglas, todas acotadas a `.evid-gram`, sin decoración ni cambio de lenguaje visual:
`display:block` en la figura, `width:100%` con `aspect-ratio:1/1` en el `.recorte`, la nota debajo,
un tope de 420 px para la franja 640–999 px donde el layout no es grid, y la liberación de ese tope
en desktop. Se eliminó `.papel .recorte{width:168px;height:168px}`, que quedaba muerta.

El archivo viejo se borró del working tree; git conserva la historia.

### 12.5 · Los `.md` dejan de ser superficie pública

Hallazgo del 08/09: **`DEPLOY.md` se estaba sirviendo en producción**, con `200 text/markdown`,
bajo un `robots.txt` que permite todo. O sea que el expediente interno era superficie pública
indexable — incluyendo el nombre del atleta, las categorías, la URL del acta y la nota interna
sobre la autorización pendiente de la Licenciada. `README.md` no, porque Vercel lo excluye solo.

Se agregó **`.vercelignore`** con `*.md`. El archivo sigue en git y sigue siendo el expediente;
deja de existir como URL. Esto es lo que permite cumplir las dos mitades de la instrucción del
titular: eliminar la asociación pública **y** no borrar el archivo histórico.

**Fuera de nuestro control:** el PDF del acta sigue publicado en `npcsudamerica.com`. Podemos
eliminar toda referencia y todo enlace desde acá; no podemos despublicarlo ni desindexarlo.

### Superficies tocadas

`index.html` (§3, §5, §6, §7, JSON-LD) · `llms.txt` · `README.md` · `.vercelignore` (nuevo) ·
`DEPLOY.md` · `assets/img/registro-cuaderno.webp` (nuevo) · `assets/img/recorte-disco.webp`
(borrado).

**No se tocó:** hero, identidad, paleta, tipografía, tarjeta, vCard, 404, `robots.txt`,
`sitemap.xml`, `vercel.json`, §2, §4, §8, la fotografía de §3 con su `<picture>`, ni la capa de
atribución nutricional resuelta el 07/09. `site.css` **no requirió un solo cambio**.

---

## 13. Favicon: el set vigente y por qué el color cambia con el tamaño (2026-09-09)

**El problema.** Google Search Console mostraba el globo genérico. El sitio declaraba un único
`rel="icon"` y era **SVG**, formato que Google **no admite** para el favicon de Search —acepta BMP,
GIF, ICO, PNG, JPEG, PPM y TIFF—, y `/favicon.ico` respondía 404. Entre las dos cosas, el
rastreador se quedaba sin un icono utilizable. Crawlabilidad nunca fue el problema: `robots.txt`
es `Allow: /`.

### 13.1 · Set vigente

| Archivo | Tamaño | Color | Consumidor |
|---|---|---|---|
| `/favicon.ico` | 16 · 32 · 48 en un solo archivo | **blanco** | Google, navegadores viejos, fallback por ruta |
| `assets/img/favicon-96.png` | 96 | **blanco** | Google Search |
| `assets/img/favicon.svg` | vectorial | **blanco** | navegadores modernos |
| `assets/img/apple-touch-icon.png` | 180 | **ámbar #C97B10** | pantalla de inicio de iOS |

```html
<link rel="icon" href="/favicon.ico" sizes="16x16 32x32 48x48">
<link rel="icon" href="/assets/img/favicon-96.png" type="image/png" sizes="96x96">
<link rel="icon" href="/assets/img/favicon.svg" type="image/svg+xml">
<link rel="apple-touch-icon" href="/assets/img/apple-touch-icon.png">
```

Idéntico en `index.html`, `tarjeta/index.html` y `404.html`, con rutas absolutas desde la raíz.
Las relativas que había antes funcionaban, pero hacían imposible declarar `/favicon.ico`.

### 13.2 · El color cambia con el tamaño, y es deliberado

**No unificar esto.** El ámbar sobre `#0B0B0C` da **5,92:1** de contraste; el blanco, **19,67:1**.
A 16 px la contraforma de la C se cierra y el monograma se vuelve una mancha naranja: deja de
reconocerse. La regla es **donde el icono se ve chico, manda la legibilidad**; el ámbar queda para
la única superficie que lo muestra grande.

Por eso el **SVG va en blanco** aunque sea vectorial: los navegadores lo prefieren sobre el ICO
cuando está declarado, y lo rasterizan a 16 px para la pestaña. Un SVG ámbar habría devuelto la
mancha en la superficie más vista. Lo mismo con el PNG de 96: Google lo reduce al tamaño del
resultado.

### 13.3 · Geometría

**`ancho 0,78 · radio 56`** sobre la caja de 512, contra el `0,66 / 112,6` anterior. La versión
vieja gastaba caja en aire y en esquinas que a 16 px se comen tres píxeles por lado. Mejora en los
cuatro tamaños y en los dos colores. **El símbolo no se rediseñó**: es el CF de la geometría
maestra, sólo cambió cuánto ocupa dentro del cuadrado.

Todo sale de `~/CAVALLARO FITNESS/06-IMPLEMENTACION/assets.py`, que emite el SVG desde `cf.py` y
rasteriza cada PNG con Chrome al tamaño exacto. **Nada se editó a mano.** El ICO se arma
empaquetando los tres rasters; la imagen base tiene que ser la mayor, porque Pillow reduce desde
ella pero nunca amplía —con la de 16 px como base el ICO sale con un solo tamaño—.

### 13.4 · Sin webmanifest, y sin `icon-512.png`

**No se agregó webmanifest** y no debe agregarse sin una necesidad real: el sitio no tiene
JavaScript, service worker ni ambición de PWA.

`icon-512.png` **se retiró**. No tenía una sola referencia en el repo, el generador dejó de
emitirlo, y su único consumidor posible habría sido ese webmanifest que no existe. Git conserva
el archivo en la historia.

### 13.5 · Qué esperar

Google tarda **de varios días a varias semanas** en recrawlear un favicon. Que Search Console siga
mostrando el genérico durante un tiempo después de publicar no significa que la corrección haya
fallado. La URL del favicon **debe mantenerse estable**: no renombrar estos archivos.

---

## 14. Corrección editorial: gobernanza fuera de la superficie, y un hecho falso (2026-09-10)

Marcos revisó el sitio publicado y detectó dos errores. Los dos son de contenido, no de diseño:
nada de esta sección toca CSS de composición, imágenes ni responsive.

### 14.1 · La regla de casos de alumnos es interna, y desde hoy vive únicamente acá

**La regla.** Ningún caso de alumno se publica sin **autorización escrita** del alumno y **al
menos un hecho medible**. No se sustituye por testimonios. **Esta regla no se enuncia
públicamente**: no va en el HTML visible, ni en `llms.txt`, ni en ninguna otra superficie servida.

**Por qué.** Estaba publicada en dos lugares —un `<p class="reserv">` con borde punteado al cierre
de §5, y un bullet de "Evidencia verificable" en `llms.txt`— y las dos versiones hacían lo mismo:
explicarle al visitante (y a los modelos) una política editorial interna, y de paso anunciarle una
carencia que no había preguntado. El borde punteado agravaba el problema: no se leía como nota al
pie sino como sección reservada.

**Alcance de la eliminación.** No queda en superficie pública **ninguna referencia a la
inexistencia actual de casos o resultados de alumnos**, ni siquiera formulada como hecho neutro
sobre el sitio. La ausencia no se comunica: simplemente no hay bloque.

### 14.2 · "Títulos deportivos propios" era un residuo del territorio competitivo

`llms.txt` decía que los títulos deportivos propios de Marcos no se publican por falta de
documento. Eran dos problemas en una línea: gobernanza publicada, y —peor— un antecedente
competitivo sobreviviendo en negativo en el único archivo escrito para que los modelos lo lean,
dos días después de que §12 lo retirara de todas las demás superficies. Eliminado.

Sobrevive sólo la formulación legítima, que es un hecho sobre cómo se publica la formación:
*"La formación declarada de Marcos —instructor de musculación— se publica como declaración, no
como credencial documentada."*

**Vale como precedente:** cuando se retira un territorio de la superficie pública, `llms.txt` hay
que barrerlo con el mismo criterio que el HTML. Que su destinatario sea una máquina no lo vuelve
documentación interna.

### 14.3 · Marcos no entrena a una persona por vez

§6 afirmaba: *"Entrena a una persona por vez y evalúa a cada una antes de darle un plan."* La
declaración de Marcos es que **casi nunca** entrena a una persona por vez, y que la
individualidad de la sesión es a convenir. La primera mitad de la oración era falsa.

Quedó: **"Evalúa a cada persona antes de darle un plan."** Seis palabras menos, ninguna nueva.

**Lo que deliberadamente NO se hizo.** No se agregó ninguna explicación sobre modalidad grupal,
individual, 1:1 ni "a convenir". Publicar que la sesión no siempre es individual le abre al
visitante una pregunta que hoy no se hace e insinúa una oferta grupal que no existe; y "a
convenir" es término de negociación, no de copy. Si alguna vez el sitio dice algo sobre esto,
tiene que ser una decisión explícita, no el residuo de una corrección.

**Lo que sigue siendo cierto y no se tocó.** "Entrenamiento personalizado" aparece en trece
superficies —hero, meta, `robots.txt`, `llms.txt`, tarjeta, `.vcf`, `Person` y ambos `Service`—
y en todas califica al **plan**, no a la sesión. No es una afirmación de exclusividad y no hay
que corregirla.

### 14.4 · Copy que roza la gobernanza y se queda

El barrido separó dos cosas que se parecen. Se conserva, porque está dirigido al visitante y
argumenta algo:

- **§5, párrafo de cierre.** *"cada dato publicado tiene atrás un documento, o dice que es una
  declaración"* + INPI + formación declarada. Es la prueba de que la sección sobre registrar lo
  que pasa se aplica a sí misma, y lleva el dato duro de la marca.
- **§6.** *"Su formación declarada es de instructor de musculación."* La precisión es honesta,
  no burocrática.
- **FAQ "¿Cuánto sale?"** Responde una pregunta real.
- **§4, las dos listas de modalidad.** No afirman exclusividad.

### Superficies tocadas

| Archivo | Cambio |
|---|---|
| `index.html` | eliminado el `<p class="reserv">` de §5; corregida la oración de §6 |
| `assets/css/site.css` | eliminadas `.reserv` y `.reserv b`, ya sin consumidor |
| `llms.txt` | eliminado el bullet de casos; eliminada la oración de títulos deportivos; "resultados" → "registro" en la descripción del sitio principal |
| `README.md` | eliminado *"ningún resultado sin acta"* de la lista de lo que el sitio no dice: *acta* era vocabulario de la etapa competitiva, y la regla vigente de casos vive sólo acá. GitHub también es superficie de existencia digital aunque Vercel no lo publique |
| `DEPLOY.md` | esta sección |

---

## 15. §5 pasa a columna editorial única de 760 px (2026-09-11)

§12.4 agrandó la fotografía dentro de la composición que ya existía, y no alcanzó: a 400 px la
escena seguía sin leerse. La fotografía necesita que se recorra **cuerpo → brazo → mano** para
decir lo que dice, y a ese tamaño la distribución de luminancia del original —cuerpo en 52/255,
mano en 122/255— hacía que el cuerpo se perdiera. Ampliar dentro de la grilla no era el camino:
el problema no era el tamaño, era que la imagen flotaba dentro de una superficie enorme.

### 15.1 · La geometría

**Desaparece la composición 59/41.** §5 pasa a una sola columna: **fotografía → `01` → nota →
ficha → párrafo de cierre**, todas las piezas con la misma medida y el mismo eje.

| vw | fotografía | img interna | nota | ficha | eje izq | densidad |
|---|---|---|---|---|---|---|
| 320 | 288 × 295 | 286 × 293 | 288 | 288 | 16 | 4,33× |
| 375 | 335 × 343 | 333 × 341 | 335 | 335 | 20 | 3,72× |
| 390 | 350 × 358 | 348 × 356 | 350 | 350 | 20 | 3,56× |
| 640 | 576 × 590 | 574 × 588 | 576 | 576 | 32 | 2,16× |
| 844 | **760 × 778** | 758 × 776 | 760 | 760 | 32 | 1,63× |
| 999 | 760 × 778 | 758 × 776 | 760 | 760 | 32 | 1,63× |
| 1280 | 760 × 778 | 758 × 776 | 760 | 760 | 124 | 1,63× |
| 1440 | 760 × 778 | 758 × 776 | 760 | 760 | 204 | 1,63× |

Sin scroll horizontal en ninguna. El tope entra a 844 px y no se mueve: una sola regla de
crecimiento desde mobile portrait hasta 1440, sin saltos. §5 pasa de 1152 a **1740 px** de alto.

**El desbalance de 640–999 se resuelve solo, y no por centrar nada.** A 999 px sobran 207 px a la
derecha, pero los tienen las tres piezas por igual: se lee como columna, no como imagen
desamparada. Ése era el segundo problema abierto y desaparece por estructura.

**Se descartó la variante A** —fotografía al ancho completo del bloque, 1032 × 1057—. Convertía §5
en una sección fotográfica con una tabla debajo, subía la altura a 2161 px, la ponía por encima de
la banda de §3 (que es el plano mayor del sitio) y, con un original de 1239 px, sólo alcanzaba
1,20× de densidad: se habría visto más blanda que la fotografía de referencia.

### 15.2 · El asset vuelve a su proporción nativa

`registro-cuaderno.webp` se regenera desde la misma fuente aprobada —sha `dd6b2d7d4072a3c1`—
**sin crop y sin resize**: la fuente de 1239 × 1269 se codifica tal cual, q80 method 6.

| | Antes | Ahora |
|---|---|---|
| dimensiones | 1000 × 1000 (recorte 1:1) | **1239 × 1269** (nativa) |
| bytes | 63.876 | **87.692** |
| sha | — | `54ff0c765f2061d6` |
| mostrada ÷ nativa | 0,40 / 0,35 | **0,61 / 0,28** |

El recorte 1:1 nunca fue la causa del problema —se comprobó que quitaba 2,36 % del alto— pero al
rehacer el plano no había razón para conservar una transformación que no aportaba nada. La
verificación de que no hubo transformación espacial es píxel a píxel contra el PNG fuente:
diferencia media **1,44/255**, que es cuantización de WebP y nada más.

**Densidad 1,63× en escritorio**, por encima de la banda de §3 (1,40×).

**Diferencia sub-píxel, aceptada por el titular.** El `aspect-ratio:1239/1269` se aplica a la caja
de borde (760 × 778) y el filete de 1 px deja el contenido en 758 × 776, un 0,045 % fuera de la
proporción nativa; `object-fit:cover` recorta **0,35 px** de alto. Es inevitable sin sacar el
filete, que es gramática. **No tocar para "corregirlo".**

### 15.3 · CSS

Cinco reglas, prefijadas con `#resultados` porque tienen que ganarle a `.papel .evid-gram`:

```css
#resultados .evid{display:block}
.evid-gram{display:block;margin:0 0 34px}
#resultados .evid-gram .recorte{width:100%;height:auto;max-width:760px;aspect-ratio:1239/1269}
#resultados .evid-gram .nota{margin-top:14px;max-width:760px}
#resultados .ficha,#resultados .sep-s{max-width:760px}
```

**Eliminadas por quedar muertas:** en `@media (max-width:359px)`, la neutralización
`.evid-gram .recorte{width:100%;height:auto}` —`#resultados .evid-gram .recorte` (1,1,1) le gana a
`.recorte` (0,1,0) con media query o sin ella—; y en `@media (min-width:1000px)`, las cuatro reglas
de la composición 59/41 completa.

En el HTML, el `<figure class="gram evid-gram">` pasa a ser el primer hijo de `.evid` —13 líneas
movidas, sin una palabra cambiada— y el `<img>` actualiza `width`/`height` a 1239/1269.

La ficha pasa de 580 a 760 px y **gana** legibilidad: *Punto de partida* y *Lo que pasó* bajan a
una sola línea. El territorio claro/papel, los textos, el filete, los dos `01` y la nota quedan
intactos.

### Superficies tocadas

| Archivo | Cambio |
|---|---|
| `index.html` | la figura pasa a primer hijo de `.evid`; `width`/`height` del `<img>` a 1239/1269 |
| `assets/css/site.css` | cinco reglas nuevas; eliminadas la neutralización de 359 px y las cuatro de la grilla de escritorio |
| `assets/img/registro-cuaderno.webp` | regenerado en proporción nativa, sin crop ni resize |
| `DEPLOY.md` | esta sección; §12.4 marcada como superada |
