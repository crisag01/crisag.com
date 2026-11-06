# crisag.com — portafolio de Cristian Armenta

Página web de una sola pieza (`index.html`): HTML, CSS y JavaScript sin dependencias,
con el retrato animado, el CV descargable y los datos de contacto incluidos en el mismo archivo.

## Cómo editar
- Datos de contacto: objeto `CONFIG` al inicio del `<script>` final de `index.html`
  (`email`, `linkedin`, `github`).
- Para ver cambios en local, abre `index.html` con doble clic en tu navegador.

## Flujo de trabajo
1. `main` es la versión publicada. No se edita directamente.
2. Cada cambio se hace en una rama nueva (`cambio-...`), se revisa en local y se une a `main` con un Pull Request.
3. Las versiones estables se marcan con etiquetas (`v1.0`, `v1.1`...) para poder volver a ellas.
