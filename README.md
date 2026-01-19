# Proyecto: Tienda de Memorias - E-Commerce

## Descripción General

Proyecto de e-commerce desarrollado con JavaScript vanilla para la venta de memorias (MicroSD, RAM y USB). El proyecto implementa funcionalidades de carrito de compras persistente usando localStorage.

---

## Conceptos de JavaScript Aplicados

### 1. **DOM (Document Object Model)**

- **querySelector()**: Selecciona elementos HTML específicos
  - Ejemplo: `document.querySelector('.box-container')` en `app.js`
  - Uso: Obtener referencias a elementos para manipularlos

- **createElement()**: Crea elementos DOM dinámicamente
  - Ejemplo: `document.createElement('div')` en ambos archivos
  - Uso: Generar tarjetas de productos desde datos JSON

- **appendChild()**: Añade elementos creados al DOM
  - Ejemplo: `contenedor.appendChild(div)` en `app.js`
  - Uso: Renderizar productos en la página

- **innerHTML**: Inserta contenido HTML dentro de elementos
  - Uso: Rellenar tarjetas con información del producto

### 2. **Fetch API**

- **fetch()**: Realiza peticiones HTTP asincrónicas
  - Ejemplo: `fetch("../js/stock.json")`
  - Obtiene datos del archivo JSON de productos

- **.then()**: Maneja promesas y transforma respuestas
  - Encadenamiento: `.json()` convierte respuesta a objeto JavaScript

### 3. **JSON (JavaScript Object Notation)**

- Formato de datos en `stock.json` con array de productos
- Cada producto contiene: id, tipo, imagen, marca, descripción, capacidad, precio
- Se parsea con `JSON.parse()` y se serializa con `JSON.stringify()`

### 4. **localStorage API**

- **getItem()**: Recupera datos almacenados
  - `localStorage.getItem('carrito')` obtiene carrito guardado
- **setItem()**: Guarda datos en el navegador
  - `localStorage.setItem('carrito', JSON.stringify(carrito))`

- **Persistencia**: Los datos se mantienen entre sesiones

### 5. **Métodos de Array**

- **forEach()**: Itera sobre arrays
  - Recorre productos para renderizarlos
  - Recorre carrito para calcular totales

- **find()**: Busca un elemento que cumple condición
  - `productos.find(p => p.id === productoId)` busca producto por ID

- **filter()**: Crea nuevo array con elementos filtrados
  - `carrito.filter(producto => producto.id !== productoId)` elimina producto

### 6. **Arrow Functions**

- Sintaxis moderna: `() => {}`
- Usadas en callbacks de eventos y métodos de array
- Mantienen contexto `this` léxico

### 7. **Event Listeners**

- **addEventListener()**: Asocia eventos a elementos
  - `document.addEventListener('DOMContentLoaded', ...)` espera carga del DOM
  - `contenedor.addEventListener('click', ...)` delega eventos
  - `onclick="addToCart()"` en HTML para botones

- **Event Delegation**: Un listener captura eventos de múltiples elementos
  - Verifica `e.target.classList.contains('cart-btn')`

### 8. **Métodos de String**

- **textContent**: Asigna texto a elementos
  - `carritoCompra.textContent = contador`

- **toFixed()**: Redondea números a decimales específicos
  - `totalPagar.toFixed(2)` formatea precio a 2 decimales

### 9. **Clases CSS y Manipulación**

- **classList.contains()**: Verifica si elemento tiene clase
- **className**: Asigna múltiples clases
  - Bootstrap: `col-xl-4 col-lg-6` para responsive design

### 10. **Template Literals**

- Sintaxis con backticks: `` `Texto ${variable}` ``
- Interpolación de datos: `${item.precio}`, `${item.img}`
- Facilita creación de HTML dinámico

### 11. **Operadores Lógicos y Condicionales**

- **||**: OR lógico para valores por defecto
  - `JSON.parse(...) || []` usa array vacío si carrito no existe

- **if statements**: Condicionales para validaciones
- **+=**: Operador de asignación compuesta para contadores

### 12. **Librerías Externas**

- **SweetAlert (Swal)**: Notificaciones UI mejoradas
  - Toast para confirmación de agregado al carrito
  - Modal para confirmación de eliminación

---

## Flujo de Funciones

### **PÁGINA PRINCIPAL (productos.html + app.js)**

```
INICIO
  ↓
pintarProductos()
  ├─ fetch("../js/stock.json")
  │  └─ Obtiene array de productos
  │
  ├─ .then(stock => {
  │  ├─ stock.forEach(item => {
  │  │  ├─ createElement('div')
  │  │  ├─ innerHTML con detalles del producto
  │  │  └─ appendChild(div) al contenedor
  │  │})
  │  └─})
  │
  └─ Renderiza 9 productos en la página

ESCUCHA DE EVENTOS
  ├─ Contador = 0 (Inicializa)
  │
  └─ contenedor.addEventListener('click', (e) => {
     ├─ if (e.target.classList.contains('cart-btn'))
     │  ├─ contador++
     │  └─ actualizarContador()
     │     └─ carritoCompra.textContent = contador
     │
     └─ Al hacer click en "Agregar al carrito"
  })

AGREGAR AL CARRITO: addToCart(productoId)
  ├─ carrito = JSON.parse(localStorage.getItem('carrito')) || []
  │  └─ Recupera carrito existente o crea vacío
  │
  ├─ producto = productos.find(p => p.id === productoId)
  │  └─ Encuentra producto por ID
  │
  ├─ carrito.push(producto)
  │  └─ Añade producto al array
  │
  ├─ localStorage.setItem('carrito', JSON.stringify(carrito))
  │  └─ Guarda carrito actualizado en navegador
  │
  └─ Toast.fire({ icon: 'success', ... })
     └─ Muestra notificación "Producto agregado al carrito"
```

