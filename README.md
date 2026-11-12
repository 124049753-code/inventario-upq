# Sistema de Inventario UPQ — versión nueva

Proyecto web local para administrar inventario, solicitudes, préstamos, devoluciones y adeudos.

## Contenido del inventario importado
Se importan automáticamente **655 registros** del archivo `InventarioLyTP2025 (1)(2).xlsx`:
- LT1: 316
- LT2: 155
- LT3: 115
- Edificio A – LT1: 32 (conserva la ubicación Edificio A – Laboratorio de Química)
- Edificio C: 37

En 31 registros de LT3 el Excel no tenía cantidad habilitada; se importan con cantidad 0 para no inventar existencias.

## Requisitos
- Windows 10/11
- Node.js LTS (npm incluido)
- Visual Studio Code recomendado

No requiere MySQL ni MySQL Workbench. Usa SQLite local (archivo `data/inventario.sqlite`).

## Ejecutar desde VS Code
1. Extrae el ZIP en una carpeta.
2. Abre esa carpeta directamente en VS Code. `package.json` debe verse en el nivel principal.
3. Abre Terminal → Nueva terminal.
4. Ejecuta:
   ```powershell
   npm install
   npm start
   ```
5. Abre http://localhost:3000

También puedes ejecutar `INICIAR.bat` desde el Explorador de Windows. Instala dependencias si faltan e inicia el servidor.

## Accesos iniciales
- Administrador: usuario `admin`, contraseña `Admin2026!`
- Alumnos/docentes: su matrícula como usuario, contraseña fija `UPQ22`.
- El administrador puede crear las cuentas y asignar nivel A, B, C o admin. Para crear cuentas, se requiere nombre y matrícula/usuario.
- Las cuentas con estatus `baja` no pueden iniciar sesión.

## Funciones de esta revisión
- Catálogo con 655 materiales e imágenes individuales, buscador y filtro por almacén.
- Una sola pestaña **Apartados y devoluciones**: formulario de apartado y, para administración, solicitudes pendientes y préstamos que esperan devolución.
- Campos obligatorios: solicitante, matrícula, carrera/área, docente responsable, fecha estimada de devolución y al menos un material con cantidad válida. Las observaciones son opcionales.
- Fecha y hora de petición automáticas al guardar el apartado.
- La fecha y hora de entrega se registran automáticamente cuando el administrador o nivel A acepta el apartado.
- Validación en servidor de existencias; no permite apartar más unidades que las disponibles ni repetir un material en dos renglones.
- Checklist por material para registrar devoluciones parciales o completas. Lo que no se marca continúa pendiente.
- El material recibido en buen estado vuelve a estar disponible; las piezas dañadas se descuentan del inventario físico y generan un adeudo.
- La pestaña **Historial** permite filtrar movimientos por mes y consultar petición, entrega, docente, materiales y devoluciones.
- Adeudos, reportes, exportación CSV y administración de usuarios según permisos.
- Se corrigen cantidades negativas heredadas al iniciar la aplicación y se impide registrar bajas por encima de la existencia física.

## Integración con SII
La validación automática del SII no se activa porque no se proporcionó una API institucional ni credenciales autorizadas. Por ahora el administrador puede registrar el estado activo/baja de cada cuenta desde Usuarios y permisos. No se simula una conexión al SII.

## Datos
La base se crea automáticamente al primer arranque y carga `seed_inventory.sql` cuando el inventario está vacío. Los datos se guardan en `data/inventario.sqlite`. No borres esa carpeta si quieres conservar solicitudes, movimientos, usuarios y adeudos.

Para reiniciar todo desde cero, detén el servidor y elimina la carpeta `data`; al volver a iniciar se importa nuevamente el inventario inicial. Esto borra también los movimientos y usuarios creados localmente.

## Recursos visuales
La galería incluye imágenes ilustrativas de LT1, LT2, LT3, Química y Redes. Cada renglón del catálogo tiene una miniatura ilustrada genérica de herramienta; para fotografías específicas de los 655 artículos se necesitaría un catálogo fotográfico real de cada material.


