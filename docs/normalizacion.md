## Normalización hasta 3NF
Todos los requisitos son una copia de la guía para el TC

---

### Primera Forma Normal (1NF)

**Requisitos:**
- Todos los atributos deben ser atómicos (no listas ni valores múltiples)
- No debe haber grupos repetidos

**¿Cumple nuestro modelo?** ✅ Sí
- Cada campo almacena un único valor: `email` contiene un email, `price` contiene un número, `status` contiene un estado.
- Los productos de un pedido **no** se almacenan como una lista en `orders`. En cambio, cada producto tiene su propia fila en `order_items`. Esto elimina los grupos repetidos.
- Las fechas se almacenan por separado (`ordered_at`, `shipped_at`, `delivered_at`) en lugar de en un campo tipo `dates: [fecha1, fecha2]`.

---

### Segunda Forma Normal (2NF)

**Requisitos:**
- Debe cumplir 1NF
- No debe haber dependencias parciales (todos los atributos no-clave deben depender de la clave primaria completa, no solo de parte de ella)
- Esto es especialmente relevante en tablas con clave compuesta

**¿Cumple nuestro modelo?** ✅ Sí
Las tablas creadas usan PKs simples (`order_item_id`, `order_id`, etc.), por lo que no hay dependencias parciales.

El ejemplo más claro es la tabla `order_items`. Aunque solo sirve para unir pedidos con productos, le pusimos su propio ID único (`order_item_id`). Así, todo lo que pasa en esa línea queda bien registrado:
- La cantidad (`quantity`): No depende solo del pedido o del producto por separado, sino de esa mezcla específica (es decir, cuántas unidades de ese producto se llevaron en ese pedido).
- El precio (`unit_price`): Tampoco depende solo del producto, porque el precio puede cambiar mañana. Lo guardamos aquí para que quede grabado el precio exacto al que se vendió en ese momento.

---

### Tercera Forma Normal (3NF)

**Requisitos:**
- Debe cumplir 2NF
- No debe haber dependencias transitivas (ningún atributo no-clave puede depender de otro atributo no-clave)

**¿Cumple nuestro modelo?** ✅ Sí
Todos los atributos de cada tabla dependen directamente de la PK, sin pasar por otro atributo no-clave.

---

**Ejemplos de decisiones a justificar:**

*¿Por qué `unit_price` está en `order_items` y no se lee de `products.price`?*
Porque el precio de un producto puede cambiar. Si leyéramos de `products.price`, como indicamos en la justificación de requisitos 2NF, perderíamos el precio histórico de la venta. `unit_price` depende directamente del `order_item_id`, no de `product_id`. 


¿Por qué `country` está directamente en `customers` y no es una tabla countries separada?
Porque, por ahora, solo nos interesa saber el nombre del país.
Si estuviéramos guardando más cosas de cada país (como el idioma, etc..), lo correcto sería crear una tabla aparte llamada countries. Como solo guardamos el nombre, no vale la pena crear una tabla extra solo para una palabra. 

Si en `orders` almacenásemos el nombre del cliente (`customer_name`) además de `customer_id`, ¿qué forma normal se violaría y por qué?
Si pusiéramos el nombre del cliente en la tabla de pedidos, estaríamos rompiendo la 3NF. 
El problema es que el nombre de una persona no tiene nada que ver con el número de pedido, sino con el cliente en sí. Estarías creando un 'paso extra' innecesario: el pedido te lleva al cliente, y el cliente te da el nombre. Eso es la dependencia transitiva: si el cliente cambia su nombre, tendrías que ir pedido por pedido actualizándolo (lo que no tendría ningun sentido). Es una posible complicación que evitamos dejando el nombre solo en la tabla de clientes.
