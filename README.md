# Inventario UPQ — versión actualizada

## Cambios incluidos
- Una sola sección de apartados y devoluciones.
- Nombre del docente obligatorio y validación de campos requeridos.
- Fecha de devolución fijada al mismo día de la solicitud; la entrega y devolución guardan sus marcas de tiempo automáticas.
- Checklist de devolución por material, con devoluciones parciales y registro de daños.
- Historial filtrable por mes.
- Cuentas A y B unificadas bajo el rol Operador para nuevas cuentas; los roles antiguos A/B se muestran como Operador.
- Solo el Administrador puede crear usuarios, aprobar/rechazar préstamos, gestionar devoluciones, registrar adeudos y realizar altas/bajas de inventario.
- El alumno puede consultar sus propias solicitudes y ver folio, estado, materiales y horas de petición/entrega.
- Cada usuario puede cambiar su contraseña desde el botón de perfil, verificando la contraseña actual y confirmando una nueva de al menos 8 caracteres.

## Instalación local
1. Instalar Node.js 20 o superior.
2. Abrir una terminal en esta carpeta.
3. Ejecutar `npm install`.
4. Ejecutar `npm start`.
5. Abrir `http://localhost:3000`.

## Importante para publicar
No se incluye `node_modules`, la base de datos local `data/inventario.sqlite` ni archivos `.env`. Render instalará dependencias con `npm install`.
La base de datos debe almacenarse en un disco persistente de Render para conservar cuentas, préstamos e inventario entre despliegues. No activar un disco de pago sin revisar primero el costo del servicio.

## Cuentas
Las contraseñas iniciales solo se usan al crear por primera vez las cuentas predeterminadas. Después de iniciar sesión, cada usuario puede cambiarlas desde Mi cuenta. Los cambios ya guardados no se sobrescriben en cada reinicio.