## Catálogo visual completo
Se asignó una imagen única a cada uno de los 655 materiales del inventario. Se recuperaron 621 fotografías incrustadas en el Excel original y se generaron 34 ilustraciones individuales para los registros que no tenían una fotografía utilizable. Las 655 rutas son distintas y están incluidas localmente en `public/assets/materiales`; no dependen de internet.


## Acceso para alumnos y docentes
- Usuario compartido: `usuarioupq`
- Contraseña: `UPQ22`
- Administrador: `admin` / `Admin2026!`

Como los alumnos y docentes comparten una sola cuenta, al crear cada solicitud el sistema pide el nombre completo y la matrícula de la persona que solicita. Estos datos se guardan en el vale y se muestran en préstamos, devoluciones y adeudos para identificar correctamente a cada persona.


## Uso desde teléfonos y HTTPS
La interfaz se adapta a pantallas pequeñas: menú desplazable, formularios en una columna y tablas que se pueden deslizar horizontalmente.
Para que funcione con HTTPS y desde cualquier teléfono fuera de la red local, debe publicarse en un servicio de alojamiento. Se incluye `render.yaml` como referencia de despliegue. Al publicar:
1. Sube esta carpeta a un repositorio privado de GitHub.
2. En Render, crea un servicio nuevo usando ese repositorio y el archivo `render.yaml` (Blueprint).
3. Espera a que termine el despliegue y abre la URL `https://...onrender.com` que Render asigna.
4. Comparte esa URL con los usuarios. Render proporciona el certificado HTTPS; el servidor redirige las visitas HTTP a HTTPS.
No publiques el repositorio como público si contiene datos reales. Cambia las contraseñas compartidas antes de usarlo con información institucional real. La cuenta compartida no identifica automáticamente a la persona que inició sesión: el solicitante debe capturar su nombre y matrícula en cada solicitud.


## Vistas según el nivel de acceso
- La cuenta compartida `usuarioupq` (Nivel C) muestra Resumen, Inventario y Solicitar herramienta. No muestra opciones administrativas ni listas de movimientos de otras personas.
- Las cuentas administrativas conservan Préstamos activos, Devoluciones, Adeudos, Reportes y Usuarios y permisos según su nivel.
- **Importante:** mientras alumnos y docentes compartan una sola cuenta, el sistema no puede identificar de forma segura quién consulta sus préstamos o adeudos. Por privacidad, la cuenta compartida no recibe esos listados. Para habilitar “Mis préstamos”, “Mis devoluciones” y “Mis adeudos” de forma privada, cada persona debe tener una identidad autenticada individual (por ejemplo, matrícula y contraseña propia).


## Importante sobre el plan gratuito de Render
El servicio web gratuito puede suspenderse por inactividad y el sistema puede tardar en responder al primer acceso. Además, el sistema usa SQLite en un archivo local. En un servicio gratuito sin disco persistente, los datos operativos (solicitudes, usuarios creados, préstamos, devoluciones y adeudos) no están garantizados ante reinicios o nuevos despliegues. El catálogo de 655 materiales se puede volver a importar desde `seed_inventory.sql`, pero los movimientos no. Antes de usarlo como sistema institucional real, configura almacenamiento persistente compatible o migra la base a un servicio de base de datos administrado y establece respaldos. No cargues datos reales de estudiantes hasta resolver esa persistencia.

## Cambios de la revisión
- Se agregó `teacher_name` y `delivered_at` a las solicitudes con migración automática para bases existentes.
- Se agregó la tabla `return_events` para conservar el historial de devoluciones parciales.
- La cuenta compartida `usuarioupq` no puede consultar historiales globales de alumnos; la pestaña Historial queda para niveles administrativos B/A y admin. Para historial individual privado se requieren cuentas personales autenticadas.
