---
name: senior-flutter
description: Desarrolla apps Flutter con Clean Architecture, BLoC/Cubit, UX consistente, seguridad móvil y entornos reproducibles entre equipos.
---

# Senior Flutter

Desarrolla aplicaciones Flutter mantenibles y seguras con Clean Architecture, BLoC/Cubit y una UX coherente. Mantén la arquitectura existente salvo una razón técnica documentada para cambiarla.

## Arquitectura y estado

Organiza cada feature en `data`, `domain` y `presentation`; las dependencias apuntan al dominio:

```text
Widget → BLoC/Cubit → Use case → Repository → Data source → API/almacenamiento
```

- Los widgets renderizan y disparan eventos; no contienen lógica de negocio.
- Usa `Cubit` para flujos simples y BLoC para flujos complejos por eventos.
- Modela estados explícitos: `Initial`, `Loading`, `Success`, `Empty` y `Error`; añade `Offline`, `Refreshing` o paginación cuando sean necesarios.
- Usa `BlocListener` para efectos secundarios y `BlocBuilder` para renderizado.
- Inyecta dependencias; no instancies repositorios o casos de uso dentro de widgets.

## UX y calidad

- Diseña cada pantalla para estados de carga, contenido, vacío y error, con una acción de recuperación cuando corresponda.
- Centraliza color, tipografía, espacios y componentes en un design system.
- Usa modelos y estados inmutables, `const` cuando aplique y claves en listas.
- Ejecuta `flutter analyze`, `flutter test` y el build pertinente antes de cerrar una tarea. Prueba BLoCs/Cubits, casos de uso y estados de éxito, error y vacío.

## Seguridad móvil

- No incluyas secretos, URLs privadas, keystores ni tokens en el repositorio o código. Usa configuración por entorno y almacenamiento seguro para credenciales o tokens de sesión (`flutter_secure_storage` u opción equivalente aprobada).
- Revisa autenticación, expiración/renovación de sesión, autorización en el servidor, validación de respuestas y manejo de errores sin filtrar datos.
- Solicita sólo permisos móviles necesarios y explica su propósito en cada plataforma. Valida deep links, URLs y datos recibidos de otras apps.
- Mantén claves de firma y perfiles de distribución fuera de Git. Controla la firma de Android/iOS desde el entorno seguro de CI o el gestor de secretos.
- Evalúa pinning de certificados y ofuscación sólo cuando el riesgo, producto y operación lo justifiquen; no los agregues como una falsa garantía.
- Revisa dependencias y actualizaciones vulnerables como parte de CI cuando haya una herramienta aprobada.

## Reproducibilidad: macOS y Windows

Fija la versión de Flutter con FVM (o una herramienta equivalente) y conserva `pubspec.lock` para aplicaciones. Documenta los comandos de desarrollo:

```text
fvm flutter pub get
fvm flutter analyze
fvm flutter test
```

Comprueba `flutter doctor` en cada máquina. Android requiere SDK/Java adecuados; iOS requiere macOS, Xcode y firmas válidas.

## Docker

No fuerces Docker para desarrollar una app Flutter: no sustituye Xcode, simuladores iOS, certificados ni la firma de iOS. Puede ser útil para:

- backend, base de datos y servicios locales que consume la app;
- herramientas de desarrollo compartidas;
- CI Linux/Android cuando la imagen esté fijada y mantenida.

Si se usan contenedores para esos servicios, aplica `compose.yaml`, imágenes versionadas, `.dockerignore`, usuarios no-root, secretos fuera de la imagen y documentación de arranque. El código Flutter se ejecuta y prueba con el SDK nativo del equipo salvo una necesidad concreta de CI.

## Git, SDD y Notion

- Usa ramas por feature/fix, commits descriptivos y PRs según las reglas del repositorio.
- Antes de crear un proyecto pregunta si es **Personal** o de **Trabajo**. Usa el SDD para requisitos y tareas verificables.
- Registra el proyecto y sus tareas en **Proyectos personales** o **Proyectos trabajo** de Notion. Enlaza SDD, repositorio, pruebas y PR; no uses la antigua página `Template - Proyectos` como plantilla para proyectos nuevos.
- Si está disponible `software-project-operations`, úsala para crear repositorio privado y coordinar el registro inicial de Notion.

## Informe final

Indica pantallas/arquitectura modificadas, estados cubiertos, pruebas y análisis ejecutados, consideraciones de seguridad, versión Flutter/FVM, servicios Docker usados si existen, commit/PR y pendientes.
