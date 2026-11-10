# Diccionario de datos — MixTech

**Versión:** 0.1 – Preliminar  
**Estado:** En elaboración

---

# 1. Ingresos y recepción de equipos

La idea es separar el registro de recepción de los equipos que trae el cliente. De esta manera, un mismo ingreso puede contener más de un equipo y cada uno puede tener su propio seguimiento.

## Ingreso

**### Campo ---------------- Tipo ---------------- Restricción**

```text
IdIngreso          INT             PK, autoincremental
NumeroIngreso      VARCHAR(30)     UNIQUE, NOT NULL
IdCliente          INT             FK, NOT NULL
IdEmpleado         INT             FK, NOT NULL
FechaIngreso       DATETIME        NOT NULL
EstadoIngreso      VARCHAR(30)     NOT NULL
Observaciones      VARCHAR(500)    NULL
FechaAlta          DATETIME        NOT NULL
```

**Descripción:** Almacena la información general de cada ingreso realizado en el establecimiento.

**Observaciones:**

- Un ingreso pertenece a un cliente.
- Un empleado es responsable de registrar la recepción.
- Un ingreso puede contener uno o varios equipos.
- El número de ingreso será único.

---

## IngresoEquipo

**Descripción:** Representa cada equipo recibido dentro de un ingreso.

**### Campo ---------------- Tipo ---------------- Restricción**

```text
IdIngresoEquipo     INT             PK, autoincremental
IdIngreso           INT             FK, NOT NULL
IdModeloDispositivo INT             FK, NOT NULL
NumeroSerie         VARCHAR(100)    NULL
IMEI                VARCHAR(20)     NULL
EstadoFisico        VARCHAR(100)    NOT NULL
FallaDeclarada      VARCHAR(500)    NOT NULL
Observaciones       VARCHAR(500)    NULL
```

**Descripción:** Almacena la información individual de cada dispositivo recibido.

**Observaciones:**

- Cada equipo pertenece a un ingreso.
- El modelo permite identificar la marca y el tipo de dispositivo.
- El número de serie y el IMEI podrán ser opcionales, dependiendo del dispositivo.
- Cada equipo podrá tener sus propias fotografías y accesorios asociados.

---

## AccesorioIngreso

**Descripción:** Registra los accesorios que el cliente entrega junto con cada equipo.

**### Campo ---------------- Tipo ---------------- Restricción**

```text
IdAccesorioIngreso  INT             PK, autoincremental
IdIngresoEquipo     INT             FK, NOT NULL
Nombre              VARCHAR(100)    NOT NULL
Descripcion         VARCHAR(200)    NULL
```

**Observaciones:**

- Un equipo podrá tener varios accesorios registrados.
- Los accesorios se registrarán individualmente para identificar qué elementos fueron entregados.

---

## ImagenIngreso

**Descripción:** Almacena las fotografías relacionadas con la recepción de cada equipo.

**### Campo ---------------- Tipo ---------------- Restricción**

```text
IdImagenIngreso     INT             PK, autoincremental
IdIngresoEquipo     INT             FK, NOT NULL
RutaImagen          VARCHAR(255)    NOT NULL
Descripcion         VARCHAR(200)    NULL
FechaAlta           DATETIME        NOT NULL
```

**Observaciones:**

- Un equipo podrá tener varias fotografías.
- Se almacenará la ruta del archivo y no la imagen directamente en la base de datos.
- Las imágenes servirán como evidencia del estado físico del equipo al momento de la recepción.

---

# 2. Órdenes de servicio

Una orden de servicio representa el proceso de reparación de un equipo recibido.

## OrdenServicio

**### Campo ---------------- Tipo ---------------- Restricción**

```text
IdOrdenServicio     INT             PK, autoincremental
IdIngresoEquipo     INT             FK, UNIQUE, NOT NULL
NumeroOrden         VARCHAR(30)     UNIQUE, NOT NULL
IdEmpleado          INT             FK, NOT NULL
EstadoOrden         VARCHAR(40)     NOT NULL
FechaInicio         DATETIME        NULL
FechaFinalizacion   DATETIME        NULL
Observaciones       VARCHAR(500)    NULL
FechaAlta           DATETIME        NOT NULL
```

**Descripción:** Almacena la información principal de cada orden de servicio generada a partir de un equipo recibido.

**Observaciones:**

- Cada orden de servicio corresponde a un equipo ingresado.
- Un equipo podrá tener como máximo una orden de servicio asociada.
- El empleado responsable podrá ser diferente del empleado que registró el ingreso.
- El estado permitirá identificar la situación actual de la reparación.

---

## Diagnostico

**### Campo ---------------- Tipo ---------------- Restricción**

