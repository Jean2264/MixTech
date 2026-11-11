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

    id_empleado    INT            FK, UNIQUE, NULL

    nombre_usuario VARCHAR(50)    UNIQUE, NOT NULL

    email          VARCHAR(100)   UNIQUE, NOT NULL

    passwordHash   VARCHAR(255)   NOT NULL

    estado         BOOLEAN        NOT NULL

    fecha_alta     DATE           NOT NULL

## empleado

### campo----------tipo----------restriccion

    id_empleado   INT            PK, autoincremental

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
