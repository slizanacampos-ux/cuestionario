# Cuestionario probabilístico con Google Sheets

Este paquete contiene una versión interactiva del cuestionario **Pensamiento probabilístico orientado a la toma de decisiones y conocimiento didáctico para la enseñanza de la probabilidad**.

## Cambios incorporados

- El instrumento está organizado en **dos bloques**: Bloque A con 8 situaciones de decisión y Bloque B con 4 escenarios de análisis didáctico.
- La base de datos es un **Google Sheet privado**.
- Se solicitan **nombre completo, universidad y año de ingreso a la carrera**.
- Se eliminaron las imágenes de las situaciones 1, 2, 3, 4, 6, 7 y 8.
- La situación 5 conserva el archivo PNG original extraído del documento, sin redibujarlo ni editarlo.
- En el Bloque B, la situación propuesta y la respuesta del/de la estudiante aparecen en recuadros diferenciados.
- Las respuestas se guardan progresivamente y el envío final se verifica contra Google Sheets.
- El progreso también queda temporalmente en el navegador para poder retomarlo.

## Archivos

- `index.html`: cuestionario para participantes.
- `admin.html`: acceso seguro al Google Sheet y comprobación de la conexión.
- `config.js`: URLs y versión del instrumento.
- `Code.gs`: backend que debe instalarse en Google Apps Script.
- `assets/situacion5_ruta_taxi.png`: imagen original de la situación 5.

---

## 1. Crear la base de datos en Google Sheets

1. Ingrese a Google Drive y cree una hoja de cálculo nueva.
2. Asigne un nombre, por ejemplo: `Respuestas cuestionario probabilístico`.
3. En la hoja, abra **Extensiones > Apps Script**.
4. Elimine el contenido inicial del archivo `Code.gs`.
5. Copie y pegue todo el contenido del archivo `Code.gs` incluido en este paquete.
6. Guarde el proyecto.
7. En la lista de funciones, seleccione `setupDatabase` y pulse **Ejecutar**.
8. Autorice el acceso solicitado por Google.

La función creará dos pestañas:

- `Respuestas`: una fila por participante.
- `Diccionario`: descripción de las columnas.

## 2. Publicar Google Apps Script como aplicación web

1. En Apps Script, pulse **Implementar > Nueva implementación**.
2. En **Tipo de implementación**, elija **Aplicación web**.
3. Configure:
   - **Ejecutar como:** Usted.
   - **Quién tiene acceso:** Cualquier persona.
4. Pulse **Implementar**.
5. Copie la URL que termina en `/exec`.

Cada vez que modifique `Code.gs`, cree una versión nueva de la implementación o edite la implementación existente para que los cambios se publiquen.

## 3. Configurar el cuestionario

Abra `config.js` y complete:

```javascript
window.APP_CONFIG = {
  GOOGLE_APPS_SCRIPT_URL: "https://script.google.com/macros/s/XXXXXXXXXXXX/exec",
  GOOGLE_SHEET_URL: "https://docs.google.com/spreadsheets/d/XXXXXXXXXXXX/edit",
  INSTRUMENT_VERSION: "3.0",
  STUDY_TITLE: "Cuestionario sobre pensamiento probabilístico orientado a la toma de decisiones y conocimiento didáctico para la enseñanza de la probabilidad",
  ADMIN_CONTACT: ""
};
```

- `GOOGLE_APPS_SCRIPT_URL`: URL `/exec` obtenida al desplegar Apps Script.
- `GOOGLE_SHEET_URL`: dirección privada de la hoja de cálculo.
- No coloque contraseñas ni claves privadas dentro de `config.js`.

## 4. Probar antes de publicar

Abra una terminal en la carpeta del proyecto y ejecute:

```bash
python3 -m http.server 8000
```

Luego visite:

- `http://localhost:8000/index.html`
- `http://localhost:8000/admin.html`

Prueba mínima recomendada:

1. Complete una aplicación con datos ficticios.
2. Avance por varias situaciones o escenarios y confirme que la fila se actualiza en la pestaña `Respuestas`.
3. Envíe el cuestionario.
4. Compruebe que `estado` cambia de `en_curso` a `completado`.
5. Verifique que las respuestas del Bloque A y del Bloque B estén registradas.

## 5. Publicar en GitHub Pages

1. Cree o abra el repositorio de GitHub donde publicará el cuestionario.
2. Suba estos archivos manteniendo la carpeta `assets`:

```text
index.html
admin.html
config.js
Code.gs
README.md
assets/
  situacion5_ruta_taxi.png
```

3. Abra **Settings > Pages**.
4. En **Build and deployment**, seleccione **Deploy from a branch**.
5. Seleccione la rama `main` y la carpeta `/ (root)`.
6. Guarde y espere a que GitHub publique el sitio.

## 6. Estructura de la base de datos

La pestaña `Respuestas` utiliza una sola fila por participante. Incluye:

- identificador técnico de la sesión;
- nombre completo;
- universidad;
- año de ingreso;
- consentimiento;
- versión del instrumento;
- estado de la aplicación;
- fechas de inicio, actualización y finalización;
- duración total;
- Bloque A: alternativa, otra respuesta, justificación y tiempo de cada situación;
- Bloque B: interpretación, acción didáctica y tiempo de cada escenario.

El guardado progresivo actualiza siempre la misma fila. El envío final vuelve a escribir todas las respuestas y luego cambia el estado a `completado`.

## 7. Acceso y protección de datos

El Google Sheet contiene datos identificatorios. Por ello:

- mantenga la hoja configurada como **Restringida**;
- compártala solamente con el equipo autorizado;
- no use **Archivo > Compartir > Publicar en la web**;
- no publique exportaciones con nombres o afiliaciones;
- ajuste el consentimiento a la versión aprobada por su comité de ética;
- realice copias de respaldo periódicas en formato Excel o CSV y almacénelas de forma segura.

`admin.html` no descarga ni muestra las respuestas. Solo comprueba que Apps Script esté activo y abre el Google Sheet privado en una pestaña nueva. Esto evita exponer datos personales desde una página pública.
