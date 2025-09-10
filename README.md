Arquitectura de Microservicios para Gestión de Ventas
Este desarrollo consiste en una plataforma de ventas descentralizada construida con Spring Boot, compuesta por microservicios independientes para administrar productos, transacciones de venta, registros contables, orquestación de procesos y un Gateway unificado. La interacción entre los servicios se realiza mediante APIs REST, y todos están integrados con un servidor de descubrimiento (Eureka).

Componentes del Sistema
Servicio	Funcionalidad Principal
warehouse-service	Control de inventario y productos
accounting-service	Gestión de entradas contables
sale-service	Procesamiento de ventas
gateway-service	Punto único de entrada para APIs
orchestrator-service	Coordinación de flujos de venta
discovery-service	Registro y localización de servicios (Eureka)
Configuración de Bases de Datos
Cada microservicio utiliza PostgreSQL o MySQL según su contexto:

Ventas: jdbc:postgresql://localhost:15432/sales

Contabilidad: jdbc:postgresql://localhost:15432/accounting

Inventario: jdbc:mysql://localhost:13306/warehouse

Despliegue de Servicios
Para iniciar la plataforma, ejecutar en orden:

discovery-service

warehouse-service

accounting-service

sale-service

orchestrator-service

gateway-service

Comando por servicio:

bash
cd <directorio-del-servicio>  
mvn spring-boot:run -X
Pruebas y Endpoints
Utilizar el archivo test-sales.http para realizar pruebas.

Gateway: http://localhost:8082

Orquestador Saga: http://localhost:8082/ms-orch/saga/sale

Eureka Console: http://localhost:8761

Ejemplo de solicitud para nueva venta (POST):

http
POST http://localhost:8082/ms-orch/saga/sale
Content-Type: application/json
Accept: application/json

{
  "saleNumber": "SALE-0026",  # Modificar en cada prueba
  "productId": 5,
  "quantity": 1,
  "unitPrice": 1.99,  # Valor menor a 1.00 activa trigger
  ... (resto de campos)
}
Aspectos Clave
Las transacciones de venta se almacenan en la base de datos sales.

Los servicios actualizan automáticamente inventario y contabilidad mediante APIs REST.

El orquestador garantiza consistencia en ventas, stock y registros financieros.

Eureka permite la detección dinámica de servicios.

