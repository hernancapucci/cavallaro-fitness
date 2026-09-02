# CAVALLARO FITNESS

Sitio de **Marcos Cavallaro**, entrenador. Entrenamiento personalizado, presencial en
Rosario y online.

**URL:** https://marcoscavallaro.com/
**Hosting:** Vercel, sin build. **Estado:** publicada e indexable desde el 02/09/2026.

## Qué es

Sitio estático de dos superficies. Sin backend, sin base de datos, sin CMS, sin
framework, sin analytics, sin cookies, sin service worker.

```
/                     home, ocho bloques
/tarjeta/             tarjeta de contacto
/marcos-cavallaro.vcf vCard 3.0
/404.html             página de error
/robots.txt  /sitemap.xml  /llms.txt
vercel.json           trailingSlash, cabeceras del .vcf y cache de /assets
/assets/css/site.css  una sola hoja, compartida por las dos páginas
/assets/fonts/        Archivo variable (SIL OFL), 34 KB
/assets/img/          identidad (SVG), fotografía (WebP), OG (JPG), íconos
```

## Cómo está construido

HTML escrito a mano y una hoja de estilo. La razón es de proporción: dos páginas
estáticas no justifican un generador, un bundler ni una dependencia. Todo el contenido
crítico está en el HTML servido; no hay nada inyectado por JavaScript. **La única línea
de JavaScript del sitio es la que no existe.**

El diseño no es una plantilla: implementa el sistema visual cerrado en
`~/CAVALLARO FITNESS/05-SISTEMA-VISUAL/` (dirección D, ámbar quemado, dosis cromática).
Las reglas que no hay que romper están en `DEPLOY.md` §6.

## Qué NO dice el sitio, y es deliberado

Nada de nutrición, dietas ni suplementación · nada de rehabilitación ni de patologías ·
ningún título sin documento · ningún resultado sin acta · ninguna dirección, ninguna
sede, ningún gimnasio nombrado · ningún precio · ninguna promesa de plazo.

## Reproducir los assets

```sh
python3 "~/CAVALLARO FITNESS/06-IMPLEMENTACION/assets.py"
```

Emite la identidad desde la geometría maestra congelada y aplica a la fotografía el
tratamiento aprobado. No editar los SVG ni las imágenes a mano.

---

Sitio diseñado y desarrollado por [OwnSite Studio](https://ownsitestudio.com).
