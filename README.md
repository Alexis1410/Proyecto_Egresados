# Proyecto_Egresados
Este repositorio incluye los codigos para egresados de la base TecNM.
Manual de usuario que se ira actualizado acorde a lo que este se necesite.
Instructivo — Automatización de “Base Maestra” y Resumen de Egresados (2022–2025).
Autor: A-E(con soporte de Office Scripts).
1. Objetivo
Centralizar información de egresados (2022–2025) y futuras actualizaciones a 2026 y 2027 en una Base Maestra y generar un Resumen con indicadores y gráficas por Carrera, Sexo y Periodo de egreso (Ene–Jun, Ago–Dic) con estética clara.
2. Requisitos previos
•	Excel en la web con Office Scripts habilitado.
•	Un solo libro con las hojas de origen (por ejemplo: “egresados enero-junio 2023”, “egresado agosto-dic 2023”, etc.).
•	(Opcional) Hoja “Respondieron” con una columna que contenga los correos que contestaron.
•	Los nombres de columnas en origen pueden variar; los scripts detectan encabezados y sinónimos.
3. Flujo de trabajo
•	Ejecutar los scripts por año para normalizar cada periodo en su hoja “Base YYYY”.
•	Ejecutar el Unificador “Base Maestra” para juntar todas las “Base YYYY” y marcar “Respondió”.
•	Ejecutar “Resumen + Gráficas” para obtener cuadros y gráficas finales en la hoja “Resumen”.
4. Estructura esperada del libro
•	Origen (ejemplos): “egresados enero-junio 2022”, “egresados agosto-dic-2022”, “egresados ene-jun 2025”.
•	(Opcional) “Respondieron”: una lista de correos en cualquier columna.
•	Salidas que generan los scripts: Base 2022, Base 2023, Base 2024, Base 2025 Ene-Jun, Base Maestra, Resumen.
5. Scripts por año (crear Base YYYY)
Cada script construye una hoja “Base YYYY” con las columnas estándar: Nombre, Sexo, Edad, Periodos, Correos, Celular, Carrera, Modalidad, Respondió, Origen. Ajusta los nombres de hojas de origen en la constante SHEETS según el año.
5.1. Base 2022
Pegue en Automatizar → Nuevo script.
// Ajusta los nombres de hojas si difieren
const SHEETS: string[] = ["egresados enero-junio-2022","egresados agosto-dic-2022"];
const OUT = "Base 2022";
const RESP_SHEET = "Respondieron";
const RESP_COL_LETTER = "B";
// ... (resto del script tal como lo tienes)

5.2. Base 2023
const SHEETS = ["egresados enero-junio 2023","egresado agosto-dic 2023"];
const OUT = "Base 2023";
// ... (resto del script tal como lo tienes)

5.3. Base 2024
const SHEETS = ["egresados enero-junio 2024","egresados agosto-dic 2024"];
const OUT = "Base 2024";
// ... (resto del script tal como lo tienes)

5.4. Base 2025 (Ene–Jun)
const SHEETS = ["egresados ene-jun 2025"];
const OUT = "Base 2025 Ene-Jun";
// ... (resto del script tal como lo tienes)

6. Unificador: construir “Base Maestra”
Une Base 2022, Base 2023, Base 2024 y Base 2025 Ene-Jun en una sola hoja “Base Maestra”. Detecta Núm. de control y marca “Respondió” comparando correos con la hoja “Respondieron”.
// Fuentes que espera el unificador (ajusta si cambiaste nombres)
const sources = ["Base 2022","Base 2023","Base 2024","Base 2025 Ene-Jun"];
// ... (resto del script tal como lo tienes)

7. Resumen y gráficas (Carrera, Sexo, Periodo)
Genera la hoja “Resumen” con KPIs, tablas y tres gráficas (por carrera, por sexo y por periodo).
const DATA="Base Maestra"; const OUT="Resumen";
// ... (resto del script tal como lo tienes, para KPIs y gráficos)

