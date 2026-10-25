# Chuleta de comandos `php artisan`

Referencia organizada por tareas para los comandos Artisan más usados en proyectos Laravel. Ejecuta los ejemplos desde la raíz de la aplicación.

> **Compatibilidad:** los comandos y sus opciones cambian entre versiones de Laravel y pueden depender de paquetes instalados. Comprueba tu instalación con `php artisan list` y `php artisan <comando> --help`.
>
> **Precaución:** `migrate:fresh`, `migrate:reset`, `migrate:refresh`, `db:wipe`, `schema:drop` y otros comandos marcados como destructivos pueden borrar datos. Revisa el entorno antes de confirmar.

## Índice

- [Ayuda e información](#ayuda-e-información)
- [Desarrollo local](#desarrollo-local)
- [Crear archivos y componentes](#crear-archivos-y-componentes)
- [Migraciones](#migraciones)
- [Base de datos, seeders y modelos](#base-de-datos-seeders-y-modelos)
- [Rutas](#rutas)
- [Configuración, cachés y optimización](#configuración-cachés-y-optimización)
- [Colas y trabajos](#colas-y-trabajos)
- [Eventos, listeners y notificaciones](#eventos-listeners-y-notificaciones)
- [Vistas, recursos y almacenamiento](#vistas-recursos-y-almacenamiento)
- [Pruebas](#pruebas)
- [Mantenimiento y tareas programadas](#mantenimiento-y-tareas-programadas)
- [Paquetes y comandos personalizados](#paquetes-y-comandos-personalizados)
- [Flujos de trabajo frecuentes](#flujos-de-trabajo-frecuentes)

## Ayuda e información

| Comando | Qué hace |
|---|---|
| `php artisan list` | Muestra los comandos disponibles en la aplicación. |
| `php artisan help <comando>` | Explica el uso y las opciones de un comando. Ejemplo: `php artisan help make:model`. |
| `php artisan <comando> --help` | Alternativa para consultar la ayuda específica. |
| `php artisan about` | Muestra información general de la aplicación, el entorno y sus componentes (disponibilidad según versión). |
| `php artisan env` | Indica el entorno de aplicación activo, por ejemplo `local` o `production`. |
| `php artisan inspire` | Muestra una frase; es un comando de ejemplo, no una herramienta de diagnóstico. |

## Desarrollo local

| Comando | Qué hace |
|---|---|
| `php artisan serve` | Inicia el servidor de desarrollo local, normalmente en `http://127.0.0.1:8000`. Detén el proceso con `Ctrl+C`. |
| `php artisan serve --host=0.0.0.0 --port=8080` | Inicia el servidor en una interfaz y puerto especificados. Expón el servidor solo en una red de confianza. |
| `php artisan key:generate` | Genera la clave de cifrado de la aplicación y la escribe en `.env`. No regeneres la clave a la ligera en una aplicación existente: puede invalidar datos cifrados y sesiones. |
| `php artisan storage:link` | Crea el enlace público desde `public/storage` hacia el almacenamiento público de Laravel. |
| `php artisan down` | Activa el modo de mantenimiento y devuelve respuestas de indisponibilidad. |
| `php artisan up` | Desactiva el modo de mantenimiento. |
| `php artisan down --secret=<token>` | Activa mantenimiento permitiendo acceso temporal a quien visite el token secreto. Trata el token como una contraseña. |
| `php artisan down --render=errors::503` | Intenta renderizar una vista durante el mantenimiento; confirma que la opción esté disponible en tu versión. |

## Crear archivos y componentes

Los comandos `make:*` generan esqueletos de archivos; normalmente no implementan la lógica de negocio. Las rutas y opciones pueden variar según la versión y los paquetes.

### Aplicación y lógica HTTP

| Comando | Qué genera / para qué sirve |
|---|---|
| `php artisan make:model Post` | Modelo Eloquent, habitualmente en `app/Models/Post.php`. |
| `php artisan make:model Post -m` | Modelo y migración asociada. `-m` equivale a `--migration`. |
| `php artisan make:model Post -mf` | Modelo, migración y factory. |
| `php artisan make:model Post -mfs` | Modelo, migración, factory y seeder. |
| `php artisan make:model Post -a` | Genera varios archivos asociados (comúnmente migración, factory, seeder, policy y resource controller); revisa `--help` para el alcance exacto. |
| `php artisan make:controller PostController` | Crea un controlador. |
| `php artisan make:controller PostController --resource` | Crea métodos convencionales para las acciones CRUD de una resource route. |
| `php artisan make:controller PostController --api` | Crea un controlador de recurso sin acciones HTML `create` y `edit`. |
| `php artisan make:controller PostController --resource --model=Post` | Genera un resource controller vinculado al modelo indicado (opciones disponibles según versión). |
| `php artisan make:middleware EnsureUserIsAdmin` | Crea middleware para filtrar o modificar solicitudes HTTP. |
| `php artisan make:request StorePostRequest` | Crea una clase Form Request para validación y autorización. |
| `php artisan make:resource PostResource` | Crea una transformación de modelo para respuestas de API. |
| `php artisan make:policy PostPolicy --model=Post` | Crea una policy para organizar la autorización de acciones sobre el modelo. |
| `php artisan make:provider BillingServiceProvider` | Crea un service provider para registrar servicios o configuración. |
| `php artisan make:exception PaymentFailedException` | Crea una excepción personalizada. |
| `php artisan make:rule Uppercase` | Crea una regla de validación personalizada. |
| `php artisan make:cast AsCurrency` | Crea un cast personalizado para atributos Eloquent. |
| `php artisan make:enum OrderStatus` | Crea un enum, si la versión de Laravel ofrece este generador. |
| `php artisan make:class Services/PaymentService` | Crea una clase PHP. La ruta/namespace depende de la convención del proyecto. |

### Interfaz, comunicación y tareas en segundo plano

| Comando | Qué genera / para qué sirve |
|---|---|
| `php artisan make:job ProcessPodcast` | Crea un trabajo que puede ejecutarse en una cola o de forma síncrona. |
| `php artisan make:job ProcessPodcast --sync` | Genera un job síncrono en versiones que incluyen esta opción. |
| `php artisan make:event OrderShipped` | Crea una clase de evento. |
| `php artisan make:listener SendShipmentNotification --event=OrderShipped` | Crea un listener asociado al evento indicado. |
| `php artisan make:notification InvoicePaid` | Crea una notificación que puede enviarse por los canales configurados. |
| `php artisan make:mail OrderReceipt` | Crea una clase Mailable para preparar correos. |
| `php artisan make:mail OrderReceipt --markdown=mail.orders.receipt` | Crea un Mailable con una vista Markdown (opción dependiente de versión). |
| `php artisan make:observer PostObserver --model=Post` | Crea un observer para responder a eventos del ciclo de vida de un modelo. |
| `php artisan make:command ImportProducts` | Crea un comando Artisan personalizado. |
| `php artisan make:component Alert` | Crea un componente Blade (clase y vista, según tipo y opciones). |
| `php artisan make:component Alert --view` | Crea un componente Blade anónimo basado en vista (si la opción existe en tu versión). |
| `php artisan make:view dashboard` | Crea una vista Blade. Los nombres con puntos suelen convertirse en carpetas. |
| `php artisan make:factory PostFactory --model=Post` | Crea una factory para generar modelos de prueba o datos de desarrollo. |
| `php artisan make:seeder PostSeeder` | Crea un seeder para insertar datos iniciales o de ejemplo. |
| `php artisan make:test PostTest` | Crea una prueba Feature (habitualmente). |
| `php artisan make:test PostTest --unit` | Crea una prueba unitaria. |

### Generadores de tablas auxiliares

Estos comandos crean migraciones para tablas que pueden necesitar funciones de Laravel. **Generar la migración no equivale a ejecutarla**.

| Comando | Propósito |
|---|---|
| `php artisan make:cache-table` | Genera la migración para almacenar caché en base de datos, cuando está disponible. |
| `php artisan make:session-table` | Genera la migración para sesiones en base de datos. |
| `php artisan make:queue-table` | Genera la migración para la tabla de trabajos de cola. |
| `php artisan make:queue-batches-table` | Genera la migración para lotes de trabajos. |
| `php artisan make:queue-failed-table` | Genera la migración para trabajos fallidos. |
| `php artisan make:notifications-table` | Genera la migración para notificaciones almacenadas en base de datos. |

## Migraciones

### Crear, ejecutar y revisar

| Comando | Qué hace |
|---|---|
| `php artisan make:migration create_posts_table` | Crea un archivo de migración nuevo. |
| `php artisan make:migration create_posts_table --create=posts` | Genera el esqueleto para crear la tabla `posts`. |
| `php artisan make:migration add_published_at_to_posts_table --table=posts` | Genera el esqueleto para modificar la tabla existente `posts`. |
| `php artisan migrate` | Ejecuta las migraciones pendientes. |
| `php artisan migrate --pretend` | Muestra las consultas que se ejecutarían sin aplicarlas (soporte según driver/versión). |
| `php artisan migrate --seed` | Ejecuta migraciones y luego los seeders configurados para ese flujo. |
| `php artisan migrate:status` | Indica qué migraciones se ejecutaron y cuáles están pendientes. |
| `php artisan migrate --step` | Ejecuta cada migración en su propio lote, si la versión admite la opción. |

### Revertir o reconstruir

| Comando | Qué hace | Riesgo |
|---|---|---|
| `php artisan migrate:rollback` | Revierte el último lote de migraciones. | Puede alterar o eliminar datos modificados por esas migraciones. |
| `php artisan migrate:rollback --step=2` | Revierte los últimos dos pasos/lotes según el comportamiento de la versión. | Revisar el alcance antes de ejecutar. |
| `php artisan migrate:reset` | Revierte todas las migraciones. | Alto: deja el esquema sin migraciones aplicadas y puede perder datos. |
| `php artisan migrate:refresh` | Revierte migraciones y las vuelve a ejecutar. | Alto: las operaciones `down()` pueden borrar o transformar datos. |
| `php artisan migrate:refresh --seed` | Reconstruye el esquema mediante rollback y vuelve a sembrar datos. | Destructivo para datos no preservados por los seeders. |
| `php artisan migrate:fresh` | Elimina todas las tablas y ejecuta las migraciones desde cero; no usa `down()` de cada migración. | **Destructivo: elimina las tablas y sus datos. Nunca usar en producción.** |
| `php artisan migrate:fresh --seed` | Elimina todas las tablas, migra de cero y ejecuta los seeders. | **Destructivo.** |
| `php artisan migrate --force` | Omite la confirmación interactiva en producción. | No reduce el riesgo; úsalo solo cuando el despliegue esté controlado. |

**Regla práctica:** para cambios normales usa `migrate`; para diagnosticar usa `migrate:status`; reserva `rollback`, `refresh` y especialmente `fresh` para entornos y procedimientos donde entiendas el impacto.

## Base de datos, seeders y modelos

| Comando | Qué hace |
|---|---|
| `php artisan db` | Abre una consola interactiva para la conexión predeterminada, si el driver y la versión lo admiten. |
| `php artisan db:show` | Resume información de la base de datos (comando disponible según versión). |
| `php artisan db:table posts` | Muestra información de una tabla (disponibilidad según versión). |
| `php artisan db:monitor` | Supervisa conexiones de base de datos y puede emitir eventos/alertas según configuración. |
| `php artisan db:seed` | Ejecuta el seeder principal, normalmente `DatabaseSeeder`. |
| `php artisan db:seed --class=PostSeeder` | Ejecuta un seeder concreto. |
| `php artisan db:seed --force` | Ejecuta seeders en producción sin confirmación interactiva. Úsalo solo deliberadamente. |
| `php artisan model:show Post` | Muestra información del modelo y sus atributos/relaciones cuando está disponible en esa versión. |
| `php artisan schema:dump` | Genera un volcado del esquema para acelerar instalaciones nuevas. |
| `php artisan schema:dump --prune` | Genera el volcado y elimina migraciones ya representadas por él; evalúa el historial y el control de versiones antes de usarlo. |
| `php artisan db:wipe` | Elimina tablas, vistas y/o tipos según driver y opciones. **Destructivo.** |

Las factories se usan principalmente desde código de pruebas, seeders o Tinker, por ejemplo `Post::factory()->count(10)->create()`. No todas las operaciones de factories tienen un comando Artisan dedicado.

## Rutas

| Comando | Qué hace |
|---|---|
| `php artisan route:list` | Lista las rutas registradas. |
| `php artisan route:list --path=api` | Filtra rutas cuyo URI coincide con `api`. |
| `php artisan route:list --name=posts` | Filtra por nombre de ruta. |
| `php artisan route:list --method=GET` | Filtra por método HTTP. |
| `php artisan route:list -v` | Incluye información más detallada, como middleware; las opciones exactas varían. |
| `php artisan route:cache` | Compila las rutas para despliegue. Las rutas deben poder serializarse y no depender de closures incompatibles. |
| `php artisan route:clear` | Elimina la caché de rutas. |

## Configuración, cachés y optimización

| Comando | Qué hace |
|---|---|
| `php artisan config:show app` | Muestra la configuración de una sección, si el comando existe en la versión. No compartas su salida si expone secretos. |
| `php artisan config:cache` | Combina la configuración en una caché para despliegue. Tras cachearla, Laravel no carga `.env` como en desarrollo; no uses `env()` fuera de archivos de configuración. |
| `php artisan config:clear` | Elimina la caché de configuración. |
| `php artisan cache:clear` | Limpia la caché de aplicación del store configurado. |
| `php artisan cache:table` | Alias disponible en algunas versiones para generar migración de caché; en otras se usa `make:cache-table`. Verifica con `list`. |
| `php artisan view:cache` | Precompila las vistas Blade. |
| `php artisan view:clear` | Elimina las vistas compiladas. |
| `php artisan event:cache` | Cachea el manifiesto de eventos/listeners cuando el proyecto usa descubrimiento de eventos. |
| `php artisan event:clear` | Limpia la caché del manifiesto de eventos. |
| `php artisan optimize` | Ejecuta optimizaciones habituales de despliegue disponibles en la versión instalada. |
| `php artisan optimize:clear` | Limpia cachés de optimización habituales, incluyendo caché de aplicación y archivos generados según versión. |
| `php artisan clear-compiled` | Elimina archivos compilados del framework en versiones que mantienen este comando. |

En producción, sigue el flujo de despliegue de tu proyecto. No generes cachés de configuración con valores locales o secretos incorrectos.

## Colas y trabajos

Para procesar trabajos asíncronos, configura primero la conexión de cola y ejecuta las migraciones necesarias.

| Comando | Qué hace |
|---|---|
| `php artisan queue:work` | Inicia un worker persistente que procesa trabajos hasta que se detenga. |
| `php artisan queue:work --queue=emails,default` | Prioriza las colas indicadas en ese orden. |
| `php artisan queue:work --once` | Procesa un trabajo y termina (si hay uno disponible). |
| `php artisan queue:work --tries=3` | Limita los intentos de ejecución del worker. |
| `php artisan queue:work --timeout=90` | Define el tiempo máximo de ejecución de un trabajo para ese worker. |
| `php artisan queue:listen` | Inicia un listener que reinicia la aplicación para cada trabajo; es más lento y puede ser útil durante desarrollo. |
| `php artisan queue:restart` | Señala a los workers persistentes que se reinicien al terminar el trabajo actual, por ejemplo tras desplegar código nuevo. |
| `php artisan queue:failed` | Lista los trabajos fallidos (el comando puede llamarse `queue:failed` o variar por versión). |
| `php artisan queue:retry <id>` | Reintenta un trabajo fallido concreto. |
| `php artisan queue:retry all` | Reintenta todos los trabajos fallidos. |
| `php artisan queue:forget <id>` | Elimina un registro concreto de trabajo fallido. |
| `php artisan queue:flush` | Elimina todos los registros de trabajos fallidos. No reintenta los trabajos. |
| `php artisan queue:clear` | Vacía trabajos pendientes de una conexión/cola, si el driver lo permite. Puede descartar trabajo válido. |
| `php artisan queue:prune-batches` | Elimina registros antiguos de lotes de trabajos. |
| `php artisan queue:prune-failed` | Elimina registros antiguos de trabajos fallidos, si está disponible. |

Los comandos de pausa/reanudación de cola y sus argumentos cambian entre versiones y drivers; consúltalos con `php artisan queue --help` antes de usarlos. En producción, administra los workers con el supervisor de procesos recomendado para tu infraestructura.

## Eventos, listeners y notificaciones

| Comando | Qué hace |
|---|---|
| `php artisan event:list` | Lista eventos y listeners detectados por la aplicación. |
| `php artisan event:cache` | Cachea el manifiesto de eventos para despliegue. |
| `php artisan event:clear` | Borra ese manifiesto. |
| `php artisan make:event OrderShipped` | Crea un evento. |
| `php artisan make:listener SendShipmentNotification --event=OrderShipped` | Crea un listener para el evento. |
| `php artisan make:notification InvoicePaid` | Crea una notificación. |
| `php artisan notifications:table` | Genera una migración para notificaciones en base de datos en versiones que ofrecen este alias; también puede estar disponible `make:notifications-table`. |

Los eventos y las notificaciones se despachan desde la aplicación; generar sus clases no las registra ni las envía automáticamente en todos los casos.

## Vistas, recursos y almacenamiento

| Comando | Qué hace |
|---|---|
| `php artisan make:view pages.contact` | Crea una vista Blade, a menudo en una ruta equivalente a `resources/views/pages/contact.blade.php`. |
| `php artisan view:cache` | Precompila vistas Blade para reducir trabajo en ejecución. |
| `php artisan view:clear` | Elimina las vistas precompiladas. |
| `php artisan storage:link` | Crea el enlace simbólico de archivos públicos a `storage/app/public`. |
| `php artisan storage:unlink` | Elimina enlaces de almacenamiento creados por Laravel, si está disponible. Comprueba el destino antes de operar. |
| `php artisan make:resource PostResource` | Genera una clase Resource para presentar datos en una API. |
| `php artisan make:component Alert` | Genera un componente Blade reutilizable. |
| `php artisan vendor:publish` | Publica archivos de configuración, migraciones, vistas o assets de paquetes que ofrecen publicación. Usa `--help` y `--tag` para limitar qué se publica. |

## Pruebas

| Comando | Qué hace |
|---|---|
| `php artisan test` | Ejecuta la suite de pruebas integrada con el runner del proyecto. |
| `php artisan test --filter=PostTest` | Ejecuta las pruebas cuyo nombre coincide con el filtro (soporte según runner/versión). |
| `php artisan test --testsuite=Feature` | Ejecuta la suite indicada si está declarada en la configuración de pruebas. |
| `php artisan test --parallel` | Ejecuta pruebas en paralelo cuando la versión y dependencias lo permiten. |
| `php artisan make:test PostTest` | Genera una prueba Feature. |
| `php artisan make:test PostTest --unit` | Genera una prueba unitaria. |

También se pueden pasar opciones del runner de pruebas; consulta `php artisan test --help`.

## Mantenimiento y tareas programadas

| Comando | Qué hace |
|---|---|
| `php artisan down` | Activa el modo mantenimiento (respuesta 503). |
| `php artisan down --message="Actualización en curso"` | Muestra un mensaje de mantenimiento cuando la versión lo admite. |
| `php artisan down --secret=<token>` | Permite acceso mediante un token de bypass; protege ese valor como una credencial. |
| `php artisan down --redirect=/maintenance` | Redirige a una ruta durante mantenimiento en versiones que admiten la opción. |
| `php artisan up` | Devuelve la aplicación al modo normal. |
| `php artisan schedule:list` | Lista las tareas programadas, si el comando está disponible. |
| `php artisan schedule:run` | Ejecuta las tareas que corresponden al momento actual; normalmente lo invoca el scheduler del sistema. |
| `php artisan schedule:work` | Ejecuta el scheduler continuamente para desarrollo local. |
| `php artisan schedule:clear-cache` | Limpia bloqueos de tareas programadas que usan `withoutOverlapping`, cuando está disponible. |

**Operación segura:** planifica quién ejecutará `up` y cómo se verifica el servicio antes de activar mantenimiento en producción. El scheduler del sistema operativo suele invocar `schedule:run` cada minuto; consulta la documentación de la versión del proyecto.

## Paquetes y comandos personalizados

| Comando | Qué hace |
|---|---|
| `php artisan vendor:publish` | Copia al proyecto recursos publicables de paquetes instalados. Acota la operación con `--provider` o `--tag` cuando corresponda. |
| `php artisan make:command SyncCatalog` | Genera un comando propio de consola. |
| `php artisan list make` | Filtra comandos por espacio de nombres en versiones que admiten esta sintaxis. |

Un comando personalizado se ejecuta con `php artisan <firma> [argumentos]`; su firma se define en la clase del comando.

## Flujos de trabajo frecuentes

### Crear una entidad con migración y factory

```bash
php artisan make:model Post -mf
```

Edita el modelo, la migración y la factory generados. Después, en un entorno local desechable o controlado:

```bash
php artisan migrate
```

### Añadir una columna a una tabla

```bash
php artisan make:migration add_published_at_to_posts_table --table=posts
```

Define el cambio en `up()` y una reversión coherente en `down()`, y luego ejecuta `php artisan migrate`.

### Revisar rutas de API

```bash
php artisan route:list --path=api
```

### Preparar cachés para despliegue

```bash
php artisan config:cache
php artisan route:cache
php artisan view:cache
```

Ejecuta solo los pasos apropiados para el flujo de despliegue del proyecto y limpia la caché correspondiente si necesitas volver a un estado de desarrollo.

### Diagnosticar un comando

```bash
php artisan list
php artisan help queue:work
php artisan queue:work --help
```

Si un comando no aparece, comprueba la versión de Laravel, los paquetes instalados, el directorio de trabajo y la configuración del entorno.
