# Diccionario de datos

**## usuarios, roles, empleados, clientes y proveedores**

## rol

### campo---------tipo------------restriccion

    id_rol         INT             PK, autoincremental

    nombre        varchar(100)    not null

    descripcion   varchar(100)

    fecha_alta    date

## usuario

### campo----------tipo----------restriccion

    id_usuario     INT            PK, autoincremental

   id_rol         INT            FK, NOT NULL
codigo_usuario varchar(30) unique null

    id_empleado    INT            FK, UNIQUE, NULL

    nombre_usuario VARCHAR(50)    UNIQUE, NOT NULL

    email          VARCHAR(100)   UNIQUE, NOT NULL

    passwordHash   VARCHAR(255)   NOT NULL

    estado         BOOLEAN        NOT NULL

    fecha_alta     DATE           NOT NULL

## empleado

### campo----------tipo----------restriccion

    id_empleado   INT            PK, autoincremental
dni varchar(8) unique not null

    nombre         VARCHAR(100)   NOT NULL

    apellido       VARCHAR(100)   NOT NULL

    dni            VARCHAR(8)     UNIQUE, NOT NULL

    telefono       VARCHAR(30)

    email          VARCHAR(100)

    fecha_ingreso  DATE           NOT NULL

    estado         BOOLEAN        NOT NULL

## cliente

### campo---------tipo------------restriccion

    id_cliente   INT             PK, autoincremental

    nombre        varchar(100)    not null

    cuit          varchar(11)     unique

    telefono      varchar(30)

    email         varchar(100)

    estado        boolean

    fecha_alta    date

## proveedor

### campo----------tipo----------restriccion

    id_proveedor    INT            PK, autoincremental

    nombre         VARCHAR(100)   NOT NULL

    cuit           VARCHAR(11)    UNIQUE

    telefono       VARCHAR(30)

    email          VARCHAR(100)

    direccion      VARCHAR(150)

    estado         BOOLEAN        NOT NULL

    fecha_alta     DATE           NOT NULL

## proveedor_tipo

### campo----------tipo----------restriccion

    id_proveedor     INT           PK, FK

    id_tipo_proveedor INT          PK, FK

## tipo_proveedor

### campo----------tipo----------restriccion

    id_tipo_proveedor INT       PK, autoincremental

    nombre          VARCHAR(100)  UNIQUE, NOT NULL

    descripcion     VARCHAR(150)

    estado          BOOLEAN       NOT NULL

---

**##Repuesto y dispositivos**

## repuesto

### Campo ----------- Tipo ---------- Restricción

id_repuesto       INT           PK, autoincremental

id_marca         INT       FK, NOT NULL

id_tipo_repuesto INT             FK, NOT NULL

codigo_interno     VARCHAR(30)     UNIQUE, NOT NULL

nombre           VARCHAR(120)    NOT NULL

descripcion       VARCHAR(300)    NULL

precio_compra     DECIMAL(12,2)   NULL

precio_venta       DECIMAL(12,2)   NULL

estado           BOOLEAN         NOT NULL

fecha_alta         DATE            NOT NULL

## tipo_repuesto

### Campo ----------- Tipo ---------- Restricción

id_tipo_repuesto int pk, autoincrement
nombre varchar(100) not null
descripcion varchar(100) null
estado boolean

## marca_repuesto

### Campo --------------- Tipo ---------- Restricción

id_marca int pk, autoincrement
id_tipo_marca int fk, not null
nombre varchar(100) not null
estado boolean

## tipo_marca_repuesto

### Campo --------------- Tipo ---------- Restricción

id_tipo_marca int pk, autoincrement
nombre varchar(100) not null
descripcion varchar(100) null
estado boolean

## imagen_repuesto

### Campo --------------- Tipo ---------- Restricción

id_imagen int pk, autoincrement
id_repuesto int fk, not null
ruta_imagen varchar(max) not null
es_principal boolean
fecha_alta date
estado

## repuesto_compatibilidad

### Campo --------------- Tipo ---------- Restricción

id_repuesto int pk, fk
id_modelo_dispositivo int pk, fk

## marca_dispositivo

### Campo --------------- Tipo ---------- Restricción

id_marca_dispositivo int pk, autoincrement
nombre varchar(100)
estado boolean

## tipo_dispositivo

### Campo --------------- Tipo ---------- Restricción