8. Cómo ejecutar los scripts
•	Abra Excel en la web → pestaña “Automatizar” → “Nuevo script”.
•	Pegue el código del script por año (2022, luego 2023, etc.) y ejecútelo. Verifique que aparecen las hojas “Base YYYY”.
•	Pegue y ejecute el unificador “Base Maestra”. Verifique columnas Nombre, NumControl, Carrera, Periodo, Correos y “Respondió”.
•	Pegue y ejecute el script de “Resumen y gráficas”. Revise KPIs, tablas y gráficas.
9. Personalización rápida
•	Nombres de hojas de origen: modifique la constante SHEETS en los scripts de Base.
•	Detección de columnas: si un encabezado cambia, añada sinónimos al arreglo de búsqueda (find...).
•	“Respondieron”: coloque correos en la hoja (en mayúsculas o minúsculas; el unificador los estandariza).
•	Gráficas: cambie tipo y ubicación ajustando las llamadas a addChart.
10. Solución de problemas
•	“Aliasing or assignment of Office Scripts APIs”: evite guardar métodos de la API en variables; llámelos directamente.
•	“Explicit any is not allowed”: tipifique arreglos como (string|number|boolean)[].
•	Rendimiento: no lea celdas dentro de bucles; use getUsedRange().getValues() y procese en memoria.
•	El Núm. de control no aparece: asegúrese de que alguna columna incluya “control”, “matr”, “núm”; si no, el unificador lo infiere por patrón.

Presentacion
FLUJO GENERAL DEL SISTEMA DE SEGUIMIENTO A EGRESADOS
1️ Generar el cuestionario
•	Se diseña el instrumento de recolección de datos con base en los objetivos educacionales y atributos de egreso.
•	Se define la estructura: datos personales, académicos, laborales, y percepciones del egresado.

2️ Crear el formulario en Google Forms
•	Se monta el cuestionario en Google Forms para tener una plataforma accesible.
•	Se activa la recopilación de correos electrónicos (para vincular con la base).
•	Se vincula a una Hoja de cálculo (Google Sheets) donde se almacenan las respuestas en tiempo real.

3️ Calcular el tamaño de muestra
•	Se usa la fórmula de muestreo poblacional o censal, dependiendo del total de egresados.
•	Se determina cuántos egresados deben responder por carrera o por generación para tener validez estadística.

4️ Organizar y actualizar la base de datos de egresados
•	Se consolida la información en una hoja llamada BaseEgresados con columnas:
o	Nombre completo, correo institucional/personal, CURP, generación, carrera, periodo (Ene-Jun / Ago-Dic).
•	Se revisa y limpia (sin duplicados, sin correos inválidos, sin filas vacías).

5️ Enviar el correo con el link del formulario
•	Se personaliza un correo institucional invitando a contestar el cuestionario.
•	Se envía a toda la base de egresados.
•	Se puede hacer desde Gmail o con el script recordatorios.gs (que detecta automáticamente quién no ha respondido).

6️ Recepción de respuestas
•	El formulario se llena por los egresados y cada envío se guarda automáticamente en la hoja “Respuestas de formulario 1”.
•	Aquí ya empieza a funcionar el sistema de validaciones automáticas.

7️ Procesamiento y validación de datos (ya con tu sistema)
•	Aquí comienza la parte automatizada con Apps Script.
1.	Ejecutar globalRun() (desde formatoRespuestas.gs.gs).
o	Genera o actualiza la hoja Base Maestra.
o	Aplica validaciones automáticas (CURP, teléfono, duplicados).
o	Calcula si el egresado respondió o no respondió.
o	Clasifica por periodo, año y carrera.
2.	Se crean reportes automáticos:
o	Resumen → Vista general con totales y porcentajes globales.
o	ResDet_... → Vista detallada por carrera y por semestre.
o	Ambas hojas incluyen gráficas automáticas (columnas, pastel y comparativas por semestre).
3.	Si hay egresados que no han respondido:
o	Correr enviarRecordatorios() → envía correos automáticos solo a esos contactos, invitándolos a contestar.

8️ Análisis y presentación de resultados
1.Se pueden mostrar:
o	Porcentajes de respuesta por carrera.
o	Comparación entre generaciones o periodos.
o	Gráficas globales (respondieron vs no respondieron).
o	Distribución por semestre.
2.	Esto puede ser presentado al Consejo Académico o Jefaturas de Carrera para:
o	Monitorear el avance de respuesta.
o	Identificar carreras con menor participación.
o	Planear estrategias de seguimiento o entrevistas.

9️ Opcional — Automatización continua
•	Se puede programar que globalRun() se ejecute automáticamente cada semana.
•	También programar el envío de recordatorios cada 3 o 5 días hasta alcanzar el porcentaje deseado de respuestas.

