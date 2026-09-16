# Decisiones de diseño

- Se crea la clase `RabbitMQConnectionManager` para centralizar la conexión, reutilización y cierre de conexiones.
- La conexión se intenta hasta diez veces. Los errores de RabbitMQ se traducen a excepciones propias del middleware.
- Las colas y exchanges se declaran con `durable=True` para conservar su declaración ante reinicios del broker y con `auto_delete=True` para limpiar recursos que dejan de utilizarse.
- En los exchanges directos, cada consumidor crea una cola con nombre único, la enlaza a sus `routing_keys` y recibe solamente los mensajes correspondientes.
- Se usa `basic_qos(prefetch_count=1)` junto con `auto_ack=False` para que cada consumidor procese un mensaje antes de recibir otro sin confirmar y hacer que la distribucion de cargas sea mas balanceada.
