---
name: nuevo-proyecto
description: Agrega un proyecto nuevo a la sección Proyectos de crisag.com a partir del formulario de Cristian (título, etiqueta, descripción, herramientas, enlace, imagen y código). Úsalo cuando Cristian pida subir, agregar o publicar un proyecto en su sitio, o cuando pegue el formulario "NUEVO PROYECTO".
---

# Agregar un proyecto a crisag.com

Cristian pega el formulario de abajo (lleno). Tú lo conviertes en una tarjeta de Proyectos con su ventana
de detalle, lo pruebas en local y, solo cuando él lo apruebe, haces lo de GitHub. Sigue las reglas de
`CLAUDE.md` (rama nueva, mostrar cada comando de Git y esperar su sí, nunca tocar las cadenas base64).

## Si manda la plantilla de Word (lo normal)

La plantilla vacía está en `.claude/plantillas/Plantilla_nuevo_proyecto.docx`; si la pide, mándasela.
Cuando mande una llena (`.docx`):

1. Cópiala a una carpeta nueva y vacía del scratchpad (no la abras dentro del repositorio).
2. Lee el texto: `textutil -convert txt -stdout plantilla.docx`. Cada campo aparece como su etiqueta
   (Título, Etiqueta, Descripción, …), una línea de ayuda gris y debajo lo que él escribió.
   Ignora los textos de ejemplo que no haya borrado ("Ej. …", "Escribe aquí…", "https://…", "# pega aquí…").
3. Si pegó una imagen, sácala con `unzip -o plantilla.docx 'word/media/*' -d <carpeta>`
   (es la imagen del campo Imagen). Si no hay imagen y escribió "ilustración", dibújala.
4. Muéstrale en una lista corta lo que entendiste de cada campo y sigue con los pasos de abajo.

## Si lo manda como texto

Si Cristian pide agregar un proyecto pero no manda la plantilla ni el formulario, mándale la plantilla de
Word o muéstrale este formulario de texto para que lo llene:

```
NUEVO PROYECTO
Título: 
Etiqueta (año o lugar, ej. "[2026]" o "Lubrizol"): 
Descripción (2 a 4 líneas): 
Herramientas (separadas por coma, 2 a 5): 
Enlace (repositorio, sitio o demo; o "ninguno"): 
Texto del botón del enlace (ej. "Ver repositorio", "Ver sitio web"; opcional): 
Imagen (adjunta una captura, o escribe "ilustración" para que Claude la dibuje): 
Posición (ej. "al inicio", "al final", "después de Data Fire"; por defecto al inicio): 
Código (pega 10 a 30 líneas, o "ninguno"):
```

Campos obligatorios: Título, Etiqueta, Descripción, Herramientas. Si falta alguno, pregúntalo antes de
seguir; no lo inventes. Tampoco inventes código ni enlaces: si dice "ninguno", se omiten.

## Pasos

1. **Rama nueva** desde `main` actualizado: `proyecto-<titulo-en-minusculas-con-guiones>`.
2. **Ubica la sección**: `grep -n 'class="proj"' index.html`. Cada proyecto es un
   `<article class="proj">` dentro de `#v-proyectos .panel`. Respeta la posición pedida.
3. **Arma el artículo** copiando la estructura de los existentes:
   ```html
   <article class="proj" data-link="URL" data-link-label="TEXTO DEL BOTÓN">
     <div class="vis">IMAGEN</div>
     <div class="info">
       <div>
         <div class="top"><h3 class="disp">TÍTULO</h3><span class="yr">ETIQUETA</span></div>
         <p>DESCRIPCIÓN</p>
       </div>
       <ul class="tags"><li>HERRAMIENTA</li>…</ul>
     </div>
     <template class="proj-code">CÓDIGO</template>
   </article>
   ```
   - Sin enlace: quita `data-link` y `data-link-label`. Sin código: quita el `<template>` (el recuadro
     "Código" se oculta solo).
   - **Código**: escapa `&` → `&amp;`, `<` → `&lt;`, `>` → `&gt;` antes de ponerlo en el `<template>`.
     Respeta la sangría tal cual.
   - **Imagen**:
     - "ilustración": dibuja un `<svg viewBox="0 0 430 270">` con la paleta del sitio (fondo `#17120d`,
       crema `#efe6d8`, café `#a88b6b`, verde `#7b8f6a`), en el mismo estilo plano de las otras tarjetas.
     - Captura adjunta: redúcela a ~900 px de ancho en JPEG (calidad ~70, que pese menos de 80 KB) e
       incrústala como `<img src="data:image/jpeg;base64,…" alt="…" style="width:100%;height:100%;object-fit:cover">`.
       Avisa el peso nuevo de `index.html` (límite recomendado: 3 MB).
4. **Revisa en local** (`python3 -m http.server 8791 --bind 127.0.0.1`, abre `#proyectos`):
   - la tarjeta aparece en su lugar y las filas siguen de 3 (la última centrada);
   - la ventana abre con imagen, título, descripción, herramientas y código; el enlace abre en otra pestaña;
   - computadora y celular (375 px), sin desbordamiento horizontal ni errores en la consola.
   Manda capturas a Cristian.
5. **Cuando apruebe**: muestra los comandos (`git add index.html`, commit en español con la línea
   Co-Authored-By, `git push -u origin <rama>`, `gh pr create` con `/opt/homebrew/bin/gh`) y espera su sí.
   Después del merge, comprueba crisag.com y propón la siguiente etiqueta de versión.