10️ Cierre y respaldo
•	Exportar la hoja resumen y base maestra a PDF o Excel para el archivo institucional.
•	Respaldar en Drive una carpeta con:
o	Cuestionario,
o	Base maestra,
o	Resumen general,
o	Resumen detallado,
o	Evidencia de correos enviados.
------------------------------------------------------------------------------------------

Parte 2 en exel
Guía de Presentación del Sistema de Seguimiento de Egresados
Objetivo general
Explicar de forma clara y práctica cómo funciona el Sistema de Seguimiento de Egresados, desde la creación del cuestionario hasta la obtención de resultados consolidados y visuales, aun para personas que no tienen conocimiento previo del sistema.
Flujo general del proceso
1️ Generar el cuestionario
Propósito: obtener datos actualizados sobre los egresados (contacto, empleo, opinión, seguimiento).
Preguntas clave:
¿Qué información se necesita del egresado?
¿Qué preguntas son obligatorias?
¿Cómo se garantizará la confidencialidad de los datos?
Herramienta sugerida: Google Forms (fácil, gratuito y con exportación a Sheets).

2️ Subirlo a Google Forms
Propósito: alojar el cuestionario en línea para facilitar la recolección de respuestas.
Preguntas guía:
¿Quién administrará el formulario?
¿Se limitará a 1 respuesta por correo institucional?
¿Qué mensaje se mostrará al enviar el formulario?
Recomendación: activa validación de correo y configura mensaje de confirmación personalizado.

3️ Calcular el tamaño de muestra
Propósito: determinar cuántas respuestas se requieren para tener resultados representativos.
Preguntas guía:
¿Cuál es el número total de egresados (N)?
¿Qué nivel de confianza y margen de error usaré?
¿Se necesita un cálculo por carrera o global?
Fórmula base:
	​(Donde p=0.5, Z≈1 para confianza 80%, e=0.1 = 10% de error.)

4️ Ordenar y actualizar la base de datos de egresados
Propósito: preparar la información de contacto antes del envío del cuestionario.
Preguntas guía:
¿Están actualizados los correos personales e institucionales?
¿Cada egresado tiene asignada su carrera y periodo de egreso?
¿Faltan registros duplicados o vacíos?
Acción: usar los scripts del sistema (Base 2022, Base 2023, etc.) para normalizar y limpiar la información.

5️ Enviar el link del cuestionario
Propósito: distribuir el cuestionario a todos los egresados de forma eficiente.
Preguntas guía:
¿Qué canal se usará para el envío (correo, WhatsApp, redes sociales)?
¿Habrá un mensaje formal de invitación y recordatorio?
¿Se establecerá una fecha límite de respuesta?
Sugerencia: personaliza el mensaje por carrera o generación para aumentar la tasa de respuesta.

6️ Recibir respuestas
Propósito: recopilar la información enviada desde el Google Form.
Preguntas guía:
¿Cada cuánto se revisarán las respuestas?
¿Quién será responsable de la actualización?
¿Dónde se almacenarán los datos recibidos?
Acción: exportar las respuestas del formulario a una hoja de cálculo y mantener respaldo semanal.

7️.-¿Qué sigue después de recibir las respuestas? 
Una vez cerrada la etapa de recepción, se continúa con la automatización en Excel (Office Scripts):

🔹 Paso 7.1 — Actualizar la hoja Respondieron
Pegar los correos de quienes completaron el formulario (en minúsculas).
Se puede usar una columna con 1 = “Respondió”, 0 = “No respondió”.

🔹 Paso 7.2 — Ejecutar los scripts por año
Correr los scripts: Base 2022, Base 2023, Base 2024, Base 2025 Ene-Jun, etc.
Cada script limpia y unifica los datos por generación.

🔹 Paso 7.3 — Unificar todo con base_maestra_parte2
Combina las bases de todos los años en una hoja única “Base Maestra”.
Marca automáticamente a quienes respondieron (Sí/No).
Genera un dataset centralizado para análisis.

🔹 Paso 7.4 — Generar resumen y gráficas (resumen_detallado_parte1)
Crea indicadores clave:
Total de egresados
Total que respondieron
Porcentaje de respuesta
Distribución por carrera y periodo
Genera gráficas automáticas de barras y columnas agrupadas.