### **PÁGINA DE CARRITO (cart.html + cart.js)**

```
INICIO DEL DOM
  ├─ document.addEventListener('DOMContentLoaded', () => {
  │  │
  │  ├─ cartContainer = document.querySelector('.cart-container')
  │  ├─ totalProductosElem = document.getElementById('total-productos')
  │  └─ totalPagarElem = document.getElementById('total-pagar')
  │
  └─ carrito = JSON.parse(localStorage.getItem('carrito')) || []
     └─ Carga carrito guardado

RENDERIZAR CARRITO
  ├─ totalProductos = 0
  ├─ totalPagar = 0
  │
  └─ carrito.forEach(producto => {
     ├─ createElement('div') - Crear tarjeta
     ├─ innerHTML con detalles y botón "Eliminar"
     ├─ appendChild(div) al contenedor
     ├─ totalProductos += 1
     └─ totalPagar += producto.precio
  })

MOSTRAR TOTALES
  ├─ totalProductosElem.textContent = totalProductos
  └─ totalPagarElem.textContent = totalPagar.toFixed(2)

ELIMINAR DEL CARRITO: removeFromCart(productoId)
  ├─ carrito = JSON.parse(localStorage.getItem('carrito')) || []
  │
  ├─ carrito = carrito.filter(producto => producto.id !== productoId)
  │  └─ Filtra el producto a eliminar
  │
  ├─ localStorage.setItem('carrito', JSON.stringify(carrito))
  │  └─ Guarda carrito actualizado
  │
  └─ MostrarNotificación()

NOTIFICACIÓN: MostrarNotificación()
  ├─ Swal.fire({
  │  ├─ Pregunta: "¿Está seguro?"
  │  ├─ Advertencia sobre eliminación
  │  └─ Botones: Eliminar / Cancelar
  │ })
  │
  ├─ if (result.confirmed)
  │  ├─ Muestra: "Producto eliminado"
  │  ├─ location.reload()
  │  └─ Recarga página para actualizar
  │
  └─ location.reload() - Actualiza página

FIN
```

---

## Estructura de Datos

### **stock.json - Productos**

```javascript
{
  "id": 1,                    // Identificador único
  "tipo": "Memoria MicroSD",  // Categoría
  "img": "../assets/...",     // Ruta de imagen
  "marca": "ADATA",           // Marca del producto
  "descripcion": "...",       // Descripción completa
  "capacidad": "32GB",        // Especificación técnica
  "precio": 200               // Precio en moneda local
}
```

### **localStorage - Carrito**

```javascript
// Formato guardado:
[
  {
    id: 1,
    tipo: "...",
    img: "...",
    marca: "...",
    descripcion: "...",
    capacidad: "...",
    precio: 200,
  },
  {
    id: 2,
    tipo: "...",
    img: "...",
    marca: "...",
    descripcion: "...",
    capacidad: "...",
    precio: 400,
  },
];
```

---

## Flujo General de Usuario

1. **Usuario visita productos.html**
   - Se ejecuta `pintarProductos()`
   - Los 9 productos se cargan desde JSON
   - Se muestran en tarjetas Bootstrap responsive

2. **Usuario hace click en "Agregar al carrito"**
   - Se ejecuta `addToCart(id)`
   - Producto se guarda en localStorage
   - Contador se incrementa
   - Se muestra notificación Toast

3. **Usuario navega a cart.html**
   - Se ejecuta el listener `DOMContentLoaded`
   - Se cargan todos los productos del carrito desde localStorage
   - Se calculan y muestran los totales

4. **Usuario hace click en "Eliminar"**
   - Se ejecuta `removeFromCart(id)`
   - Se muestra modal de confirmación
   - Si confirma, se elimina producto del localStorage
   - Se recarga la página para actualizar

---

## Características Técnicas

| Característica   | Descripción                                         |
| ---------------- | --------------------------------------------------- |
| **Persistencia** | localStorage mantiene carrito entre sesiones        |
| **Responsivo**   | Bootstrap col-xl-4 col-lg-6 adapta a dispositivos   |
| **Async/Await**  | Fetch + .then() para carga asincrónica de datos     |
| **Dinámica**     | Elementos creados desde JavaScript, no hardcodeados |
| **Validación**   | find() y filter() validan productos por ID          |
| **UX**           | SweetAlert proporciona notificaciones elegantes     |

---

## Archivos Implicados

- **app.js**: Lógica de productos y agregar al carrito
- **cart.js**: Lógica de visualización y eliminación del carrito
- **stock.json**: Base de datos de productos
- **productos.html**: Página de listado de productos
- **cart.html**: Página de visualización del carrito

---

## Conceptos Avanzados Aplicados

✅ **Asincronía**: Promesas con fetch y .then()
✅ **Persistencia**: localStorage para datos entre sesiones
✅ **Event Delegation**: Un listener captura eventos de múltiples botones
✅ **Métodos Funcionales**: map, filter, find, forEach
✅ **Template Literals**: Interpolación de datos en HTML
✅ **DOM Manipulation**: Creación y modificación dinámica
✅ **JSON**: Parseo y serialización de datos
✅ **Librerías**: Integración de SweetAlert
