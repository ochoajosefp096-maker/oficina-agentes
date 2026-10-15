# Oficina de agentes: cómo montarla en otra cuenta de Claude

Este paquete trae todo lo necesario para tener la misma oficina en otra cuenta.

| Archivo | Para qué sirve |
|---|---|
| `oficina-agentes.html` | La oficina en 3D completa (agentes, rutinas, cadenas, Drive, imágenes). |
| `configuracion-oficina.json` | Tu configuración: los agentes con sus instrucciones (Guionista, Creador Protagónico, Prompts e imágenes, Creador Audiovisual, Estratega de contenido y Copy de anuncios), la descripción del negocio, el calendario con sus reels pendientes y aprobados, las últimas fichas, las fotos del producto y los ajustes. |
| `make-escenario-crear-imagen.json` | El escenario de Make que crea las imágenes con APIMart (con y sin hoja de referencia). |
| `GUIA.md` | Esta guía. |

El historial de tareas no viaja: la nueva oficina empieza con la lista de tareas vacía. Los resultados anteriores siguen en tu Google Drive.

---

## Paso 1. Conecta Google Drive y Make en la nueva cuenta de Claude

1. En la nueva cuenta, entra a **Configuración › Conectores**.
2. Conecta **Google Drive** con la cuenta de Google donde quieres guardar todo.
3. Conecta **Make** con tu cuenta de Make.

Los nombres de los conectores deben quedar como **Google Drive** y **Make**, que son los que usa la oficina.

## Paso 2. Publica la oficina

1. Abre un chat nuevo en la otra cuenta y adjunta el archivo `oficina-agentes.html`.
2. Escribe algo como:

> Publica este archivo como artifact sin cambiarlo. Declara estos permisos: sample con imágenes activadas (sample: {images: true}), db, user, assets, y mcp con el servidor "Google Drive" (herramientas search_files, create_file y update_file) y el servidor "Make" (herramientas scenarios_run, executions_get y executions_get-detail).

3. Abre la oficina publicada y acepta los permisos que te pida Claude (usar tu cuenta, enviar imágenes a Claude, guardar fotos, Google Drive y Make).

## Paso 3. Importa tu configuración

1. En la oficina, toca **Ajustes**.
2. En **Importar configuración**, elige el archivo `configuracion-oficina.json`.
3. Confirma. Los agentes quedan con sus nombres e instrucciones, se carga la descripción de tu negocio, el calendario recupera sus reels pendientes y aprobados, y las fotos del producto se suben solas a la nueva oficina. Espera a que diga "Configuración importada".
4. Toca **Guardar**.

## Paso 4. El escenario de Make para las imágenes

Hay dos casos.

**A. La nueva cuenta de Claude se conecta a la misma cuenta de Make.** No tienes que hacer nada: la oficina ya apunta al escenario **6611603**.

**B. Vas a usar otra cuenta de Make.** Hay que importar el escenario:

1. En Make, entra a **Scenarios › Create scenario**.
2. Abajo, en el menú de los tres puntos (`…`), elige **Import Blueprint** y sube `make-escenario-crear-imagen.json`.
3. Abre cada módulo de **Google Drive** (son 3: dos "Upload a File" y un "Get a File") y elige o crea la conexión con tu Google Drive. Al conectar, en la pantalla de Google **marca todas las casillas de permisos**.
4. Abre los **4 módulos HTTP** que llaman a APIMart y, en el header `Authorization`, reemplaza `TU_CLAVE_APIMART` por tu clave real. Debe quedar como `Bearer sk-...`.
5. En la programación del escenario elige **On demand**, guárdalo y actívalo.
6. Copia el número del escenario: es el que aparece en la dirección del navegador, por ejemplo `.../scenarios/1234567/edit`.
7. En la oficina, en **Ajustes › Imágenes con APIMart**, pega ese número en **ID del escenario de Make** y guarda.

### El modelo de imagen

El escenario usa **gpt-image-2-ext**. Si quieres otro modelo (por ejemplo `gemini-3-pro-image-preview`), cámbialo en los 2 módulos HTTP que hacen la petición POST a `/v1/images/generations`.

