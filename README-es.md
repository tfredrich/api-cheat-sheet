[English](./README.md) | [简中版](./README-zh-Hans.md) | [Español](./README-es.md) 

### Consulta también: [Guía Rápida para Construcción de Plataformas](https://github.com/RestCheatSheet/platform-cheat-sheet#platform-building-cheat-sheet)

# Guía Rápida para Diseño de APIs

1. **Diseña la API pensando en los consumidores, como si fuera un producto en sí mismo.**  
   * No para una interfaz de usuario específica.  
   * Fomenta la flexibilidad y adaptabilidad de cada endpoint (ver puntos #5, #6 y #7).  
   * Usa tu propia API ("eat your own dogfood"), incluso si necesitas crear un prototipo de una interfaz de usuario de ejemplo.  

2. **Utiliza la metáfora de Colección.**  
   * Define dos URLs (endpoints) por recurso:  
     * La colección de recursos (por ejemplo, `/orders`).  
     * Un recurso individual dentro de la colección (por ejemplo, `/orders/{orderId}`).  
   * Usa formas plurales (por ejemplo, ‘orders’ en lugar de ‘order’).  
   * Alterna nombres de recursos con IDs como nodos en la URL (por ejemplo, `/orders/{orderId}/items/{itemId}`).  
   * Mantén las URLs lo más cortas posible. Preferiblemente, no más de tres nodos por URL.  

3. **Usa sustantivos como nombres de recursos** (por ejemplo, no uses verbos en las URLs).  

4. **Haz que las representaciones de los recursos sean significativas.**  
   * “¡No IDs desnudos!” No incluyas IDs simples en las respuestas. Usa enlaces y objetos de referencia.  
   * Diseña representaciones de recursos. No te limites a representar tablas de la base de datos.  
   * Combina representaciones. No expongas tablas de relaciones como dos IDs separados.  

5. **Admite filtrado, ordenamiento y paginación en las colecciones.**  

6. **Admite la expansión de enlaces en relaciones.** Permite que los clientes expandan los datos incluidos en la respuesta mediante representaciones adicionales, además de, o en lugar de, enlaces.  

7. **Admite proyecciones de campos en recursos.** Permite a los clientes reducir la cantidad de campos devueltos en la respuesta.  

8. **Usa los nombres de los métodos HTTP con significado:**  
   * `POST`: para crear y otras operaciones no idempotentes.  
   * `PUT`: para actualizar.  
   * `GET`: para leer un recurso o colección.  
   * `DELETE`: para eliminar un recurso o colección.  

9. **Usa códigos de estado HTTP con significado:**  
   * `200`: Éxito.  
   * `201`: Creado. Devuelve este código al crear un recurso nuevo exitosamente. Incluye un encabezado 'Location' con un enlace al recurso recién creado.  
   * `400`: Solicitud incorrecta. Por ejemplo, problemas con los datos como JSON no válido.  
   * `404`: No encontrado. Recurso no encontrado al usar `GET`.  
   * `409`: Conflicto. Datos duplicados o estado de datos no válido.  

10. **Usa formatos de fecha en representaciones conforme a ISO 8601.**  

11. **Considera la conectividad utilizando una estrategia de enlace.** Algunos ejemplos populares son:  
   * [HAL](http://stateless.co/hal_specification.html)  
   * [Siren](https://github.com/kevinswiber/siren)  
   * [JSON-LD](http://json-ld.org/)  
   * [Collection+JSON](http://amundsen.com/media-types/collection/)  

12. **Usa [OAuth2](http://oauth.net/2/) para proteger tu API.**  
   * Usa un token Bearer para autenticación.  
   * Requiere HTTPS / TLS / SSL para acceder a tus APIs. Los tokens Bearer de OAuth2 lo exigen. La comunicación no encriptada a través de HTTP permite interceptaciones e impersonaciones simples.  

13. **Usa la negociación de Content-Type para describir los payloads de las solicitudes.**  

    Por ejemplo, si manejas calificaciones (como "pulgar arriba/abajo" y calificaciones de cinco estrellas), puedes usar una única ruta para crear una calificación: **POST /ratings**.  
    En lugar de crear rutas separadas como **POST /ratings/five_star** o **POST /ratings/thumbs_up**, usa el encabezado `Content-Type` para distinguir los tipos de calificación:  
    * `Content-Type: application/vnd.company.rating.thumbsup`  
    * `Content-Type: application/vnd.company.rating.fivestar`  

14. **Evolución sobre versionado.** Sin embargo, si necesitas versionar, usa el encabezado `Accept` en lugar de versionar en la URL.  
   * El versionado en la URL representa una versión "plataforma".  
   * El versionado mediante el encabezado `Accept` versiona el recurso individual.  
   * Los cambios en respuestas JSON no requieren versionado, pero los cambios en solicitudes que sean obligatorios sí podrían necesitarlo.  

15. **Considera la capacidad de caché.** Usa los siguientes encabezados de respuesta:  
   * `ETag`: Cadena arbitraria para la versión de una representación.  
   * `Date`: Fecha y hora en que se devolvió la respuesta.  
   * `Cache-Control`: Controla la edad máxima o si la respuesta no se debe almacenar en caché.  
   * `Expires`: Fecha en que expira la respuesta.  
   * `Pragma`: Indica "no-cache" si se requiere.  
   * `Last-Modified`: Fecha y hora de la última modificación del recurso.  

16. **Asegúrate de que las operaciones `GET`, `PUT` y `DELETE` sean [idempotentes](http://www.restapitutorial.com/lessons/idempotency.html).** No debe haber efectos secundarios adversos de estas operaciones.  