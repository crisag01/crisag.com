# crisag.com — instrucciones para Claude Code

Portafolio personal de Cristian Armenta (ingeniero industrial y analista de datos).
Idioma de la página y de los mensajes de commit: español de México.

## Qué es este proyecto
- Una sola página estática: `index.html` (HTML + CSS + JS dentro del mismo archivo, sin frameworks ni build).
  ÚNICA excepción: el CV va como archivo aparte, `CV_Cristian_Armenta.pdf`, junto a `index.html`
  (incrustado en base64 no se descargaba en iPhone). Todo lo demás se queda dentro de `index.html`.
- Se publica con GitHub Pages desde la rama `main`, carpeta raíz, en el dominio `crisag.com`.
  El archivo `CNAME` (contiene `crisag.com`) es necesario para el dominio: no borrarlo ni renombrarlo.
- Cinco pantallas dentro del mismo archivo: `v-inicio`, `v-perfil`, `v-proyectos`, `v-trayectoria`, `v-contacto`.
  Se navega con `#inicio`, `#perfil`, `#proyectos`, `#trayectoria`, `#contacto` (no cambiar esos nombres).
- Dos pantallas más que se abren desde Perfil (no están en el menú; mientras se ven, el menú marca Perfil):
  `v-habilidades` (`#habilidades`) y `v-desarrollo` (`#desarrollo`), cada una con "← Volver a Perfil".
- El tema es "Noche": fondo `#1a1613`, texto `#e6dccd`. No usar negro puro ni blanco puro
  (se eligió así a propósito para no cansar la vista de noche).

## CUIDADO con el tamaño del archivo
`index.html` pesa unos 1.8 MB porque lleva las imágenes en base64, en cinco líneas gigantescas:
- `window.CRISAG_ASSETS`: el retrato de Inicio y su máscara.
- `window.CRISAG_RUNNER`: la foto runner y su máscara (para el retrato de Perfil).
- `window.CRISAG_DESARROLLO`: las fotos del maratón y del fútbol (pantalla Desarrollo personal).
- `window.CRISAG_HERRAMIENTAS`: los logos de herramientas de la pantalla Habilidades técnicas
  (lista `.tools-grid`; cada `<img data-tool="...">` toma su logo de este bloque).
- `window.CRISAG_TRAYECTORIA`: logos de las tarjetas de Trayectoria (Lubrizol, UNAM, preparatoria,
  Duolingo, Platzi, Google, MATLAB, Lean Six Sigma); cada `<img data-xp="...">` toma su logo de aquí.
- En el `<head>`: favicon (`<link rel="icon">`, 64 px transparente) e ícono de iPhone
  (`<link rel="apple-touch-icon">`, 180 px fondo crema), también en base64.
- No imprimas ni leas el archivo completo. Busca con `grep -n` y lee solo rangos de líneas.
- No reescribas el archivo completo: haz ediciones puntuales sobre texto único.
- Nunca modifiques ni muestres las cadenas base64.

## Dónde está cada cosa
- Contacto (correo, LinkedIn, GitHub): objeto `CONFIG` al inicio del `<script>` final.
- Textos de proyectos y trayectoria: dentro de `<div class="view" id="v-...">`.
- Proyectos: tarjetas `<article class="proj">` en filas de 3 (la última fila se centra sola). Cada tarjeta abre
  la ventana de detalle `#pm`. Para darle enlace a la imagen: `data-link="https://..."` (y opcional
  `data-link-label="Ver repositorio"`). Un segundo enlace (p. ej. el sitio) va en `data-link2` y
  `data-link2-label`. REGLA: los botones de enlace van siempre sobre la imagen (abajo a la izquierda),
  todos blancos/crema, nunca debajo de la descripción; un clic en el resto de la imagen abre el primero. Para mostrar código: `<template class="proj-code">…</template>` dentro
  del artículo; si no hay, el recuadro "Código" no aparece.
  Para agregar un proyecto nuevo usa el comando `/nuevo-proyecto` (`.claude/skills/nuevo-proyecto/SKILL.md`),
  que tiene los pasos. Cristian llena la plantilla de Word `.claude/plantillas/Plantilla_nuevo_proyecto.docx`.
  El código de los proyectos vive en sus repositorios de GitHub; las tarjetas enlazan a ellos.
- Trayectoria: cuatro carruseles de tarjetas (`.xp`), en este orden: Experiencia (`#xp`), Certificaciones
  (`#xp-cert`, cada tarjeta con su botón "Ver credencial"), Habilidades (`#xp-hab`) y Educación (`#xp-edu`).
  Un solo script mueve todos; al entrar a `#trayectoria` cada uno se centra en la tarjeta del medio
  (o en la que diga `data-start`).
- Colores del tema Noche: bloque `/* Tema Noche */` al final del `<style>`.
- Espacio de arriba en computadora (altura del menú y del contenido de todas las pantallas): bloque
  `@media (min-width:761px)` "Menos espacio arriba…", lo último antes de `</style>`. En celular no aplica.
- Retrato animado de Inicio: función `initPortrait()` (WebGL, canvas `#c`). No tocar el shader sin avisar.
- Retrato ingeniero / runner de Perfil: sección `.runner` al inicio de `v-perfil` (canvas `#c2`) y su
  propio `<script>` al final del archivo, después de `CRISAG_RUNNER`. Reutiliza `CRISAG_ASSETS.img`.
  Tampoco tocar su shader sin avisar. El runner va a la izquierda y el ingeniero a la derecha.
- Textos 3D de Perfil: botones `.runner-btn` ("Desarrollo Personal" a la izquierda → `#desarrollo`,
  "Habilidades Técnicas" a la derecha → `#habilidades`).
  Invitación al clic: cada 3 s los botones saltan (`.nudge`, animación `rbInvita`) y un trazo `.rb-guide`
  nace en los hombros, da una vuelta y apunta a cada botón (`drawGuide()` en el script del retrato de Perfil;
  el punto de inicio es la constante `NECK`). Se detiene al primer contacto y vuelve a empezar al entrar a Perfil.

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
