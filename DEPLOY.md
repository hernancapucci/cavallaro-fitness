# DEPLOY — CAVALLARO FITNESS

## 1. Estado actual

Publicado en GitHub Pages desde la rama `main`, carpeta raíz, con dominio propio
**`marcoscavallaro.com`** (registrado el 02/09/2026 en dattatec.com / DonWeb).

**El sitio es indexable.** Se abrió el 02/09/2026. `404.html` conserva su `noindex`,
como corresponde a una página de error.

Pendiente documental que no bloquea: la cesión de derechos del retrato M-02 sigue
sin resolverse (ver §9). El logotipo de Under Armour en cuadro quedó resuelto por
decisión del responsable del proyecto y no se modifica.

## 2. Dominio propio y apertura a indexación — hecho el 02/09/2026

El sitio vive en el ápice `marcoscavallaro.com`; `www` redirige ahí. La conexión se
hace con el archivo **`CNAME`** en la raíz del repositorio, que es el mecanismo de
GitHub Pages: su contenido es una sola línea con el dominio, sin `https://` ni barra.

**DNS cargado en DonWeb** — cuatro registros A en el ápice a `185.199.108.153`,
`.109.153`, `.110.153` y `.111.153`, más un CNAME de `www` a `hernancapucci.github.io.`
Ningún registro más.

Lo que se cambió al migrar (los seis lugares donde vivía la URL absoluta):

1. `<link rel="canonical">` de `index.html` y de `tarjeta/index.html`
2. `og:url`, `og:image` y `twitter:image` de las dos páginas
3. `@id`, `url` y `contentUrl` del JSON-LD de las dos páginas
4. `sitemap.xml` (2 URLs + `lastmod`) y `robots.txt` (`Disallow` → `Allow` + Sitemap)
5. `llms.txt` (URLs + sección Estado) y el campo `URL` de `marcos-cavallaro.vcf`
6. `404.html`: las rutas absolutas dejaron de ser `/cavallaro-fitness/...` y pasaron a
   `/...`, porque el sitio ya no vive en un subdirectorio. **Esto es fácil de olvidar
   y rompe el 404 en silencio.**

Y se retiró el `<meta name="robots" content="noindex, nofollow">` de las dos páginas
públicas. GitHub hace **301 automático** desde `hernancapucci.github.io/cavallaro-fitness/`
al dominio: la URL vieja no se pierde, redirige.

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

## 5. Repositorio: la mejora que conviene hacer

Hoy el sitio vive en un repositorio de proyecto, y por eso la URL tiene una carpeta
anidada (`/cavallaro-fitness/`). Lo correcto es una **organización de GitHub llamada
`cavallarofitness`** con un repositorio `cavallarofitness.github.io`: la URL queda en
la raíz (`https://cavallarofitness.github.io/`), la migración al dominio es más limpia,
y de paso **reserva el nombre de la marca en GitHub**. La organización se crea a mano
desde la web, es gratis y lleva dos minutos.

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
