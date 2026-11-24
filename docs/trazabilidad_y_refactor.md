# Trazabilidad y registro de refactor (Tema 5, CIIN1322P)

Proyecto: sistema de pedidos de Ecoflor. Autora: Yopla Huamán, Ana Elizabeth.

### Tabla 1. Requisitos y casos de uso base

| ID | Descripción | Caso de uso | Actor |
|---|---|---|---|
| RF-01 | Registrar la cosecha por vivero | CU-01 Registrar cosecha | Productor |
| RF-02 | Registrar pedidos de clientes (WhatsApp) | CU-02 Registrar pedido | Administrador |
| RF-03 | Calcular precio según largo de tallo | CU-02 Registrar pedido | Administrador |
| RF-04 | Registrar envío a Chiclayo por bus | CU-03 Registrar envío | Administrador |
| RF-05 | Consultar stock de paquetes disponibles | CU-04 Consultar stock | Administrador |

### Tabla 3. Matriz de trazabilidad requisito–clase

| Clase | RF-01 (CU-01) | RF-02 (CU-02) | RF-03 (CU-02) | RF-04 (CU-03) | RF-05 (CU-04) |
|---|---|---|---|---|---|
| Cliente |  | X |  |  |  |
| Pedido |  | X | X | X |  |
| DetallePedido |  | X | X |  |  |
| Paquete | X | X | X |  | X |
| Tarifa |  |  | X |  |  |
| Cosecha | X |  |  |  | X |
| Vivero | X |  |  |  |  |
| Envio |  |  |  | X |  |
| ControlPedido |  | X |  |  |  |

### Tabla 4. Matriz CU–responsabilidad–clase del CU-02 Registrar pedido

| N.° | Responsabilidad | Clase responsable | Req. | CU |
|---|---|---|---|---|
| 1 | Recibir los datos del pedido | ControlPedido | RF-02 | CU-02 |
| 2 | Identificar al cliente | Cliente | RF-02 | CU-02 |
| 3 | Verificar stock del paquete | Paquete | RF-02 | CU-02 |
| 4 | Obtener el precio según largo de tallo | Tarifa | RF-03 | CU-02 |
| 5 | Crear el pedido y calcular su total | Pedido | RF-02, RF-03 | CU-02 |
| 6 | Crear los detalles y calcular sus subtotales | DetallePedido | RF-02, RF-03 | CU-02 |
| 7 | Confirmar el registro | ControlPedido | RF-02 | CU-02 |

### Tabla 5. Registro de refactor del modelo (R1–R7)

| N.° | Req/CU | Problema detectado | Cambio aplicado | Justificación |
|---|---|---|---|---|
| R1 | RF-03 / CU-02 | Pedido guardaba precios y destino | Se separaron Tarifa y Envio | Mejora la cohesión: una responsabilidad por clase |
| R2 | RF-01 / CU-01 | Paquete mezclaba datos de la cosecha | Se creó Cosecha (lote por vivero) | Evita duplicar datos y traza el origen |
| R3 | RF-02 / CU-02 | Alto acoplamiento Cliente–Paquete | La relación pasa por DetallePedido | Reduce dependencias directas |
| R4 | RF-02 / CU-02 | Flujo de CU-02 repartido entre Cliente, Paquete y Tarifa | Se agregó ControlPedido (solo coordina) | Un solo CU; delega en las clases de dominio |
| R5 | RF-04 / CU-03 | Ciclo Ventas ↔ Logística (Pedido y Envio se referenciaban) | Envio navega solo hacia Pedido | Elimina la dependencia circular |
| R6 | RF-05 / CU-04 | Paquete sin atributo de stock | Se agregó -stock; se quitó descontarStock() | RF-05 consulta el stock. Limitación: no baja al registrar pedidos |
| R7 | RF-01 / CU-01 | Cosecha.cantidadPaquetes duplicaba la relación con Paquete | Se eliminó el atributo | Evita redundancia |

