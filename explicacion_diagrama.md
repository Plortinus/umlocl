# Explicación del Diagrama de Clases UML - Sistema de Comercio Electrónico

## Descripción General

El diagrama de clases modeliza un sistema de comercio electrónico que permite a los usuarios comprar productos organizados por tipos, con un sistema de carritos de compra y descuentos aplicables tanto a productos individuales como a tipos de productos.

## Clases Principales

### 1. Usuario
- **Atributos**: `nick`, `clave`
- **Propósito**: Representa a los usuarios registrados del sistema
- **Relaciones**: Puede tener múltiples carritos de compra

### 2. TipoProducto
- **Atributos**: `nombre`, `impuesto`
- **Propósito**: Categoriza los productos (libros, discos, muebles, etc.)
- **Relaciones**: 
  - Contiene múltiples productos
  - Puede tener un descuento opcional

### 3. Producto
- **Atributos**: `nombre`, `precio`, `stock`
- **Propósito**: Representa productos concretos disponibles para la venta
- **Relaciones**: 
  - Pertenece a un tipo de producto
  - Puede tener un descuento opcional
  - Participa en múltiples órdenes de compra

### 4. CarritoCompra
- **Atributos**: `fechaCompra`
- **Propósito**: Contenedor de órdenes de compra de un usuario
- **Relaciones**: 
  - Pertenece a un usuario
  - Contiene una o más órdenes de compra

### 5. OrdenCompra
- **Atributos**: `cantidad`
- **Propósito**: Especifica la cantidad de un producto específico en un carrito
- **Relaciones**: 
  - Pertenece a un carrito
  - Se refiere a un producto específico

### 6. Descuento
- **Atributos**: `descripcion`, `tipo`, `porcentaje`, `unidadesParaRegalo`
- **Propósito**: Define descuentos aplicables a productos o tipos de productos
- **Tipos**: 
  - **Porcentaje**: Descuenta un porcentaje del precio
  - **Regalo de unidad**: Regala una unidad por cada X unidades compradas

## Decisiones de Diseño

### 1. Enumeración TipoDescuento
Se utilizó una enumeración para garantizar que solo existan dos tipos de descuento válidos: `porcentaje` y `regalo_unidad`.

### 2. Separación de Producto y TipoProducto
Esta separación permite:
- Aplicar impuestos a nivel de categoría
- Manejar descuentos tanto a nivel individual como de categoría
- Facilitar la gestión de inventario

### 3. Relación CarritoCompra-OrdenCompra
Se modeló como una composición donde:
- Un carrito contiene múltiples órdenes
- Cada orden especifica la cantidad de un producto específico
- Esto permite múltiples productos por carrito

### 4. Gestión de Descuentos
- Los descuentos pueden aplicarse tanto a productos individuales como a tipos de productos
- La restricción 7 asegura que no haya conflictos entre descuentos del mismo tipo

## Restricciones OCL Implementadas

### Restricciones de Integridad Básicas (1-4)
- **R1**: `cantidad >= 0` en OrdenCompra
- **R2**: `stock >= 0` en Producto  
- **R3**: `precio > 0` en Producto
- **R4**: `impuesto >= 0` en TipoProducto

### Restricciones de Negocio (5-8)
- **R5**: Porcentaje de descuento entre 0 y 100 (excluidos)
- **R6**: Debe existir al menos un descuento en el sistema
- **R7**: Productos y sus tipos no pueden tener el mismo tipo de descuento
- **R8**: Las unidades pedidas no pueden superar el stock disponible

### Restricciones Adicionales
Se agregaron restricciones adicionales para mejorar la robustez del modelo:
- Nombres no vacíos para usuarios, productos y tipos
- Carritos deben tener al menos una orden
- Descuentos de regalo deben especificar >= 2 unidades

## Validación

El archivo `ejemplos_objetos.use` contiene:
- **Casos válidos**: Demuestran el funcionamiento correcto del sistema
- **Casos de prueba**: Comentados para probar violaciones de restricciones
- **Consultas OCL**: Para verificar el cumplimiento de las restricciones

## Uso del Sistema

1. **Carga el modelo**: `use sistema_comercio_electronico.use`
2. **Crea instancias**: `open ejemplos_objetos.use`
3. **Verifica restricciones**: Las restricciones OCL se evalúan automáticamente
4. **Genera diagramas**: Utiliza la funcionalidad de USE para visualizar el diagrama de objetos

## Extensibilidad

El diseño permite futuras extensiones como:
- Nuevos tipos de descuento
- Métodos de pago
- Historial de compras
- Calificaciones de productos
- Gestión de envíos