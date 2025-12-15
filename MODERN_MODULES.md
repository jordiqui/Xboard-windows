# Propuestas de integración de módulos modernos

Este documento resume módulos y librerías contemporáneas que podrían integrarse para modernizar el proyecto y facilitar su mantenimiento.

## Interfaz gráfica
- **GTK 4**: migrar gradualmente la capa GTK existente para aprovechar mejoras en rendimiento y accesibilidad. Empezar por componentes aislados (p. ej. diálogos) y usar `libadwaita` para estilos coherentes.
- **Soporte de temas SVG**: reutilizar la carpeta `svg/` con renderizado mediante `librsvg` para escalado nítido en pantallas HiDPI.

## Audio y notificaciones
- **SDL2 (audio)**: encapsular la reproducción de sonidos en un módulo pequeño usando `SDL_AudioDeviceID` para mejorar compatibilidad multiplataforma.
- **libnotify**: en entornos compatibles, mostrar notificaciones de eventos (movidas, fin de partida) con un backend opcional.

## Configuración y datos
- **libyaml** o **jansson**: manejar configuraciones y listas de partidas en formatos legibles (YAML/JSON) para interoperabilidad con herramientas externas.
- **SQLite**: almacenar históricos de partidas y estadísticas en un backend opcional para consultas rápidas; envolverlo detrás de una API para mantener independencia del núcleo.

## Plug-ins y extensibilidad
- Definir una interfaz de plug-ins basada en `dlopen`/`LoadLibrary` para módulos de entrada/salida (p. ej. nuevos protocolos de motor) sin recompilar el núcleo.
- Documentar la ABI y proporcionar ejemplos en `examples/plugins/` para facilitar contribuciones.

## Observabilidad y depuración
- **fmt / printf-style wrappers**: centralizar el formateo de logs para evitar inconsistencias en `printf` dispersos.
- **tracing opcional** con `LTTng` o `ETW` (Windows): permitir perfiles ligeros en builds de desarrollo sin penalizar la distribución.

## Calidad y automatización
- **CI moderna (GitHub Actions/GitLab CI)**: añadir flujos que compilen en Linux/Windows/macOS, ejecuten pruebas, y analicen traducciones (`po/`).
- **clang-format y clang-tidy**: establecer reglas de estilo y análisis estático para evitar regresiones antes del merge.

## Estrategia de adopción
1. Elegir un módulo piloto (p. ej. audio con SDL2) y crear una capa de abstracción pequeña.
2. Añadir comprobaciones de disponibilidad en `configure` o scripts equivalentes para mantener compatibilidad con entornos antiguos.
3. Incorporar pruebas básicas o harnesses de integración para cada módulo nuevo.
4. Documentar las rutas de activación/desactivación en `README` y `configure --help`.

Estas propuestas pueden implementarse de forma incremental, manteniendo la compatibilidad con la base de código actual y ofreciendo mejoras visibles para usuarios y desarrolladores.
