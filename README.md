# CAVALLARO FITNESS — sitio V1

Sitio estático de **Marcos Cavallaro** (marca *Cavallaro Fitness*), entrenador.
Entrenamiento personalizado, presencial en Rosario y online.

- **Dos páginas:** `/` (8 bloques, uno reservado) y `/tarjeta/`.
- **Sin backend, sin framework, sin dependencias, sin analytics, sin cookies.**
- Todo el contenido crítico está en el HTML servido. El único JavaScript propio
  son 5 líneas que muestran la barra fija de WhatsApp en móvil.

## Estado

**V1 EN REVISIÓN — publicada con `noindex`.** Ver `DEPLOY.md` para el procedimiento
de publicación y para la migración a dominio propio.

## Estructura

```
index.html                 página principal
tarjeta/index.html         tarjeta digital de contacto
marcos-cavallaro.vcf       archivo de contacto (vCard 3.0)
llms.txt · robots.txt · sitemap.xml
assets/fonts/              Barlow Condensed 600 y 700 (woff2, autoalojadas, SIL OFL)
assets/img/                retratos, favicon, apple-touch-icon, imagen OG
```

## Reglas duras que no se pueden romper al editar

1. **Nunca se declara una dirección.** Ni en el HTML, ni en el `.vcf`, ni en los datos
   estructurados, ni en una ficha de Google Business. Marcos trabaja dentro de gimnasios
   de terceros. La ubicación se comunica por **zonas**: *centro* y *Echesortu*.
2. **No se nombra ningún gimnasio.** La marca no hace pie en ninguno.
3. **Nada de nutrición, dietas ni suplementación.** Ni como servicio, ni como contenido,
   ni como palabra clave, ni en `knowsAbout`. El sitio lo dice explícitamente en el bloque
   de preguntas, y eso es deliberado.
4. **Nada de esteroides ni fármacos, en ninguna superficie.**
5. **Nada sanitario:** ni rehabilitación, ni tratamiento, ni recuperación de lesiones.
6. **Ninguna promesa de resultado ni de plazo** (art. 8, Ley 24.240).
7. **Ningún título deportivo sin documento.** El único resultado publicado es el que
   figura en un acta oficial enlazada.
8. **Schema:** `Person` + `Brand`. Nunca `LocalBusiness`, `Organization`, `Review`
   ni `AggregateRating`.

El fundamento completo de cada regla está en el expediente del proyecto
(`00-EXPEDIENTE-INVESTIGACION.md`) y en la carpeta `02-ARQUITECTURA/`, fuera de este repo.

---

Sitio diseñado y desarrollado por [OwnSite Studio](https://ownsitestudio.com).