id_tipo_dispositivo int pk, autoincrement
nombre varchar(100)
descripcion varchar(100)
estado boolean

## modelo_dispositivo

### Campo --------------- Tipo ---------- Restricción

id_modelo_dispositivo int pk, autoincrement
id_marca_dispositivo int fk
id_tipo_dispositivo int fk
nombre varchar(100)
estado boolean

**##ingreso y recepcion de equipos**

## ingreso

### Campo --------------- Tipo ---------- Restricción

id_ingreso int pk, autoincrement
codigo_ingreso varchar(30) unique, not null
id_cliente int fk, null
id_empleado int fk, not null
fecha_ingreso date
estado_ingreso varchar(100) not null
observaciones varchar(500) null

## ingreso_equipo

### Campo --------------- Tipo ---------- Restricción

id_ingreso_equipo int pk, autoincrement
id_ingreso int fk, not null
id_modelo_dispositivo int fk, not null
numero_serie varchar(100) null
imei varchar(20) null
estado_fisico varchar(100) not null
fecha_alta date
observaciones varchar(500) null

## imagen_ingreso

### Campo --------------- Tipo ---------- Restricción

id_imagen_ingreso int pk, autoincrement
id_ingreso_equipo int fk, not null
ruta_imagen varchar(max)
fecha_alta date
estado boolean

## accesorio_ingreso

### Campo --------------- Tipo ---------- Restricción

id_accesorio_ingreso int pk, autoincrement
id_ingreso_equipo int fk, not null
nombre varchar(250)

**##ordenes de servicio**

## orden_servicio

### Campo --------------- Tipo ---------- Restricción

id_orden_servicio int pk, autoincrement
id_ingreso_equipo int fk, unique, not null
numero_orden varchar(30) unique, not null
id_empleado int fk, not null
estado_orden varchar(40) not null
fecha_inicio date null
fecha_finalizacion date null
observaciones varchar(500) null
fecha_alta date not null

## diagnostico

### Campo --------------- Tipo ---------- Restricción

id_diagnostico int pk, autoincrementt
id_orden_servicio int fk, not null
id_empleado int fk, not null
descripcion varchar(max) not null
fecha_diagnostico date not null
observacion varchar(500) null

\*_observacion_
una misma orden puede tener varios diagnosticos

## trabajo_servicio

### Campo --------------- Tipo ---------- Restricción

id_trabajo_servicio int pk, autoincrement
id_orden_servicio int fk, not null
id_empleado int fk, not null
descripcion varchar(300) not null
estado_trabajo varchar(30) not null
fecha_inicio date null
fecha_finalizacion date null
observaciones VARCHAR(500) null

**Descripción:** Registra los trabajos realizados o pendientes dentro de una orden de servicio.

**Observaciones:**

- Una orden podrá contener varios trabajos.
- Cada trabajo podrá tener un empleado responsable.
- Se registrará el estado y las fechas correspondientes a cada trabajo.

## presupuesto

### Campo --------------- Tipo ---------- Restricción

id_presupuesto int pk, autoincrement
codigo_presupuesto varchar(30) unique null
id_cliente int fk, null
id_orden_servicio int fk
id_modelo_dispositivo int fk, not null
numero_serie varchar(100) null
estado_presupuesto varchar(30) not null
fecha_emision date not null
observaciones varchar(300) null

**Descripción:** Almacena la información general de los presupuestos realizados para clientes.

**Observaciones:**

- Un presupuesto podrá estar asociado a una orden de servicio existente.
- Si `IdOrdenServicio` es NULL, se considerará un presupuesto independiente.
- El modelo del dispositivo permitirá identificar su marca y tipo.
- El estado permitirá identificar si el presupuesto está pendiente, aceptado, rechazado, vencido o cancelado.

## presupuesto_detalle

### Campo --------------- Tipo ---------- Restricción

id_presupuesto_detalle int pk, autoincrement
id_presupuesto int fk, not null
id_repuesto int fk
tipo_concepto varchar(30) not null
descripcion varchar(30) not null
cantidad int  
precio_unitario decimal(12,2) not null
subtotal decimal(12,2) not null

**Descripción:** Registra los conceptos incluidos en cada presupuesto, como mano de obra, repuestos u otros servicios.

**Observaciones:**

