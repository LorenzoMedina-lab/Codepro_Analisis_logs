Analisis de Logs de Servidor
Descripcion
Este notebook realiza un analisis de logs de servidor con el objetivo de detectar momentos criticos de degradacion del sistema, identificar los servicios y endpoints afectados, y comparar el comportamiento durante el incidente contra una linea base de operacion normal.
Requisitos

Python 3.x
pandas
matplotlib

Instalacion de dependencias:
bashpip install pandas matplotlib
Estructura del Analisis
1. Carga y preparacion de datos
Se carga el archivo server_logs.csv y se convierte la columna timestamp_event al tipo datetime para permitir operaciones temporales.
2. Exploracion inicial
Se realiza una revision general del dataset que incluye:

Total de registros de log
Distribucion de eventos por nivel de severidad
Servicio con mayor y menor cantidad de logs
Mensaje de error mas frecuente

3. Clasificacion de eventos criticos
Se define un "bad event" como aquel que cumple al menos una de las siguientes condiciones:

Severidad igual a ERROR o CRITICAL
Codigo de estado HTTP mayor o igual a 500

Los eventos se agrupan en ventanas temporales de 5 minutos y se calcula el Bad Rate (proporcion de eventos malos sobre el total) para cada ventana.
4. Deteccion del momento critico
Se filtran unicamente las ventanas con al menos 20 eventos y se seleccionan los 5 periodos con mayor Bad Rate. El intervalo de mayor Bad Rate es identificado como el momento critico del incidente.
5. Diagnostico del incidente
Para el momento critico identificado se analiza:

Servicios con mayor cantidad de bad events
Mensajes de error mas frecuentes
Endpoints con mayor cantidad de errores

6. Comparacion Incidente vs Baseline
Se construye una tabla comparativa entre el periodo del incidente y el resto del dataset (baseline), evaluando las siguientes metricas:
MetricaDescripcionTotal EventsCantidad de eventos registradosBad RateProporcion de eventos con errorAvg Latency (ms)Latencia promedio en milisegundos% 5xxPorcentaje de respuestas con error 5xx
7. Visualizaciones
Se generan dos graficos:

Evolucion del conteo de eventos por severidad a lo largo del tiempo (bins de 5 minutos)
Evolucion del Bad Rate en el tiempo, con referencia al promedio general

Entrada esperada
El archivo server_logs.csv debe contener al menos las siguientes columnas:
ColumnaTipoDescripciontimestamp_eventdatetimeFecha y hora del eventoseveritystringNivel de severidad del logstatus_codeintCodigo de estado HTTPservice_namestringNombre del servicio que genero el logmessagestringMensaje del logendpointstringEndpoint al que corresponde el eventolatency_msfloatLatencia de la solicitud en ms
Salida
El notebook imprime en consola un resumen del incidente detectado e incluye una seccion de conclusion con los siguientes datos a completar una vez ejecutado el analisis:

Hora del momento critico
Valor del Bad Rate en ese instante
Servicio mas afectado
Endpoint con mas errores
Mensaje de error dominante
Comparacion porcentual de errores 5xx entre el incidente y el baseline