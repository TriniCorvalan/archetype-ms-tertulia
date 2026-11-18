# ${artifactId}

${appDescription}

Generado con el arquetipo Maven **`archetype-ms-tertulia`** (sistema de foros Tertulia).

- Puerto por defecto: **${serverPort}**
- Stack: Spring Boot 4, JPA, Security (valida JWT de `ms-usuarios`), Oracle Cloud

## Estructura de capas

| Capa | Paquete |
|------|---------|
| Controlador | `controller` |
| Servicio | `service` |
| Repositorio | `repository` |
| Modelo | `model` |
| Seguridad | `security` (JWT + CORS) |

## Configuración

1. Colocá el wallet de Oracle Autonomous en una ruta **sin espacios** (o un symlink), por ejemplo `~/oracle_wallets/foro`.
2. Creá las propiedades locales a partir de la plantilla (no se versiona `application.properties`):
   ```bash
   cp src/main/resources/application.properties.example src/main/resources/application.properties
   ```
3. Usá el mismo `jwt.secret` que el resto de microservicios del sistema.

## Ejecutar

```bash
mvn spring-boot:run
```

Si el proyecto incluye Maven Wrapper:

```bash
./mvnw spring-boot:run
```

Health: `GET http://localhost:${serverPort}/health`

## Tests

```bash
mvn test
```
