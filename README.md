#🧩 README – Flujo n8n: Ejercicio 1
##📋 Descripción general

Este flujo automatiza el monitoreo de pedidos y cumplimiento de SLA.
Cada día, revisa los pedidos, calcula los días sin actualización, y:
    -   Registra los resultados en Google Sheets.
    - Envía mensajes empáticos a clientes con pedidos demorados (fuera de SLA).
    - Guarda los casos “en tiempo” en otra hoja.
    - Genera un reporte HTML y archivo CSV con el resumen del día.

#⚙️ Variables y credenciales requeridas

Elemento                | Tipo              | Descripción                                                                             |
| ----------------------- | ----------------- | --------------------------------------------------------------------------------------- |
| `Google Sheets account` | Credencial OAuth2 | Permite escribir y actualizar registros en hojas de cálculo.                            |
| `Gmail account`         | Credencial OAuth2 | Usada para enviar correos automáticos de reporte.                                       |
| `OpenAi account`        | Credencial API    | Utilizada por el nodo “Message a model” para generar mensajes empáticos personalizados. |

#🧾 Datos de entrada simulados

Nodo: Ingreso de Datos
Define un conjunto de pedidos de ejemplo (JSON) con campos:

#🚀 Pasos del flujo

1. Programa para envío diario
    - Nodo tipo Schedule Trigger: ejecuta el flujo automáticamente cada día.

2. Ingreso de Datos
    - Carga o recibe los pedidos a procesar.

3. Diferencia de fecha SLA
    - Calcula los días desde la última actualización (days_since_update).

4. If (Condición SLA)
    - Compara days_since_update.days con sla_days.

5. Si el pedido supera el SLA, continúa por la rama True; de lo contrario, por False.
    - Rama True (fuera de SLA)
    - Log: Registra el pedido en la hoja “Fuera de SLA”.
    - Message a model: Llama al modelo de OpenAI para redactar un mensaje empático.
    - Send a message: Envía el mensaje por Gmail.
    - Resumen: Actualiza la hoja “Reporte” con todos los pedidos demorados.
    - Markdown: Genera un reporte visual HTML del día.
    - Convert to File: Convierte el reporte en archivo CSV para registro o envío.

6. Rama False (en tiempo)
    - Pedidos en tiempo: Registra el pedido en la hoja “Dentro de SLA”.

#📊 Hojas de Google utilizadas

Hoja                         | URL               | Proposito                                                              |
| -----------------------    | ----------------- | ---------------------------------------------------------------------- |
| Ejercicio1 / Fuera de SLA` | Credencial OAuth2 | Permite escribir y actualizar registros.                               |
| Ejercicio1 / Lista mensajes| Credencial OAuth2 | Usada para enviar correos automáticos reporte.                         |
| Reporte diario             | Credencial API    | Utilizada por el nodo “Message a model” para generar mensajes empáticos|
|                            |                   | personalizados.                                                        |

#🧠 Lógica de decisión principal

Si days_since_update.days > sla_days:
    -> Registrar en “Fuera de SLA”
    -> Generar mensaje al cliente (OpenAI)
    -> Enviar correo
Sino:
    -> Registrar en “Pedidos en tiempo”


#Diagrama de Flujo

![alt text](Flujo.png)
