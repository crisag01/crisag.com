---
name: nuevo-proyecto
description: Agrega un proyecto nuevo a la sección Proyectos de crisag.com a partir de la plantilla de Word o el formulario de Cristian (título, etiqueta, descripción, herramientas, repositorio de GitHub, sitio o demo, posición e imagen). Úsalo cuando Cristian pida subir, agregar o publicar un proyecto en su sitio, o cuando mande la plantilla "NUEVO PROYECTO".
---

# Agregar un proyecto a crisag.com

Cristian manda la plantilla de Word (o el formulario de texto) llena. Tú la conviertes en una tarjeta de
Proyectos con su ventana de detalle, la pruebas en local y, solo cuando él la apruebe, haces lo de GitHub.
Sigue las reglas de `CLAUDE.md` (rama nueva, mostrar cada comando de Git y esperar su sí, nunca tocar las
cadenas base64).

El código de cada proyecto vive en su **repositorio de GitHub**; la tarjeta solo enlaza a él. Ya no se
pide ni se copia código al sitio.

## Si manda la plantilla de Word (lo normal)

La plantilla vacía está en `.claude/plantillas/Plantilla_nuevo_proyecto.docx`; si la pide, mándasela.
Cuando mande una llena (`.docx`):

1. Cópiala a una carpeta nueva y vacía del scratchpad (no la abras dentro del repositorio).
2. Lee el texto: `textutil -convert txt -stdout plantilla.docx`. Cada campo aparece como su etiqueta
   (Título, Etiqueta, Descripción, Herramientas, Contexto, Mi rol, Resultado, Repositorio en GitHub, Sitio o demo,
   Posición, Imagen),
   una línea de ayuda y debajo lo que él escribió. Ignora los textos de ejemplo que no haya borrado
   ("Ej. …", "Escribe aquí…", "https://github.com/crisag01/…", "https://…", "Pega aquí la imagen…").
3. Si pegó una imagen, sácala con `unzip -o plantilla.docx 'word/media/*' -d <carpeta>`
   (es la imagen del campo Imagen). Si no hay imagen y escribió "ilustración", dibújala.
4. Muéstrale en una lista corta lo que entendiste de cada campo y sigue con los pasos de abajo.

## Si lo manda como texto

Si Cristian pide agregar un proyecto pero no manda nada, mándale la plantilla de Word o muéstrale este
formulario de texto para que lo llene:

```
NUEVO PROYECTO
Título: 
Etiqueta (año o lugar, ej. "[2026]" o "Lubrizol"): 
Descripción (2 a 4 líneas): 
Herramientas (separadas por coma, 2 a 5): 
Contexto (opcional, dónde o para quién lo hiciste): 
Mi rol (opcional, qué hiciste tú): 
Resultado (opcional, una cifra o logro concreto): 
Repositorio en GitHub (o "ninguno"): 
Sitio o demo (opcional): 
Posición (ej. "al inicio", "al final", "después de Data Fire"; por defecto al inicio): 
Imagen (adjunta una captura, o escribe "ilustración" para que Claude la dibuje): 
```

Campos obligatorios: Título, Etiqueta, Descripción, Herramientas. Si falta alguno, pregúntalo antes de
seguir; no lo inventes. Tampoco inventes enlaces: si dice "ninguno" o lo deja vacío, se omite.
Contexto, Mi rol y Resultado son opcionales: si los deja vacíos (o con el "Ej. …"), se omiten; no los inventes.
Si el repositorio parece de un proyecto de empresa (por ejemplo Lubrizol), recuérdale no publicar datos
privados.

## Pasos

1. **Rama nueva** desde `main` actualizado: `proyecto-<titulo-en-minusculas-con-guiones>`.
2. **Ubica la sección**: `grep -n 'class="proj"' index.html`. Cada proyecto es un
   `<article class="proj">` dentro de `#v-proyectos .panel`. Respeta la posición pedida.
3. **Arma el artículo** copiando la estructura de los existentes:
   ```html
   <article class="proj" data-link="URL" data-link-label="TEXTO DEL BOTÓN"
            data-contexto="CONTEXTO" data-rol="MI ROL" data-resultado="RESULTADO">
     <div class="vis">IMAGEN</div>
     <div class="info">
       <div>
         <div class="top"><h3 class="disp">TÍTULO</h3><span class="yr">ETIQUETA</span></div>
         <p>DESCRIPCIÓN</p>
       </div>
       <ul class="tags"><li>HERRAMIENTA</li>…</ul>
     </div>
   </article>
   ```
   - **Enlace de la imagen** (`data-link`): el repositorio de GitHub, con `data-link-label="Ver repositorio"`.
     Si no tiene repositorio pero sí sitio o demo, usa el sitio con `data-link-label="Ver sitio web"`.
     Sin ninguno de los dos, quita `data-link` y `data-link-label`.
   - **Si tiene repositorio Y sitio**: repositorio en `data-link` (`data-link-label="Ver repositorio"`) y
     sitio en `data-link2` (`data-link2-label="Ver sitio web"`). Así está Data Fire.
   - **Ficha del detalle** (`data-contexto`, `data-rol`, `data-resultado`): la ventana de detalle los muestra
     como renglones bajo la descripción. Pon solo los que él llenó; si falta uno, quita ese atributo.
   - **Regla de los botones**: la ventana los dibuja sola. Van siempre sobre la imagen, abajo a la
     izquierda, ambos blancos/crema; nunca debajo de la descripción. Un clic en el resto de la imagen
     abre el primer enlace. No agregues botones a mano.
   - **Imagen**:
     - "ilustración": dibuja un `<svg viewBox="0 0 430 270">` con la paleta del sitio (fondo `#17120d`,
       crema `#efe6d8`, café `#a88b6b`, verde `#7b8f6a`), en el mismo estilo plano de las otras tarjetas.
     - Captura adjunta: redúcela a ~900 px de ancho en JPEG (calidad ~70, que pese menos de 80 KB) e
       incrústala como `<img src="data:image/jpeg;base64,…" alt="…" style="width:100%;height:100%;object-fit:cover">`.
       Avisa el peso nuevo de `index.html` (límite recomendado: 3 MB).
   - **Proyecto reciente de Inicio (siempre)**: cada proyecto nuevo pasa a ser el "Proyecto reciente" de
     Inicio (`<div class="abs out rise">` en `v-inicio`). Cambia el `<h2>` por el título, el `aria-label`
     del `<a class="card">` y su imagen: con ilustración,
     `<svg class="vis" viewBox="0 0 430 270" preserveAspectRatio="xMidYMid slice" aria-hidden="true"><use href="#v-…" width="430" height="270"/></svg>`;
     con logo o imagen sobre fondo claro, `<a class="card card-claro">` y un `<div class="vis">` con el `<img>`.
4. **Revisa en local** (`python3 -m http.server 8791 --bind 127.0.0.1`, abre `#proyectos` y `#inicio`):
   - en Inicio, "Proyecto reciente" muestra el proyecto nuevo;
   - la tarjeta aparece en su lugar y las filas siguen de 3 (la última centrada);
   - la ventana abre con imagen, título, descripción y herramientas; el enlace abre en otra pestaña;
   - computadora y celular (375 px), sin desbordamiento horizontal ni errores en la consola.
   Manda capturas a Cristian.
5. **Cuando apruebe**: muestra los comandos (`git add index.html`, commit en español con la línea
   Co-Authored-By, `git push -u origin <rama>`, `gh pr create` con `/opt/homebrew/bin/gh`) y espera su sí.
   Después del merge, comprueba crisag.com y propón la siguiente etiqueta de versión.
