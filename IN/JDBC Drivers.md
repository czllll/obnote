JDBC Drivers are client-side adapters that convert requests from Java programs to a protocol that the DBMS can understand. JDBC drivers implements interface in JDBC APIs to enable their capability.
There are 4 types of JDBC drivers. It acts as a middle-layer interface between Java application and databases.
## Types
### JDBC-ODBC bridge driver(Type-1)
* This kind of driver converts JDBC method calls into ODBC ones.
* abandon after java8
### Native-API driver(Type-2)
* Partially Java driver
* This driver converts JDBC method calls into native calls(JNI(Java Native Interface)) of the database API.
### Network Protocol driver(Type-3)
* Fully Java driver
* This kind of driver uses **middleware** that convert JDBC calls directly or indirectly into the vendor-specific database protocol.
* No client side, all the database connectivity drivers are present in a single server
### Thin driver(Type-4)
* This kind of driver is also called native protocol driver, interact directly with database. 
* Only need a driver JAR. No need any library.
