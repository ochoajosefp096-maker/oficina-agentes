# Contexto para continuar en otra conversación

Resumen de todo lo construido para que otra sesión de Claude pueda seguir el trabajo. Este repositorio es público, así que **aquí no hay claves ni identificadores de cuentas**. Las claves (APIMart, Meta, Google) viven solo en Make y en los conectores de Claude. Los escenarios, archivos y carpetas se encuentran por su nombre con los conectores de Make y Google Drive.

## La oficina
- Página: `oficina/oficina-agentes.html` (una sola página con three.js r128).
- Está publicada como artifact privado en la cuenta del dueño. Para actualizarla, se publica este archivo sobre el mismo artifact (el dueño tiene el link), y así se conservan el link y las capacidades: sample, db, user, assets y mcp con Google Drive y Make.
- Guía para el dueño: `oficina/GUIA.md`.
- Datos guardados en la base del artifact (por usuario):
  - `oficina`: tareas, rutinas, ajustes y ventas. Límite de 256 KB por documento; el guardado recorta solo la copia, nunca los resultados en memoria.
  - `oficina-extra`: instrucciones del equipo, productos y descripción del negocio.
  - `calendario`: calendario de reels.

## Departamentos y agentes
- **Ventas:** Seguimiento de ventas (botón "Hacer seguimiento", automático a las 9, 12, 16 y 22, hora de Colombia) y Copywriter de ventas. Cerrador y Seguimiento de leads están ocultos.
- **Marketing:** Investigador de productos (Apify vía Make, en dos fases), Estratega de contenido (calendario) y Copy de anuncios.
- **Audiovisual:** Creador Protagónico, Guionista de reels, Creador Audiovisual y Prompts e imágenes (imágenes con APIMart vía Make).

## Reglas del dueño (ya aplicadas en la oficina)
- El último bloque del guion y el último clip llevan **siempre** un llamado a la acción para escribir al WhatsApp.
- Los guiones del calendario duran mínimo 60 segundos (150 a 170 palabras).
- Creador Audiovisual:
  - 3 tomas con la protagonista: gancho, **una sola** del desarrollo con la parte más importante y llamado a la acción.
  - Clips de 8 segundos con 16 a 20 palabras.
  - El resto del guion va como voz en off para las tomas de apoyo.
- Formato de los prompts de video: primero un resumen en español (cámara, acción, diálogo y voz) y después "PROMPT PARA PEGAR" en inglés por secciones (CAMERA, ACTION, DIALOGUE en español, VOICE, ACCENT y RULES con "Vertical 9:16 video"). El dueño los pega en Google Flow.
- Las ventas salen solo de la hoja "Tabla de Utilidad Cursos", que es de solo lectura. De Meta se toman solo el gasto y las métricas.
- En Ventas, preguntar antes de hacer cambios.

## Escenarios de Make (equipo del dueño)
- **Crear imagen APIMart:** imágenes con gpt-image-2-ext; guarda el PNG en Drive.
- **Investigar con Perplexity:** sin usar, porque Perplexity no está conectado.
- **Pedir Apify (Oficina)** y **Traer Apify (Oficina):** lanzan actores de Apify y traen los resultados a Drive.
- **Resumen de ventas (Oficina):** a las 9, 12, 16 y 22 trae el gasto de Meta (con desglose por sexo, edad, región y ubicación) y lee la hoja de ventas; lo guarda en `ventas-resumen.txt` en Drive.

## Google Drive
- Todo va en la carpeta "Oficina de agentes", con una carpeta por departamento y una subcarpeta por agente. Las investigaciones y el resumen de ventas van en `_investigaciones`.
- Nombres de archivos: `dd-mm-aa Apodo del reel - Rol`.

## Pendiente
- Copywriter de ventas: faltan los textos actuales del dueño (inicio, bono y apertura) y saber de dónde salen el bono, el link y el precio de cada curso.
- Video final: hacerlo en una sesión de Claude en el computador del dueño (app de escritorio, sesión local), usando el navegador integrado para manejar Google Flow.
- Prompts e imágenes sigue creando 3 imágenes; confirmar con el dueño si se usan las 3.
