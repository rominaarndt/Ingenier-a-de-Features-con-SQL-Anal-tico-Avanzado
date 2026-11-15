Ingeniería de Features con SQL Analítico Avanzado
Este proyecto implementa un modelo analítico de segmentación y predicción de comportamiento de usuarios utilizando SQL avanzado (SQLite) y Python/Pandas. A partir de los datos transaccionales de compras, se calculan variables de comportamiento clave para cada usuario.

📋 Estructura de Datos
El modelo utiliza dos tablas principales creadas en una base de datos SQLite en memoria:

usuarios (usuario_id, nombre)
pedidos (pedido_id, usuario_id, fecha, monto)
📊 Métricas Calculadas
Mediante una consulta consolidada con expresiones de tabla común (WITH ... AS), se derivan las siguientes variables analíticas:

RFM (Recency, Frequency, Monetary):

Recency (Recencia):
Qué mide: Los días transcurridos desde la última compra del usuario hasta la fecha de referencia (corte en 2024-03-31).
Cómo se calcula: julianday('2024-03-31') - julianday(MAX(fecha)) por usuario (equivalente a DATE_DIFF de días).
Nulos: Los usuarios sin compras reciben por defecto un valor de 999 (indica que nunca han comprado).
Por qué sirve para churn: A mayor recencia, existe una mayor probabilidad estadística de abandono o inactividad prolongada.
Frequency (Frecuencia): Número total de pedidos únicos realizados por el usuario.
Monetary (Valor Monetario): Gasto total acumulado por el usuario.
Trend (Tendencia de Gasto):

Mide el cambio en el comportamiento financiero del cliente comparando el gasto realizado en el último mes activo (2024-03-01) contra el promedio de gasto mensual de los meses históricos anteriores.
Churn (Fuga de Clientes):

Variable binaria predictiva (1 o 0). Se clasifica a un usuario en situación de Churn (1) si cumple con alguna de las siguientes condiciones de negocio:
No ha realizado ninguna transacción en el historial (frequency = 0).
Su última transacción fue hace más de 60 días (recency > 60).
🛠️ Requisitos de Ejecución
Python 3.x
Biblioteca pandas
Biblioteca estándar sqlite3
