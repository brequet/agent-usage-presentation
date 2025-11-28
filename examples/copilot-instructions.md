# APP Project - Copilot Instructions

> This is an example of a project copilot instructions file. For those kind of files, **size matters**: keep the file between 1,500 - 3,000 tokens. Use https://platform.openai.com/tokenizer[OpenAI's tokenizer] to estimate a text context size.

## Project Overview

APP is a Spring Boot-based Java application designed for deployment in production and laboratory environments. It is a
multi-module Maven project built with Java 21 and Spring Boot 4.0.0.

## Architecture & Tech Stack

### Core Technologies

- **Java Version:** 21
- **Spring Boot:** 4.0.0 (Spring Framework 6.x)
- **Build Tool:** Maven (multi-module project)
- **Server:** Embedded Tomcat
- **Default Port:** 16000
- **Context Path:** `/app`

### Modules Structure

```
app-parent/
├── api/           # API contracts and models (interfaces, DTOs, enums)
├── app/           # Main application implementation
├── docs/          # Antora-based documentation
└── updater/       # Windows installer and updater module
```

## Authentication & Security

APP uses a multi-layered security approach with **three distinct API layers**:

### API Endpoints

1. **`/api`** - CAS-protected API
    - Secured with CAS (Central Authentication Service) using APPL
    - Requires valid CAS authentication token
    - Used for standard authenticated operations
    - Backed by Skeeper REST in production

2. **`/api-local`** - Basic Auth, localhost only
    - HTTP Basic authentication
    - Only accessible from localhost
    - Used by other services (like APP Demonstrator) running on the same machine

3. **`/api-open`** - No authentication, localhost only
    - No authentication required
    - Only accessible from localhost
    - Limited to health and version endpoints: `/api-open/health`, `/api-open/version`

### Security Configuration

- **CAS:** Version 4.0.4 - APPL 1.1
- **Configuration Source:** file located at `%COMP_DATA%/common/conf/connection.properties`
- **Security Package:** `com.comp.app.core.controller.security`

### GUI Authentication

- Main GUI is CAS-protected
- Uses CSRF protection with SPA-specific token handling
- CSRF tokens stored in cookies for SPA consumption

## Code Quality & Static Analysis

### Enforced at Compilation

- **Error Prone:** Detects common bugs at compile time
- **NullAway:** Null safety analysis enforcing `@NonNull`/`@Nullable` contracts
    - Annotated packages: `com.comp`
    - Uses JSpecify annotations (`org.jspecify.annotations`)
- **Spotless + Google Java Format:** Code formatting enforced on every build
    - Style: GOOGLE
    - Runs in `validate` phase automatically

### Build Pipeline Tools

- **SonarQube/SonarLint:** Static analysis
- **BlackDuck:** Dependency vulnerability and license compliance scanning
- **Renovate:** Automated dependency updates (configured in `renovate.json`)

### Productivity Tools

- **Lombok:** Reduces boilerplate
    - Configuration in `lombok.config`

## API Documentation & OpenAPI

### SpringDoc OpenAPI Integration

- **Library:** `springdoc-openapi-starter-webmvc-ui`
- **Generation Approach:** Custom `OpenAPISpecGeneration` class in `api` module test sources
    - Runs during Maven `test` phase via `exec-maven-plugin`
    - Creates minimal Spring Boot context
    - Registers API interfaces as proxy beans
    - Generates OpenAPI spec directly to `api/target/openapi.json`

### API Contract Pattern

- **Contracts:** Defined as interfaces in `api` module (`com.comp.app.api.contract`)
    - Use `@HttpExchange` and `@GetExchange` annotations
    - Tagged with `@Tag` and `@Operation` for OpenAPI docs
- **Models:** Records and enums in `api` module
    - DTOs use record pattern when possible
    - Naming convention: `*RT` suffix for "Resource Type"
- **Implementation:** Controllers in `app` module implement API interfaces
    - Located in `com.comp.app.core.controller`

## Development Guidelines

### Coding Conventions

- **Null Safety:** Always use `@NonNull` and `@Nullable` annotations (from JSpecify) where appropriate
- **Records for DTOs:** Prefer Java records for immutable data transfer objects
- **Code Style:** Google Java Format (enforced by Spotless)
- **Controller Pattern:** Controllers should implement API interfaces from `api` module
- **Dependency Injection:** Constructor injection (via Lombok's `@RequiredArgsConstructor`)

### Configuration Properties

- Application properties in `application.yml`
- Server configuration:
    - Port: 16000
    - Context path: /app
- Forward headers strategy: `native` (for proxy support)

### Maven Build

#### Module-Specific Notes

- **api:** Generates OpenAPI spec during `test` phase
- **app:** Main Spring Boot application, uses spotless formatting
- **updater:** Creates Windows installer using InnoSetup

## Deployment & Environment

### Windows Service

- Service name: APP
- Service display: "APP"
- Java agent arguments configured for security keystore at `%COMP_DATA%\common\security\.keystore`
- Memory: `-Xmx1024m`

### Distribution

- Packaged as executable JAR
- Windows installer via InnoSetup
- Service wrapper using WinSW (version 2.12.0)

## Documentation

### Antora-Based Docs

- Documentation module: `docs/`
- Source: `docs/modules/ROOT/pages/`
- OpenAPI specs integrated into documentation: must be manually regenerated after API changes

## Best Practices for This Project

1. **Security First:** Always consider which API layer (`/api`, `/api-local`, `/api-open`) is appropriate
2. **API-First Design:** Define contracts in `api` module before implementation
3. **Null Safety:** Use JSpecify annotations consistently
4. **Type Safety:** Leverage Java 21 features (records, sealed classes, pattern matching)
5. **Documentation:** Keep OpenAPI annotations up-to-date on API interfaces
6. **Lombok Usage:** Use carefully to maintain readability

## Common Issues & Solutions

### Compilation Errors

- **NullAway errors:** Add appropriate `@NonNull` or `@Nullable` annotations
- **Java 21 exports:** Already configured in parent POM for Error Prone compatibility

### Formatting Issues

- Ensure your IDE is configured to use Google Java Format (must have the extension/plugin installed)

## IDE Setup Recommendations

1. Install Lombok plugin
2. Enable annotation processing
3. Configure Google Java Format plugin
4. Set Java 21 as project SDK
5. Install SonarLint for real-time code quality feedback
