# ContableWeb – Documentación del Frontend

## 1. Resumen del proyecto
ContableWeb es una aplicación web construida con Angular destinada a servir como base para el módulo frontend de una solución contable. El proyecto parte del esqueleto generado con Angular CLI 20 e incluye la configuración mínima para gestionar el enrutamiento, el manejo global de errores del navegador y la coalescencia de eventos de zona. Actualmente incorpora la plantilla de bienvenida de Angular, lista para ser reemplazada por la interfaz propia del dominio contable.

## 2. Tecnologías clave
- **Framework:** Angular 20 (standalone components y Angular Router).
- **Lenguaje:** TypeScript 5.9.
- **Gestión de paquetes:** npm (Node.js 18+ recomendado).
- **Herramientas de construcción y desarrollo:** Angular CLI, @angular/build.
- **Testing:** Karma + Jasmine.
- **Utilidades adicionales:** Zone.js, RxJS y configuración de formateo con Prettier.

## 3. Requisitos previos
1. **Node.js:** versión 18 LTS o superior (Angular 20 requiere Node 18.13+).
2. **npm:** incluido con la instalación de Node.
3. **Angular CLI (opcional):** instalar globalmente (`npm install -g @angular/cli`) para disponer de los comandos `ng` fuera de los scripts npm.
4. **Navegador moderno:** Chrome, Edge o Firefox para ejecutar el cliente en local.

## 4. Puesta en marcha rápida
```bash
# Clonar el repositorio
git clone <url-del-repositorio>
cd frontdocumenta

# Instalar dependencias
npm install

# Levantar el servidor de desarrollo en http://localhost:4200/
npm start
```
Angular CLI recompilará y recargará la aplicación automáticamente al detectar cambios en los archivos fuente.

## 5. Scripts disponibles
| Comando | Descripción |
| --- | --- |
| `npm start` | Ejecuta `ng serve` con configuración de desarrollo y habilita recarga en caliente. |
| `npm run build` | Genera un build optimizado en `dist/contable-web/` listo para despliegue. |
| `npm run watch` | Realiza compilaciones incrementales en modo observación (`ng build --watch`). |
| `npm test` | Lanza los tests unitarios con Karma + Jasmine en un navegador Chrome controlado. |
| `npm run ng -- <comando>` | Permite invocar cualquier comando de Angular CLI directamente. |

## 6. Estructura del proyecto
```
frontdocumenta/
├── angular.json             # Configuración de Angular CLI (build, serve, test, assets)
├── package.json             # Dependencias, scripts y reglas de formateo
├── src/
│   ├── main.ts              # Punto de entrada; arranca el componente raíz
│   ├── index.html           # Documento HTML principal
│   ├── styles.css           # Estilos globales compartidos
│   └── app/
│       ├── app.ts           # Componente raíz standalone (selector <app-root>)
│       ├── app.html         # Plantilla HTML actual (placeholder Angular)
│       ├── app.css          # Estilos específicos del componente raíz
│       ├── app.routes.ts    # Definición de rutas de Angular Router
│       ├── app.config.ts    # Configuración global (router, listeners de error, zone)
│       └── app.spec.ts      # Prueba unitaria base generada por Angular CLI
├── public/                  # Recursos estáticos copiados sin procesar al build
└── tsconfig*.json           # Configuraciones de TypeScript para app y tests
```
_Añade nuevos módulos dentro de `src/app/` siguiendo una organización por dominios o features para mantener la escalabilidad._

## 7. Arquitectura y flujo principal
1. **Punto de entrada (`main.ts`):** bootstrap con `bootstrapApplication` que registra el componente raíz (`App`).
2. **Configuración (`app.config.ts`):**
   - `provideBrowserGlobalErrorListeners()` habilita trazas y captura de errores a nivel global.
   - `provideZoneChangeDetection({ eventCoalescing: true })` reduce el número de ciclos de detección de cambios agrupando eventos.
   - `provideRouter(routes)` inicializa el enrutador con la lista de rutas definida en `app.routes.ts` (actualmente vacía).