## Paso 5. Prueba rápida

1. Elige **Audiovisual** y el agente **Prompts e imágenes**.
2. Escribe: "Foto de una mujer mostrando unas uñas acrílicas que acaba de hacer, en su cuarto".
3. Asigna la tarea. Cuando termine, en 1 o 2 minutos debe aparecer la imagen, guardada en Drive en **Oficina de agentes › Audiovisuales y guiones › Prompts e imágenes › Imágenes**.

Si funciona, ya puedes usar el flujo completo:

1. **Marketing › Estratega de contenido**: llena la Ficha del producto (curso, qué enseña, fotos, público, días y reels por día) y asigna la tarea, sin agregar pasos.
2. Abre **Calendario** (botón de arriba), revisa cada reel y apruébalo o recházalo. Dentro de cada reel puedes marcar hasta dónde llega la cadena y el opcional **Copy de anuncios**.
3. Los aprobados se producen solos el día que toca: **Guionista de reels → Creador Protagónico → Prompts e imágenes → Creador Audiovisual**, y el copy del anuncio si lo marcaste.

---

## Cosas a tener en cuenta

- **Las rutinas y el calendario solo corren con la oficina abierta.** Si está cerrada a la hora programada, la tarea de ese día se hace apenas la abras, siempre que sea antes de la hora de fin.
- **Las carpetas de Drive se crean solas** la primera vez que cada agente guarda algo.
- **Seguridad de la clave de APIMart:** este paquete no incluye tu clave. Nunca la compartas en chats ni en pantallazos. Si alguna vez se expone, bórrala en APIMart, crea una nueva y reemplázala en los 4 módulos HTTP de Make.
- **Si el botón Asignar no responde**, mira el mensaje rojo que aparece encima: la oficina explica qué pasó. Si dice "Sin conexión a Claude", toca **Reconectar** y acepta el permiso.

---

## Investigador de productos (Marketing)

Busca infoproductos ganadores para lanzar. Usa tres escenarios de Make:

| Escenario | ID | Para qué |
|---|---|---|
| Pedir Apify (Oficina) | 6638459 | Arranca una búsqueda en Apify y guarda el ID de la corrida en Drive (`_investigaciones`) |
| Traer Apify (Oficina) | 6638480 | Revisa si la corrida terminó y guarda los resultados en Drive |
| Investigar con Perplexity | 6637964 | Opcional. Solo funciona cuando pongas tu API de Perplexity |

Necesita la conexión **Apify (Oficina)** en Make. Cada búsqueda lleva un tope de gasto en Apify (mínimo 0,50 USD por corrida; es un límite, no un cobro).

Qué hace una investigación:

1. Claude planea 20 búsquedas de Google y 6 búsquedas de anuncios.
2. Apify revisa hasta 1.000 anuncios activos en la Biblioteca de anuncios de Meta, busca en Google y lee las páginas más relevantes.
3. La oficina agrupa los anuncios por anunciante (días activos, cantidad de anuncios, cuáles llevan a WhatsApp).
4. Claude entrega el ranking de infoproductos con puntaje, evidencia, precio sugerido, hooks y plan de validación.

Costo aproximado: 1 a 1,20 USD de Apify y 50 a 80 operaciones de Make por investigación. Deja la oficina abierta mientras trabaja (5 a 20 minutos).

### Investigación en dos fases (actualización)

1. **Descubrir:** anuncios activos de Meta en el país principal y en los países marcados (por defecto México y Brasil), 20 búsquedas de Google y lectura de las mejores páginas.
2. **Validar** (para 8 a 10 candidatos): anuncios activos de cada candidato, total de anuncios de los anunciantes más fuertes, sus páginas de venta (precio y oferta), una búsqueda de Google por candidato, y videos de TikTok (`clockworks~tiktok-scraper`) y YouTube (`streamers~youtube-scraper`).

Costo aproximado: 1,80 a 2,50 USD de Apify y 120 a 180 operaciones de Make por investigación.