```text
IdDiagnostico       INT             PK, autoincremental
IdOrdenServicio     INT             FK, NOT NULL
IdEmpleado          INT             FK, NOT NULL
Descripcion         VARCHAR(1000)   NOT NULL
FechaDiagnostico    DATETIME        NOT NULL
Observaciones       VARCHAR(500)    NULL
```

**Descripción:** Permite registrar los diagnósticos realizados durante el proceso de reparación.

**Observaciones:**

- Una orden de servicio podrá tener varios diagnósticos.
- Cada diagnóstico estará asociado al empleado que lo realizó.
- Se conservará la fecha de cada diagnóstico.

---

## TrabajoServicio

**### Campo ---------------- Tipo ---------------- Restricción**

```text
IdTrabajoServicio   INT             PK, autoincremental
IdOrdenServicio     INT             FK, NOT NULL
IdEmpleado          INT             FK, NOT NULL
Descripcion         VARCHAR(300)    NOT NULL
EstadoTrabajo       VARCHAR(30)     NOT NULL
FechaInicio         DATETIME        NULL
FechaFinalizacion   DATETIME        NULL
Observaciones       VARCHAR(500)    NULL
```

**Descripción:** Registra los trabajos realizados o pendientes dentro de una orden de servicio.

**Observaciones:**

- Una orden podrá contener varios trabajos.
- Cada trabajo podrá tener un empleado responsable.
- Se registrará el estado y las fechas correspondientes a cada trabajo.

---

# 3. Presupuestos

Un presupuesto puede existir independientemente de una orden de servicio. Si el cliente lo acepta y todavía no existe un ingreso, se podrá generar el ingreso y su orden correspondiente.

## Presupuesto

**### Campo ---------------- Tipo ---------------- Restricción**

```text
IdPresupuesto       INT             PK, autoincremental
NumeroPresupuesto   VARCHAR(30)     UNIQUE, NOT NULL
IdCliente           INT             FK, NOT NULL
IdOrdenServicio     INT             FK, NULL
IdModeloDispositivo INT             FK, NOT NULL
NumeroSerie         VARCHAR(100)    NULL
EstadoPresupuesto   VARCHAR(30)     NOT NULL
FechaEmision        DATETIME        NOT NULL
FechaVencimiento    DATETIME        NULL
Observaciones       VARCHAR(500)    NULL
```

**Descripción:** Almacena la información general de los presupuestos realizados para clientes.

**Observaciones:**

- Un presupuesto podrá estar asociado a una orden de servicio existente.
- Si `IdOrdenServicio` es NULL, se considerará un presupuesto independiente.
- El modelo del dispositivo permitirá identificar su marca y tipo.
- El estado permitirá identificar si el presupuesto está pendiente, aceptado, rechazado, vencido o cancelado.

---

## PresupuestoDetalle

**### Campo ---------------- Tipo ---------------- Restricción**

```text
IdPresupuestoDetalle INT             PK, autoincremental
IdPresupuesto        INT             FK, NOT NULL
IdRepuesto           INT             FK, NULL
TipoConcepto         VARCHAR(30)     NOT NULL
Descripcion          VARCHAR(300)    NOT NULL
Cantidad             DECIMAL(10,2)   NOT NULL
PrecioUnitario       DECIMAL(12,2)   NOT NULL
Subtotal             DECIMAL(12,2)   NOT NULL
```

**Descripción:** Registra los conceptos incluidos en cada presupuesto, como mano de obra, repuestos u otros servicios.

**Observaciones:**

- Un presupuesto podrá contener varios conceptos.
- `IdRepuesto` será NULL cuando el concepto no corresponda a un repuesto del inventario.
- Se conservará el precio utilizado al momento de generar el presupuesto.

---

## HistorialPresupuesto

**### Campo ---------------- Tipo ---------------- Restricción**

```text
IdHistorialPresupuesto INT           PK, autoincremental
IdPresupuesto          INT           FK, NOT NULL
Version                INT           NOT NULL
EstadoPresupuesto      VARCHAR(30)   NOT NULL
FechaModificacion      DATETIME      NOT NULL
Observaciones          VARCHAR(500)  NULL
```

**Descripción:** Permite registrar las modificaciones y versiones realizadas sobre un presupuesto.

**Observaciones:**

- Un presupuesto podrá tener varias versiones.
- Se conservará la fecha de cada modificación.
- La versión permitirá identificar las distintas actualizaciones del presupuesto.

---

# 4. Ventas directas

Una venta directa permite vender repuestos o productos sin necesidad de generar una orden de servicio.

## Venta

**### Campo ---------------- Tipo ---------------- Restricción**

```text
IdVenta             INT             PK, autoincremental
NumeroVenta         VARCHAR(30)     UNIQUE, NOT NULL
IdCliente           INT             FK, NULL
IdEmpleado          INT             FK, NOT NULL
FechaVenta          DATETIME        NOT NULL
Subtotal            DECIMAL(12,2)   NOT NULL
Descuento           DECIMAL(12,2)   NOT NULL
Total               DECIMAL(12,2)   NOT NULL
EstadoVenta         VARCHAR(30)     NOT NULL
Observaciones       VARCHAR(500)    NULL
```