3. **Componente raíz (`app.ts` y `app.html`):** es un componente standalone que expone una señal `title`. Su plantilla contiene el contenido de bienvenida que puede sustituirse por vistas propias.
4. **Routing:** `routes` se encuentra vacío. Para añadir páginas crea archivos de ruta (lazy-loaded si es necesario) y actualiza la constante exportada en `app.routes.ts`.

## 8. Estilos y convenciones de código
- **Prettier:** el proyecto incluye configuración integrada en `package.json` (anchura 100, comillas simples, parser Angular para HTML). Se recomienda ejecutar `npx prettier --write .` antes de crear un commit.
- **CSS global vs. por componente:** utiliza `styles.css` para estilos compartidos y `*.css` junto a cada componente para estilos encapsulados.
- **TypeScript:** aprovecha `strict` por defecto de Angular para escribir código tipado y seguro.
- **Estructura por features:** agrupa componentes, servicios y rutas por dominio funcional (`src/app/<feature>/`).
- **Convenciones de nombre:** utiliza `kebab-case` para archivos (ej. `balance-general.component.ts`) y `UpperCamelCase` para clases.

## 9. Pruebas y calidad
- **Unit tests:** ejecuta `npm test`. Karma abrirá Chrome en modo headless para correr las pruebas. Modifica `app.spec.ts` y añade specs adicionales dentro de la carpeta de cada feature.
- **Cobertura:** agrega la bandera `--code-coverage` (`ng test --code-coverage`) para generar reportes en `coverage/`.
- **Pruebas end-to-end:** el proyecto no incluye framework E2E de serie. Se recomienda Playwright o Cypress; crea un paquete en `e2e/` y actualiza `angular.json` para integrarlo.
- **CI/CD:** integra los comandos `npm ci`, `npm run build` y `npm test` en pipelines de integración continua para asegurar calidad en cada cambio.

## 10. Construcción y despliegue
1. Ejecuta `npm run build` para generar artefactos optimizados (minificación, budgets, hashes).
2. Los archivos se alojan en `dist/contable-web/`. Sirve el contenido estático con cualquier servidor compatible (Nginx, Apache, Firebase Hosting, etc.).
3. Para builds específicos crea configuraciones adicionales en `angular.json` (por ejemplo, `production`, `staging`).
4. Controla variables de entorno mediante archivos `environment.*.ts` y reemplazos configurados en `angular.json` (añádelos según las necesidades del proyecto).

## 11. Gestión de entornos y variables
- Crea `src/environments/` con archivos `environment.ts` y `environment.prod.ts` para separar configuraciones (API endpoints, flags de logging).
- Declara los reemplazos en `angular.json` bajo `fileReplacements` en cada configuración de build.
- Evita exponer secretos: utiliza servicios de configuración o inject tokens que lean valores en tiempo de ejecución.

## 12. Resolución de problemas frecuentes
| Problema | Posible causa / solución |
| --- | --- |
| Error `Node.js version XX is not supported` | Actualiza Node a la versión recomendada (18.18+). |
| Puerto 4200 ocupado | Ejecuta `npm start -- --port=4300` para usar otro puerto. |
| Chrome no inicia durante los tests | Instala dependencias de navegador en tu SO o configura Karma para usar `ChromeHeadlessNoSandbox`. |
| Estilos globales no aplican | Verifica que estén definidos en `src/styles.css` o importados explícitamente desde el componente. |

## 13. Próximos pasos sugeridos
- Definir estructura de módulos por funcionalidades contables (ej. `balances/`, `facturacion/`).
- Configurar un sistema de diseño o librería de componentes (Angular Material, Tailwind CSS).
- Integrar servicios para consumir APIs REST/GraphQL y manejar autenticación.
- Configurar pipelines de CI/CD y despliegue automático.

## 14. Recursos adicionales
- [Documentación oficial de Angular](https://angular.dev/)
- [Referencia de Angular CLI](https://angular.dev/tools/cli)
- [Guía de estilo de Angular](https://angular.dev/style-guide)
- [Documentación de RxJS](https://rxjs.dev/)

> Mantén este documento actualizado conforme evolucionen las funcionalidades, dependencias y procesos del proyecto.
