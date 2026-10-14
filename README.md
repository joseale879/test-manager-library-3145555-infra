# test-manager-library-3145555-infra

> **Prueba técnica · Ficha ADSO 3145555**
> Horario: **miércoles 14 de octubre de 2026, de 6:50 a. m. a 11:00 a. m.** (hora Colombia). Entrega a las **11:00 a. m.**

Este repositorio es la **infraestructura** de *Manager Library*: contenedores, red, volúmenes y el motor de base de datos vacío. **Empieza vacío a propósito**: se construye en el fork de cada aprendiz.

## Instrucciones

Cada aprendiz debe **crear el fork** de los seis repositorios del proyecto y **resolver el proyecto
con el spec planteado**.

1. Hacer fork, a su cuenta de GitHub, de cada repositorio de la tabla del final.
2. Leer el spec en [`test-manager-library-3145555-docs`](https://github.com/code-sena/test-manager-library-3145555-docs).
   Las versiones de referencia son `spec-python/` y `spec-.net/`; la traducción tecnológica del reto está en `spec-laravel-php/`.
3. Desarrollar en los forks.

## El reto se desarrolla con React y PHP (Laravel)

El spec está escrito para Python y para .NET, pero el reto **no** se hace en esos lenguajes:

| Capa | Tecnología del reto |
|---|---|
| Frontend | React |
| Backend | PHP con Laravel |

Lo que el spec define sobre el negocio —historias, criterios de aceptación, reglas, contrato de la
API, modelo de datos— se respeta. Lo que define sobre la tecnología se traduce a React y Laravel.

## La prueba no consiste en escribir el código

El propósito principal es ver la **capacidad de desempeño con SDD** (*Spec-Driven Development*,
desarrollo guiado por especificación): cómo se lee, se interpreta y se aplica una especificación
para llevarla a un stack distinto. El código es el medio, no el fin.

## Entrega

Cuenta lo que quede **subido a los forks antes de las 11:00 a. m.** Lo que llegue después no se revisa.
No hay prórroga: suban avances parciales con commits pequeños durante la prueba.

## Los seis repositorios

| Repositorio | Qué va ahí |
|---|---|
| [`test-manager-library-3145555-docs`](https://github.com/code-sena/test-manager-library-3145555-docs) | El spec: `spec-python/`, `spec-.net/` y `spec-laravel-php/` |
| [`test-manager-library-3145555-api`](https://github.com/code-sena/test-manager-library-3145555-api) | Backend en PHP (Laravel) |
| [`test-manager-library-3145555-app`](https://github.com/code-sena/test-manager-library-3145555-app) | Frontend en React |
| [`test-manager-library-3145555-page`](https://github.com/code-sena/test-manager-library-3145555-page) | Sitio público estático de presentación |
| [`test-manager-library-3145555-infra`](https://github.com/code-sena/test-manager-library-3145555-infra) | Contenedores, red, volúmenes y motor de base de datos vacío |
| [`test-manager-library-3145555-tool`](https://github.com/code-sena/test-manager-library-3145555-tool) | Utilidades: sembrador de datos de demostración |

## HU-INFRA-01 — Run the local environment with Docker Compose

**As a** developer, **I want** to start PostgreSQL, the Laravel API, and the React application with one command, **so that** I can run and verify the documented system locally.

### Acceptance criteria

- `docker compose up --build` starts PostgreSQL 16, the API, and the frontend.
- PostgreSQL starts with an empty `library` schema; migrations are not run automatically.
- Database tables, indexes, constraints, and category seed data are owned by Laravel migrations in the `api` repository. This repository contains no DDL or database initialization scripts.
- The API waits for PostgreSQL readiness before starting.
- The frontend reaches the API through the documented local configuration.
- Persistent database storage uses a named Docker volume.
- Runtime secrets and local overrides are provided through an ignored `.env` file; only `.env.example` is committed.
- The demo-data tool is run separately and uses the API; Compose does not seed application records.

### Dependency and implementation order

This story depends on task T-17: the API must already have its documented startup behavior. The API and React repositories are currently empty apart from their READMEs, so this Compose story cannot be verified until their required application artifacts exist. Implement the API tasks in the order specified by `spec-laravel-php/tasks.md`, then complete this story without changing product scope.

### Verification

1. Run `docker compose config` to validate the Compose configuration.
2. Run `docker compose up --build` and confirm all three services become ready.
3. Confirm the API health endpoint responds successfully.
4. Confirm the `library` schema has no application tables before Laravel migrations are run.
5. Confirm no migration or demo-data command ran as a side effect of starting Compose.
