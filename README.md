# Laravel Artisan: guía de consulta

Repositorio de referencia rápida para desarrollar y mantener aplicaciones con Laravel. Reúne comandos `php artisan` de uso frecuente, organizados por tarea y acompañados de una explicación breve y ejemplos.

## Contenido

- [Chuleta de comandos Artisan](./CHULETA-ARTISAN.md): consulta por categorías, ejemplos y advertencias.

## Cómo usar esta guía

1. Abre la [chuleta](./CHULETA-ARTISAN.md) y busca la categoría de la tarea que necesitas.
2. Sustituye nombres de ejemplo como `Post`, `posts` o `<id>` por los de tu proyecto.
3. Ejecuta los comandos desde la raíz de una aplicación Laravel, donde se encuentra el archivo `artisan`.
4. Antes de ejecutar un comando desconocido, consulta `php artisan <comando> --help`.

La guía explica comandos habituales; **no es una lista exhaustiva ni garantiza que todos existan en cada versión o instalación**. Laravel, los paquetes instalados y la configuración del proyecto determinan los comandos y opciones disponibles. Para comprobarlos en tu aplicación:

```bash
php artisan list
php artisan help migrate
php artisan migrate --help
```

## Requisitos y notas

- Se necesita una aplicación Laravel instalada y las dependencias de Composer disponibles.
- La versión de PHP y los requisitos de Laravel dependen de la versión del proyecto.
- Los ejemplos usan `php artisan`. En proyectos con Laravel Sail, Docker, WSL o un ejecutable PHP personalizado, usa el envoltorio correspondiente, por ejemplo `./vendor/bin/sail artisan list` o `docker compose exec app php artisan list`.
- Algunos comandos requieren servicios o tablas configurados (base de datos, Redis, colas, almacenamiento, etc.).
- Los comandos que reinician o eliminan datos se señalan en la chuleta. Confirma siempre el entorno y los datos antes de ejecutarlos; no uses comandos destructivos en producción sin un procedimiento probado y una copia de seguridad adecuada.

## Convenciones de los ejemplos

- `Post` representa un nombre de clase; `posts`, el nombre de una tabla o recurso.
- `<id>` y `<nombre>` son marcadores: reemplázalos por valores reales y no escribas los signos `<` y `>` literalmente.
- Las opciones largas, como `--resource`, suelen poder combinarse con el argumento del comando. Confirma la sintaxis exacta con `--help` para tu versión.

## Contribuciones

Las mejoras son bienvenidas. Para mantener la guía útil:

1. Comprueba el comando y sus opciones en una aplicación Laravel real usando `php artisan list` y `php artisan <comando> --help`.
2. Indica si una función requiere una versión concreta o un paquete adicional.
3. Mantén los ejemplos claros y añade una advertencia cuando una operación pueda borrar o modificar datos.
4. Actualiza este README si cambian los documentos disponibles.

## Licencia

No se especifica una licencia en este repositorio. Antes de reutilizar o redistribuir su contenido, consulta al propietario del repositorio.
