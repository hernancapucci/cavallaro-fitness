# DEPLOY — CAVALLARO FITNESS

## 1. Estado actual

Publicado en GitHub Pages desde la rama `main`, carpeta raíz.

**El sitio está en `noindex`.** Es deliberado: el copy todavía no fue aprobado por
Marcos, quedan formulaciones marcadas para revisión jurídica y la foto no tiene
cesión de derechos del fotógrafo.

## 2. Para publicarlo de verdad (indexable)

Tres cambios, en este orden:

1. `index.html` → borrar la línea `<meta name="robots" content="noindex, nofollow">`
2. `tarjeta/index.html` → borrar la misma línea
3. `robots.txt` → cambiar `Disallow: /` por `Allow: /`

Después: dar de alta el sitio en Google Search Console y enviar el `sitemap.xml`.

## 3. Verificaciones obligatorias antes de publicar

| # | Verificar | Riesgo si falla |
|---|---|---|
| 1 | El link `wa.me/5493413357925` **en un teléfono real** | Se pierden todas las consultas sin que nadie se entere |
| 2 | `curl -I <url>/marcos-cavallaro.vcf` → qué `Content-Type` devuelve | Si vuelve `text/plain`, iOS muestra el texto en vez de ofrecer guardar el contacto |
| 3 | La tarjeta abierta **desde adentro de WhatsApp**, en iPhone y en Android | Es el navegador por el que va a entrar casi todo el mundo, y el más restrictivo |
| 4 | Cómo se ve la tarjeta OG al pegar el link en un chat | Es lo primero que ve el que recibe el link |

Si el `.vcf` falla en iOS: el respaldo es el atributo `download` (ya está puesto) y,
si tampoco, generarlo con JavaScript. Último recurso: los datos en texto seleccionable.

## 4. Migración a dominio propio

GitHub Pages hace **301 automático** desde la URL de `github.io` al dominio propio en
cuanto se conecta. La migración no pierde nada.

**Los 5 lugares donde vive la URL absoluta** (más el archivo `CNAME`):

1. `<link rel="canonical">` de `index.html` y de `tarjeta/index.html`
2. `og:url` y `og:image` de las dos páginas
3. `sitemap.xml` (2 URLs)
4. `@id` y `url` dentro del JSON-LD de las dos páginas
5. `llms.txt` (3 URLs) y el campo `URL` de `marcos-cavallaro.vcf`

Buscar y reemplazar `https://hernancapucci.github.io/cavallaro-fitness` por el dominio.
Nada más. Ningún link interno es absoluto.

**Dominio recomendado:** `cavallarofitness.com` como principal y
`cavallarofitness.com.ar` defensivo (este último exige CUIT/CUIL y Clave Fiscal
nivel 2 a nombre de Marcos).

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

## 6. V2 · Construcción sobre el sistema visual aprobado (2026-08-31)

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
