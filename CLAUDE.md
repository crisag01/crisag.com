# crisag.com — instrucciones para Claude Code

Portafolio personal de Cristian Armenta (ingeniero industrial y analista de datos).
Idioma de la página y de los mensajes de commit: español de México.

## Qué es este proyecto
- Una sola página estática: `index.html` (HTML + CSS + JS dentro del mismo archivo, sin frameworks ni build).
- Se publica con GitHub Pages desde la rama `main`, carpeta raíz.
- Cinco pantallas dentro del mismo archivo: `v-inicio`, `v-perfil`, `v-proyectos`, `v-trayectoria`, `v-contacto`.
  Se navega con `#inicio`, `#perfil`, `#proyectos`, `#trayectoria`, `#contacto` (no cambiar esos nombres).
- El tema es "Noche": fondo `#1a1613`, texto `#e6dccd`. No usar negro puro ni blanco puro
  (se eligió así a propósito para no cansar la vista de noche).

## CUIDADO con el tamaño del archivo
`index.html` pesa unos 0.8 MB porque lleva incrustados el retrato, su máscara y el CV en PDF
(en `window.CRISAG_ASSETS`, una línea gigantesca en base64).
- No imprimas ni leas el archivo completo. Busca con `grep -n` y lee solo rangos de líneas.
- No reescribas el archivo completo: haz ediciones puntuales sobre texto único.
- Nunca modifiques ni muestres las cadenas base64.

## Dónde está cada cosa
- Contacto (correo, LinkedIn, GitHub): objeto `CONFIG` al inicio del `<script>` final.
- Textos de proyectos y trayectoria: dentro de `<div class="view" id="v-...">`.
- Colores del tema Noche: bloque `/* Tema Noche */` al final del `<style>`.
- Retrato animado: función `initPortrait()` (WebGL). No tocar el shader sin avisar.

## Reglas de Git (importante)
- Nunca hagas commit directo en `main`.
- Cada cambio va en una rama nueva con nombre descriptivo (`cambio-menu`, `texto-contacto`...).
- Commits pequeños y con mensaje claro en español.
- Para subir la rama usa `git push -u origin <rama>` y abre un Pull Request con `gh pr create`.
  Yo hago el merge.
- Nunca uses `git push --force` ni `git reset --hard` sin pedírmelo.
- Las versiones estables se marcan con etiquetas (`v1.0`, `v1.1`...). Para volver atrás se usa
  `git revert` o traer el archivo de una etiqueta; no se reescribe el historial.
- No guardes contraseñas, tokens ni claves en el repositorio.

## Antes de dar un cambio por terminado
- Abre `index.html` en el navegador y revisa Inicio, Perfil, Proyectos, Trayectoria y Contacto,
  en computadora y en celular (≤ 760 px).
- El menú, el botón "Descargar CV" y los enlaces de contacto deben seguir funcionando.
- No debe haber desbordamiento horizontal ni errores en la consola.
