# archetype-ms-tertulia

Arquetipo **Maven** para microservicios del sistema **Tertulia** (foro en línea).

Genera un esqueleto Spring Boot listo para conectar a Oracle Autonomous DB y validar JWT emitidos por `ms-usuarios`.

## Qué incluye

- Capas: `controller` → `service` → `repository` → `model` + `security`
- Security JWT (filtro Bearer) y CORS
- Dependencias JPA + driver Oracle (wallet)
- Endpoint público `GET /health`

Los microservicios del sistema (`ms-usuarios`, `ms-foro`, `ms-moderacion`) comparten esta misma estructura de capas. Los nuevos se generan con este arquetipo; los ya existentes quedan alineados al mismo diseño.

## Requisitos

- JDK 25
- Maven 3.9+

## Instalar el arquetipo

Desde la raíz de este repositorio:

```bash
mvn clean install
```

Queda disponible en el repositorio Maven local (`~/.m2`) como:

- `groupId`: `com.example`
- `artifactId`: `archetype-ms-tertulia`
- `version`: `1.0.0-SNAPSHOT`

## Generar un microservicio

Desde el directorio donde quieras crear el proyecto:

```bash
mvn archetype:generate \
  -DarchetypeGroupId=com.example \
  -DarchetypeArtifactId=archetype-ms-tertulia \
  -DarchetypeVersion=1.0.0-SNAPSHOT \
  -DgroupId=com.example \
  -DartifactId=ms-ejemplo \
  -Dversion=0.0.1-SNAPSHOT \
  -Dpackage=com.example.ejemplo \
  -DserverPort=8080 \
  -DappDescription="Descripción del microservicio" \
  -DinteractiveMode=false
```

Propiedades relevantes:

| Propiedad | Uso |
|-----------|-----|
| `artifactId` | Nombre del módulo / `spring.application.name` |
| `package` | Paquete base Java |
| `serverPort` | Puerto HTTP del servicio |
| `appDescription` | Texto del README y del `pom` |

### Después de generar

1. Entra al directorio del artefacto generado.
2. Copia la plantilla de configuración:
   ```bash
   cp src/main/resources/application.properties.example src/main/resources/application.properties
   ```
3. Ajusta wallet Oracle, credenciales y `jwt.secret` (debe ser el mismo en todos los MS).
4. (Opcional) Agrega Maven Wrapper si el resto de servicios del monorepo ya lo usan.
5. Implementa el dominio en `model` / `repository` / `service` / `controller` y afina `SecurityConfig` según los endpoints públicos o de rol.

## Ejecutar el servicio generado

```bash
mvn spring-boot:run
```

Comprobación rápida: `GET http://localhost:<serverPort>/health`

## Relación con el resto del sistema

| Servicio | Puerto típico | Responsabilidad |
|----------|---------------|-----------------|
| `ms-usuarios` | 8081 | Login / registro / usuarios (emite JWT) |
| `ms-foro` | 8082 | Categorías, temas y comentarios |
| `ms-moderacion` | 8083 | Banear / desbanear (ADMIN) |

Todos comparten schema Oracle `FORO_APP` y el mismo `jwt.secret`.
