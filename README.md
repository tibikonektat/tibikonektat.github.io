# Blog (GitHub Pages, gratis)

Sitio estático generado con Jekyll — el mismo motor que usa GitHub Pages, así
que **no necesitas instalar nada ni "compilar" el sitio tú mismo**: subes los
archivos a GitHub y ellos lo construyen y publican automáticamente.

## Qué hay aquí

- `_config.yml`: configuración del sitio (título, tema, direcciones de
  propinas, etc).
- `_posts/`: aquí van los artículos. El agente de contenido los genera
  automáticamente con `publish_blog.py` (ver `content_affiliate_agent/`).
- `index.md`, `about.md`: portada y página "sobre este blog" (explica cómo
  se financia el sitio y el aviso de que no es asesoramiento financiero).
- `_includes/custom-foot.html`: aquí pegas el código de A-Ads cuando lo
  tengas (se inserta solo en todas las páginas, no hay que tocar nada más).
- `descargas.md` + `descargas/`: página de descargas gratuitas con histórico
  de velas de 15m de BTC y ETH (ver sección más abajo).

## Descargas gratuitas de histórico (BTC/ETH 15m)

`descargas.md` enlaza a dos CSV en `descargas/` (BTCUSDT_15m.csv,
ETHUSDT_15m.csv) para que cualquiera los baje gratis. Se generan y mantienen
al día con `../ia/publicar_historicos_en_blog.py`, que:

1. Descarga las velas nuevas de BTC y ETH (15m) desde la API pública de
   Bitget (sin API key, solo datos de mercado).
2. Copia los CSV actualizados a `descargas/`.
3. Hace `git add/commit/push` de este repositorio.

Para que quede siempre al día sin tocar nada, prográmalo en el Programador
de tareas de Windows para que corra una vez al día:

1. Abre "Programador de tareas" → Crear tarea básica.
2. Desencadenador: Diariamente, a la hora que prefieras.
3. Acción: Iniciar un programa → `pythonw.exe` (o `python.exe`) con
   argumento `C:\Users\jaume\Desktop\ia\publicar_historicos_en_blog.py` y
   "Iniciar en": `C:\Users\jaume\Desktop\ia`.

También puedes ejecutarlo a mano cuando quieras: `python
publicar_historicos_en_blog.py` desde la carpeta `ia`.

## Monetización: sin afiliados, con A-Ads + propinas

Este blog no usa programas de afiliados (piden verificar tu identidad para
poder pagarte). En su lugar:

1. **A-Ads** (`https://a-ads.com`): red de anuncios que paga en
   criptomonedas. Para darte de alta como "publisher" solo piden un email y
   la URL donde vas a poner el anuncio — nada de verificación de identidad.
   Una vez aprobado, te dan un código `<iframe>` que pegas en
   `_includes/custom-foot.html` (ver el propio archivo, tiene un ejemplo) y
   aparecerá en todas las páginas del blog automáticamente.
2. **Propinas voluntarias**: pon tus direcciones de wallet (BTC, ETH/USDT)
   en `_config.yml` bajo `propinas:` — se muestran en `about.md`. Cualquiera
   puede crear una wallet gratis sin verificar identidad; solo hace falta
   verificarla si algún día quieres pasar ese dinero a euros en un exchange.

Los ingresos reales (lo que pague A-Ads, las propinas que recibas) hay que
apuntarlos a mano en el `ledger.py` del agente de contenido con
`record_revenue()` — A-Ads tiene API de reporting si más adelante quieres
automatizar eso.

## Cómo publicarlo gratis (paso a paso)

Esto lo tienes que hacer tú, porque implica crear una cuenta a tu nombre.
Te lo dejo lo más sencillo posible:

1. Crea una cuenta gratuita en [github.com](https://github.com) si no tienes.
2. Crea un repositorio nuevo. Si quieres que tu blog viva en
   `tunombre.github.io`, el repositorio se debe llamar **exactamente**
   `tunombre.github.io` (sustituye "tunombre" por tu usuario de GitHub).
   Si le pones otro nombre, el sitio quedará en
   `tunombre.github.io/nombre-del-repo`.
3. Sube todo el contenido de esta carpeta (`blog_site/`) a ese repositorio.
   Con Git instalado, desde esta carpeta:
   ```bash
   git init
   git add .
   git commit -m "primer commit del blog"
   git branch -M main
   git remote add origin https://github.com/TU_USUARIO/TU_USUARIO.github.io.git
   git push -u origin main
   ```
4. En GitHub, entra al repositorio → **Settings** → **Pages** → en "Build and
   deployment" elige "Deploy from a branch", rama `main`, carpeta `/ (root)`.
   Guarda.
5. En 1-2 minutos tu blog estará en `https://tunombre.github.io`. Esa es la
   URL que le darás a A-Ads cuando te pidan dónde vas a poner el anuncio.

Cada vez que el agente genere y publique artículos nuevos (`publish_blog.py`),
solo tienes que volver a hacer `git add . && git commit -m "nuevos articulos"
&& git push` para que aparezcan en el blog en vivo.

## Para que Google encuentre el blog (tráfico de buscadores)

Publicar el sitio no basta para que llegue gente por Google: hay que decirle
a Google que existe. Con `jekyll-sitemap` ya activado (ver `_config.yml`),
el sitio genera solo un `sitemap.xml`. Pasos, gratis, una sola vez:

1. Entra en [Google Search Console](https://search.google.com/search-console)
   con tu cuenta de Google.
2. Añade tu sitio (`https://tunombre.github.io`) como propiedad.
3. Verifica que eres el dueño (Search Console te da varias opciones; la más
   simple para GitHub Pages suele ser subir un archivo HTML de verificación
   a esta misma carpeta, o añadir una etiqueta meta — Search Console te
   guía paso a paso).
4. Una vez verificado, envía `https://tunombre.github.io/sitemap.xml` en la
   sección "Sitemaps".

A partir de ahí Google indexa las páginas poco a poco (puede tardar
semanas). El otro canal de tráfico es Instagram: pon la URL del blog en la
biografía de tu cuenta (ver `content_affiliate_agent/README.md`, sección de
Instagram).