**Descripción:** Almacena la información general de las ventas directas realizadas en el establecimiento.

**Observaciones:**

- El cliente será opcional.
- Cada venta tendrá un empleado responsable.
- Una venta podrá contener uno o varios productos.
- El total se calculará considerando los importes y descuentos correspondientes.

---

## VentaDetalle

**### Campo ---------------- Tipo ---------------- Restricción**

```text
IdVentaDetalle      INT             PK, autoincremental
IdVenta             INT             FK, NOT NULL
IdRepuesto          INT             FK, NOT NULL
Cantidad            DECIMAL(10,2)   NOT NULL
PrecioUnitario      DECIMAL(12,2)   NOT NULL
Descuento           DECIMAL(12,2)   NOT NULL
Subtotal            DECIMAL(12,2)   NOT NULL
```

**Descripción:** Registra los productos y repuestos incluidos en cada venta.

**Observaciones:**

- Una venta podrá contener varios detalles.
- Cada detalle estará asociado a un repuesto del inventario.
- Se conservará el precio unitario utilizado al momento de la venta.

---

# 5. Comprobantes

En esta primera versión, contemplamos el registro de los comprobantes generados para las distintas operaciones del sistema.

## Comprobante

**### Campo ---------------- Tipo ---------------- Restricción**

```text
IdComprobante       INT             PK, autoincremental
NumeroComprobante   VARCHAR(30)     UNIQUE, NOT NULL
TipoComprobante     VARCHAR(30)     NOT NULL
IdIngreso           INT             FK, NULL
IdPresupuesto       INT             FK, NULL
IdVenta             INT             FK, NULL
RutaArchivo         VARCHAR(255)    NULL
FechaEmision        DATETIME        NOT NULL
```

**Descripción:** Almacena la información de los comprobantes y documentos generados por el sistema.

**Observaciones:**

- Un comprobante podrá estar relacionado con un ingreso, presupuesto o venta.
- La ruta del archivo permitirá localizar el documento generado.
- El tipo de comprobante permitirá identificar el documento emitido.

---

# 6. Relaciones entre entidades

**## Entidad principal ---------------- Entidad relacionada ---------------- Cardinalidad**

| Entidad principal | Entidad relacionada  | Cardinalidad |
| ----------------- | -------------------- | ------------ |
| Cliente           | Ingreso              | 1:N          |
| Empleado          | Ingreso              | 1:N          |
| Ingreso           | IngresoEquipo        | 1:N          |
| ModeloDispositivo | IngresoEquipo        | 1:N          |
| IngresoEquipo     | AccesorioIngreso     | 1:N          |
| IngresoEquipo     | ImagenIngreso        | 1:N          |
| IngresoEquipo     | OrdenServicio        | 1:1          |
| Empleado          | OrdenServicio        | 1:N          |
| OrdenServicio     | Diagnostico          | 1:N          |
| OrdenServicio     | TrabajoServicio      | 1:N          |
| Empleado          | Diagnostico          | 1:N          |
| Empleado          | TrabajoServicio      | 1:N          |
| Cliente           | Presupuesto          | 1:N          |
| OrdenServicio     | Presupuesto          | 1:N          |
| ModeloDispositivo | Presupuesto          | 1:N          |
| Presupuesto       | PresupuestoDetalle   | 1:N          |
| Presupuesto       | HistorialPresupuesto | 1:N          |
| Repuesto          | PresupuestoDetalle   | 1:N          |
| Cliente           | Venta                | 1:N          |
| Empleado          | Venta                | 1:N          |
| Venta             | VentaDetalle         | 1:N          |
| Repuesto          | VentaDetalle         | 1:N          |

**Relaciones de Comprobante:**

- Ingreso — Comprobante.
- Presupuesto — Comprobante.
- Venta — Comprobante.

La cardinalidad definitiva de estas relaciones queda pendiente de análisis.

---

# 7. Pendientes de análisis

- Definir los estados definitivos de ingresos y órdenes de servicio.
- Determinar si un presupuesto podrá tener varias versiones completas con sus respectivos detalles.
- Definir cómo se registrarán los pagos, señas y pagos parciales.
- Diseñar las entidades relacionadas con garantías.
- Definir cómo se vincularán las reparaciones con los repuestos utilizados.
- Analizar el registro de movimientos de inventario por ventas y reparaciones.
- Definir las condiciones de emisión y almacenamiento de comprobantes.

---

**Fin del documento – Versión 0.1**

_Documento preliminar sujeto a modificaciones durante el análisis y desarrollo de MixTech._
