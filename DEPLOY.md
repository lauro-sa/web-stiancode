# Deploy — stiancode.dev

Hacer push a `main` **no publica nada**: es un sitio estático de Astro que se sube a mano a
Hostinger (la misma cuenta que Herreelec y TPH, alias SSH `hostinger-herreelec`).

```bash
npm run build                      # genera dist/
# 1º el respaldo de lo que está online
rsync -az hostinger-herreelec:domains/stiancode.dev/public_html/ ~/dev/respaldos/stiancode-online-$(date +%F)/
# 2º mirar qué borraría --delete (fuera de _astro/, no debería aparecer casi nada)
rsync -azn --delete --itemize-changes dist/ hostinger-herreelec:domains/stiancode.dev/public_html/ | grep '^\*deleting'
# 3º subir
rsync -az --delete dist/ hostinger-herreelec:domains/stiancode.dev/public_html/
```

- El destino va **siempre con el dominio explícito**: la cuenta aloja siete dominios y
  `~/public_html` apunta a stianlauro.com.
- **Armar desde un árbol limpio** (lo commiteado): todo lo que haya en `public/` se publica, también
  un archivo suelto que quedó sin commitear. Así se habían publicado `proyectos/src/env.d.ts` y
  `proyectos/.astro/types.d.ts`.
- **Novedades** («Pulso Tech») trae las noticias en el momento del build: cada build la cambia.

## Registro

| Fecha | Qué |
|---|---|
| 2026-10-22 | Parche de seguridad: Astro 4.16 → **7.4.2** y `@astrojs/sitemap` al día (avisos de npm: RCE crítico en la optimización de imágenes AVIF, XSS; más Vite y transitivas). `npm audit` en 0. Las 14 páginas comparadas contra el build anterior (mismo texto) y la portada contra lo online, en escritorio y celular. Se borraron del servidor los dos archivos de desarrollo publicados por error. |
