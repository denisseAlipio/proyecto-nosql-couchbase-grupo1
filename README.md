# Proyecto NoSQL Grupo 1 — Gestión de Pedidos (Couchbase)

Proyecto universitario: sistema administrativo de pedidos y transacciones para comercio electrónico, con **Couchbase Server Community 7.6.1** como motor documental / clave-valor.

## Avance verificado en la laptop de desarrollo (23/11/2026)
- Docker Desktop ejecuta un contenedor `couchbase-grupo1` con Couchbase Server 7.6.1.
- Se configuró un clúster local de **1 nodo** con servicios Data, Index y Query.
- Bucket `ecommerce_db` (256 MiB; **réplicas deshabilitadas temporalmente**).
- Scope `gestion_pedidos` y colecciones `clientes`, `productos`, `pedidos`.
- Se introdujeron manualmente 4 documentos de prueba: 1 cliente, 2 productos y 1 pedido.
- La consulta SQL++ mediante `USE KEYS` se documenta como ejercicio; la validación detallada de los filtros e índices está **pendiente**.

> IMPORTANTE: este repositorio guarda **archivos para reproducir** el avance; NO contiene la base de datos real, los volúmenes Docker ni un backup del clúster. Los JSON se reconstruyeron a partir de los datos usados en la sesión de trabajo, no son una exportación directa de Couchbase.

## Organización
- `datos/`: documentos JSON de ejemplo y sus claves.
- `sql/`: consulta de prueba por ID; las consultas adicionales se agregarán conforme se comprueben.
- `documentacion/`: instrucciones de instalación y ubicación de capturas.
- `docker-compose.yml`: contenedor de un nodo (para máquinas donde NO exista ya otro contenedor con el mismo nombre).

## Preparación en una computadora nueva
1. Instalar Docker Desktop y habilitar la virtualización/WSL2 si se utiliza Windows.
2. Abrir una terminal **en la carpeta del repositorio** y ejecutar `docker compose up -d`.
3. Abrir `http://localhost:8091` y elegir **Setup New Cluster**. Crear una **contraseña propia** y NO subirla al repositorio.
4. Activar servicios **Data, Index y Query**; desactivar Search si la memoria es limitada. Ajustar las cuotas de memoria según los requisitos de Couchbase y los recursos de cada equipo (en la instalación inicial se usó 1024 MiB Data + 256 MiB Index).
5. En **Buckets**, crear `ecommerce_db`, tipo Couchbase, 256 MiB de cuota y con réplicas deshabilitadas mientras haya un único nodo.
6. En **Scopes & Collections**, crear `gestion_pedidos` y las colecciones `clientes`, `productos`, `pedidos`.
7. En la sección **Documents** de cada colección, elegir **Add Document** y pegar los contenidos de `datos/` usando estas claves:

| Colección | Document ID | Archivo |
|---|---|---|
| clientes | `cliente::CLI-001` | `datos/cliente_CLI-001.json` |
| productos | `producto::PROD-001` | `datos/producto_PROD-001.json` |
| productos | `producto::PROD-002` | `datos/producto_PROD-002.json` |
| pedidos | `pedido::ORD-001` | `datos/pedido_ORD-001.json` |

8. Probar la consulta de `sql/01_consulta_por_clave.sqlpp` desde **Query** de Couchbase.

### Para quien ya tenga el contenedor original
No ejecutar `docker compose up -d` sobre la misma máquina sin revisar conflictos: el contenedor `couchbase-grupo1` ya existe. Para continuar el trabajo previo, abrir Docker Desktop y usar `docker start couchbase-grupo1`.

## Pendiente para el equipo
- Consultas SQL++ por cliente, fecha y estado; índices GSI y medición de latencias.
- Backend API, lógica de negocio y frontend.
- Prueba con dos nodos, replicación y failover si el entorno lo permite.
- Pruebas con volumen de datos y documento técnico final.

Informe técnico de trabajo (Google Docs, requiere permisos de acceso otorgados por la propietaria):
https://docs.google.com/document/d/1mPGRGYT35EhXcpQeJiJSocS9FQdd3B5nwZmqSSTlLy0/edit

**Nunca publicar contraseñas ni subir el contenido del volumen de Docker.**
