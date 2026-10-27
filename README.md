# Rúbrica interactiva para informes de práctica Laboral

Aplicación web de un solo archivo (`index.html`) para evaluar y calificar informes de práctica Laboral de estudiantes de Diseño mediante una rúbrica por competencias con siete criterios y cinco niveles de desempeño.

**Uso en línea:** https://TU-USUARIO.github.io/NOMBRE-DEL-REPOSITORIO/

No requiere instalación, servidor, backend ni conexión a internet una vez cargada. Está hecha con HTML, CSS y JavaScript nativos, sin dependencias externas.

## Uso rápido

1. Abre la página (o el archivo `index.html` directamente en Chrome, Firefox, Safari o Edge).
2. Completa la **identificación**: tipo de práctica, estudiante o grupo, asignatura, semestre y año.
3. En la **matriz de evaluación**, haz clic en el nivel de desempeño que corresponda a cada criterio. Puedes agregar un comentario específico por criterio.
4. Revisa la nota en la barra superior, que permanece visible mientras evalúas.
5. Redacta la **retroalimentación general** (comentario, fortalezas, mejoras y próximos pasos).
6. Guarda o exporta la evaluación.

## Funcionalidades

- Cinco niveles visibles simultáneamente por criterio, con su descriptor completo y su nota.
- Color según la nota numérica del nivel: verde desde el umbral de aprobación, rojo por debajo. La intensidad aumenta con la distancia al umbral. La aprobación también se indica con texto e íconos, no solo con color.
- Cálculo automático de la nota final ponderada, con indicadores de evaluación completa o incompleta, criterios evaluados y suma de ponderaciones.
- **Modo edición de rúbrica:** permite modificar nombres y descripciones de criterios, nombres, descriptores y notas de los niveles, y las ponderaciones. También permite añadir o eliminar criterios y restaurar la rúbrica inicial.
- Escala configurable (por defecto 1,0 a 7,0, aprobación desde 4,0), con validación de rangos.
- Guardado local, exportación e importación JSON, exportación CSV, impresión o PDF y copia de un resumen al portapapeles.
- Tema oscuro por defecto, con opción de tema claro. Diseño adaptable a escritorio, tableta y teléfono, y uso completo con teclado.

## Cómo se calcula la nota

Para cada criterio:

```
aporte = nota del nivel seleccionado × ponderación / 100
nota final = suma de los aportes
```

Con la configuración inicial: `C1×0,15 + C2×0,20 + C3×0,20 + C4×0,25 + C5×0,10 + C6×0,05 + C7×0,05`.

Decisiones de diseño que conviene conocer:

- La nota se calcula sobre las **notas numéricas** de los niveles, nunca sobre sus etiquetas.
- Los cálculos usan enteros internamente para evitar errores de coma flotante. El redondeo ocurre solo al mostrar.
- Se muestra un decimal en la interfaz principal y dos decimales junto a ella. La **aprobación se decide con la nota sin redondear**: un 3,96 se muestra como «4,0» pero figura como reprobado. Si tu reglamento redondea antes de decidir, esta regla debe ajustarse.
- Si las ponderaciones no suman 100 %, la aplicación **advierte y no normaliza** el resultado.
- Con criterios sin evaluar, la nota se marca como **provisional** y se calcula como promedio ponderado solo de los criterios evaluados.

## Datos, privacidad y almacenamiento

- Todo ocurre en el navegador. **La aplicación no envía datos a ningún servidor**, ni siquiera cuando está alojada en GitHub Pages.
- «Guardar» usa `localStorage`: guarda **una sola evaluación** por navegador y dispositivo. No se sincroniza entre equipos. Algunos navegadores bloquean este almacenamiento en ciertos contextos; en ese caso usa la exportación JSON.
- Para trabajar con varios estudiantes, exporta un JSON por cada uno.

## Formato del archivo JSON

La exportación contiene la evaluación y la rúbrica completa, y puede volver a importarse. La importación valida la estructura antes de aplicarla.

```json
{
  "version": 1,
  "exportado": "2026-10-27T20:00:00.000Z",
  "meta": { "tipo": "", "nombre": "", "asig": "", "sem": "Otoño", "anio": 2026 },
  "scale": { "min": 1, "max": 7, "pass": 4 },
  "fb": { "general": "", "fort": "", "mejora": "", "next": "" },
  "criteria": [
    {
      "id": 1,
      "name": "Nombre del criterio",
      "desc": "Descripción breve",
      "w": 15,
      "sel": 0,
      "comment": "",
      "levels": [
        { "name": "Excelente", "desc": "Descriptor…", "grade": 7 }
      ]
    }
  ]
}
```

- `sel` es la posición del nivel seleccionado (0 es el primer nivel, normalmente el más alto) o `null` si no hay selección.
- `w` es la ponderación en porcentaje.

## Formato del CSV

Separador `;`, codificación UTF-8 con BOM (se abre correctamente en Excel con tildes y eñes) y decimales con coma. Hay una fila por criterio, con estas columnas:

`Tipo`, `Estudiante/grupo`, `Asignatura`, `Semestre`, `Año`, `Criterio`, `Descripción del criterio`, `Ponderación (%)`, `Nivel seleccionado`, `Nota obtenida`, `Aporte ponderado`, `Comentario del criterio`, `Nota final`, `Estado de la nota`, `Aprobación`, `Retroalimentación general`, `Fortalezas`, `Mejoras`, `Próximos pasos`.

Los datos de identificación, la nota final y la retroalimentación se repiten en cada fila para facilitar el análisis en hojas de cálculo.

## Impresión y PDF

El botón **Imprimir / PDF** genera una hoja ordenada con la identificación, cada criterio con el nivel y descriptor seleccionados, las notas, ponderaciones, comentarios, la nota final y la retroalimentación. Para obtener un PDF, elige «Guardar como PDF» en el diálogo de impresión del navegador.

## Publicación en GitHub Pages

1. Sube `index.html` a la raíz del repositorio.
2. En **Settings → Pages**, elige **Deploy from a branch**, rama `main` y carpeta `/ (root)`.
3. Tras uno o dos minutos, la página estará disponible en la URL de Pages del repositorio.

Los repositorios privados requieren un plan de pago de GitHub para usar Pages.

## Versionado de la rúbrica

Los criterios, ponderaciones y descriptores son una decisión académica que cambia con el tiempo. Se recomienda:

- Guardar cada versión aplicada como un archivo JSON (por ejemplo `rubricas/2026-otono.json`) y registrar los cambios con commits descriptivos.
- Anotar en el commit qué se modificó y por qué (por ejemplo, un cambio de ponderación).
- Conservar los JSON de evaluaciones fuera del repositorio público: contienen nombres y comentarios sobre estudiantes.

## Limitaciones conocidas

- Se guarda una sola evaluación en el almacenamiento local por navegador.
- Los cambios hechos en el modo edición se guardan junto con la evaluación, pero no existe un historial de versiones dentro de la aplicación.
- No hay análisis conjunto de varias evaluaciones (comparación de cohortes); eso requiere procesar los CSV o JSON en otra herramienta.
- La aplicación se desarrolló sin pruebas automatizadas. Antes de usarla en una evaluación real, haz una evaluación de prueba y verifica el guardado, la exportación y la impresión en tu navegador.

## Licencia

Define aquí la licencia del proyecto (por ejemplo MIT, o Creative Commons para el contenido de la rúbrica).
