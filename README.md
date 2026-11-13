ConnectaTel: limpieza, segmentación y análisis de uso de clientes
1. Objetivo del proyecto
Analizar el comportamiento de los usuarios de ConnectaTel, una empresa de telecomunicaciones en Latinoamérica, a partir de tres tablas de datos. El proyecto busca:
    • Detectar y corregir problemas de calidad en los datos (valores centinela, fechas imposibles, nulos). 
    • Combinar las tablas en un perfil único por usuario. 
    • Identificar patrones de uso y valores extremos (outliers). 
    • Segmentar a los clientes por nivel de uso y por edad. 
    • Traducir los hallazgos en recomendaciones para el negocio (ofertas y planes). 
2. Datasets utilizados
Los tres archivos CSV deben estar en la misma carpeta que el notebook.
Archivo	Filas	Descripción
plans.csv	2	Catálogo de planes (Basico y Premium): mensajes y minutos incluidos, GB por mes, precio mensual y costos por GB, mensaje y minuto.
users_latam.csv	4000	Una fila por usuario: user_id, nombre, age, city, reg_date, plan y churn_date (fecha de baja; nulo si sigue activo).
usage.csv	40 000	Una fila por evento de uso: id, user_id, type (call o text), date, duration (llamadas) y length (mensajes).
Relaciones entre tablas
    • usage.user_id → users.user_id 
    • users.plan → plans.plan_name 
3. Etapas del análisis
    1. Carga y primera inspección: lectura de los CSV con pandas y revisión con .head() e .info(). 
    2. Identificación de problemas de calidad: 
        ◦ Conteo y proporción de nulos por columna. 
        ◦ Valores centinela: -999 en age y "?" en city. 
        ◦ Fechas: conversión a tipo fecha y detección de fechas imposibles (los datos llegan hasta 2024; hay 40 registros de reg_date con fecha 2026). 
        ◦ Nulos estructurales en duration y length (dependen de type) y filas con el valor en la columna que no corresponde al tipo. 
    3. Limpieza: 
        ◦ -999 en age reemplazado por la mediana de las edades válidas. 
        ◦ "?" en city convertido a nulo. 
        ◦ Fechas fuera de rango convertidas a nulo (NaT). 
        ◦ Los nulos de duration y length se conservan, porque no son errores. 
    4. Agregación y combinación: métricas por usuario (cant_mensajes, cant_llamadas, cant_minutos_llamada) y merge con users para crear user_profile. Los usuarios sin eventos quedan con 0. 
    5. Exploración estadística: describe() de las variables numéricas y distribución porcentual del plan. 
    6. Visualización de distribuciones: histogramas por plan, normalizados dentro de cada plan para comparar grupos de distinto tamaño. 
    7. Detección de outliers: boxplots y límites por el método IQR. Los outliers aparecen solo del lado derecho y se conservan (son valores plausibles de uso intenso). 
    8. Segmentación de clientes: 
        ◦ grupo_uso: Bajo uso, Uso medio, Alto uso. 
        ◦ grupo_edad: Joven (< 30), Adulto (< 60), Adulto Mayor. 
        ◦ Gráficos de barras y cruce de ambos grupos. 
    9. Insight ejecutivo: conclusiones y recomendaciones para los stakeholders. 