🔹 Paso 7.5 — Ejecutar final_parte2 o proyecto_final
Corre el flujo completo de manera secuencial.
Obtiene Base Maestra final y Resumen visual.
Deja el archivo listo para presentación o envío institucional.

parte 3
Manual de Usuario: Variación de Carreras – Reporte de Egresados
1. Propósito
Este script de Excel permite analizar los datos de egresados, mostrando:
Cuántos alumnos están trabajando en su perfil profesional.
Tiempo promedio que tardan en conseguir su primer empleo.

Distribución de ingresos.
Comparación por carrera y reporte global.
Está pensado para entregar a la academia información confiable y visual de los resultados de los egresados.

2. Preparación de los datos
El archivo Excel debe contener una hoja llamada “Respondieron” con todas las respuestas de los egresados.
Las columnas mínimas requeridas son:
Carrera de egreso (o sinónimo).
Si labora aplicando su carrera (o sinónimo).
Tiempo hasta primer empleo (meses o rangos de tiempo).
Ingreso promedio del primer empleo.

Notas:
Los nombres de las columnas pueden variar, el script detecta sinónimos automáticamente.
Las respuestas de texto que indiquen “aún no” o “no he tenido empleo” se excluyen de los cálculos de empleo.

3. Estructura de los reportes
El script genera:

3.1 Hoja “Resumen empleo”
Resumen global:
Total de egresados.
Número y porcentaje que laboran en su perfil.
Tiempo promedio en meses hasta su primer empleo.

Resumen por carrera:
Total de egresados por carrera.
Número y porcentaje que laboran en su perfil.
Tiempo promedio hasta primer empleo.
Distribución global por tramos de meses:
Tramos: 0–3, 4–6, 7–12, 13+ meses, y “sin dato”.
Gráfico de columnas mostrando la cantidad de egresados por tramo.
Distribución por tramos por carrera:
Muestra la cantidad de egresados en cada tramo por carrera.
Se visualiza como tabla y gráfico.
Distribución de ingresos (conteo por rango):
Tablas y gráficos que muestran cuántos egresados reportan cada rango de ingreso.
Datos presentados globalmente, sin distinguir entre carreras.

3.2 Hojas individuales por carrera
Una hoja por cada carrera, con nombre limpio y seguro para Excel.

Incluye:
Resumen: Total, dentro/fuera de perfil, % en perfil, tiempo promedio.
Distribución por tramos de meses: tabla y gráfico.
Distribución de ingresos por carrera: tabla y gráfico.
Mensaje “Sin respuestas de ingreso para esta carrera” si no hay datos de ingresos.

4. Cómo interpretar los reportes
% en perfil: indica cuántos egresados están trabajando en un área relacionada con su carrera.
Meses promedio: tiempo que tardaron en conseguir el primer empleo.
Distribución de tramos: permite ver la rapidez con que los egresados encuentran empleo.
Distribución de ingresos: permite ver el rango de remuneración y su frecuencia.

5. Mejoras implementadas respecto al script anterior “Resumen Empleo”
Conteo de ingresos por carrera, no solo global.
Creación de hojas individuales por carrera para análisis más detallado.
Gráficos mejor organizados, con títulos claros y legibles.
Mensajes automáticos para carreras sin datos de ingresos.
Estilo uniforme: fuente, tamaño, autofit y tablas estilizadas en todas las hojas.
Sanitización de nombres de hojas, evitando errores por caracteres inválidos o nombres largos.

6. Partes que se pueden mejorar o personalizar
Detección de columnas: ampliar lista de sinónimos o permitir selección manual.
Rangos de tiempo: parametrizar los tramos o calcular percentiles.
Ingresos: agregar cálculo de ingreso promedio o mediana numérica real.
Validación de datos: limpieza de texto para evitar duplicados o espacios.
Gráficos: agregar gráficos de línea, porcentaje acumulado o comparativos.
Interactividad: incluir filtros dinámicos, segmentadores o botones de selección por rango de meses, ingresos o carreras.

7. Uso recomendado
Mantener la hoja de respuestas limpia y con nombres claros de columnas.
Ejecutar el script para generar el Resumen global y las hojas de cada carrera.
Revisar los gráficos y tablas para detectar tendencias o áreas de mejora.
Entregar a la academia como reporte consolidado o por carrera según necesidad.