- Un presupuesto podrá contener varios conceptos.
- `IdRepuesto` será NULL cuando el concepto no corresponda a un repuesto del inventario.
- Se conservará el precio utilizado al momento de generar el presupuesto.

**##ventas directas**

## venta

### Campo --------------- Tipo ---------- Restricción

id_venta int pk, autoincrement
codigo_venta varchar(30) unique not null
id_cliente int fk null
id_empleado int not null
fecha_venta date
descuento decimal(12,2)
subtotal decimal(12,2)
total decimal(12,2)
estado_venta varchar(30) not null
observacion varchar(500)

**Descripción:** Almacena la información general de las ventas directas realizadas en el establecimiento.

**Observaciones:**

- El cliente será opcional.
- Cada venta tendrá un empleado responsable.
- Una venta podrá contener uno o varios productos.
- El total se calculará considerando los importes y descuentos correspondientes.

## venta_detalle

### Campo --------------- Tipo ---------- Restricción

id_venta_detalle int pk, autoincrement
id_venta int fk
id_repuesto int fk not null
cantidad int  
descuento decimal(12,2)
subtotal decimal(12,2)

**Descripción:** Registra los productos y repuestos incluidos en cada venta.

**Observaciones:**

- Una venta podrá contener varios detalles.
- Cada detalle estará asociado a un repuesto del inventario.
- Se conservará el precio unitario utilizado al momento de la venta.

**##comprobantes**

## comprobante

### Campo --------------- Tipo ---------- Restricción

id_comprobante int pk, autoincrement
codigo_comprobante varchar(30) unique
tipo_comprobante varchar(30)
id_ingreso int null
id_presupuesto int fk, null
id_venta int fk, null
ruta_archivo varchar(255)
fecha_emision date

**Descripción:** Almacena la información de los comprobantes y documentos generados por el sistema.

**Observaciones:**

- Un comprobante podrá estar relacionado con un ingreso, presupuesto o venta.
- La ruta del archivo permitirá localizar el documento generado.
- El tipo de comprobante permitirá identificar el documento emitido.
- La cardinalidad definitiva y la regla que determinará a qué operación pertenece cada comprobante quedan pendientes de análisis.

**##relaciones entre tablas**

La siguiente tabla resume las relaciones planteadas hasta el momento.

### Entidad principal -------- Entidad relacionada -------- Cardinalidad

Rol Usuario 1:N

Empleado Usuario 1:0..1

Proveedor ProveedorTipo 1:N

TipoProveedor ProveedorTipo 1:N

TipoMarcaRepuesto MarcaRepuesto 1:N

MarcaRepuesto Repuesto 1:N

TipoRepuesto Repuesto 1:N

Repuesto ImagenRepuesto 1:N

MarcaDispositivo ModeloDispositivo 1:N

TipoDispositivo ModeloDispositivo 1:N

Repuesto RepuestoCompatibilidad 1:N

ModeloDispositivo RepuestoCompatibilidad 1:N

Cliente Ingreso 1:N

Empleado Ingreso 1:N

Ingreso IngresoEquipo 1:N

ModeloDispositivo IngresoEquipo 1:N

IngresoEquipo AccesorioIngreso 1:N

IngresoEquipo ImagenIngreso 1:N

IngresoEquipo OrdenServicio 1:1

Empleado OrdenServicio 1:N

OrdenServicio Diagnostico 1:N

OrdenServicio TrabajoServicio 1:N

Empleado Diagnostico 1:N

Empleado TrabajoServicio 1:N

Cliente Presupuesto 1:N

OrdenServicio Presupuesto 1:N

ModeloDispositivo Presupuesto 1:N

Presupuesto PresupuestoDetalle 1:N

Presupuesto HistorialPresupuesto 1:N

Repuesto PresupuestoDetalle 1:N

Cliente Venta 1:N

Empleado Venta 1:N

Venta VentaDetalle 1:N

Repuesto VentaDetalle 1:N

**Relaciones de Comprobante:**

- Ingreso — Comprobante.
- Presupuesto — Comprobante.
- Venta — Comprobante.

La cardinalidad definitiva de estas relaciones queda pendiente de análisis.

**Relaciones N:M identificadas:**

- Proveedor — TipoProveedor, mediante ProveedorTipo.
- Repuesto — ModeloDispositivo, mediante RepuestoCompatibilidad.
