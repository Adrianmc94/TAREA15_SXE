# TAREA15_SXE Módulo de Odoo el cúal calcula que necesitas con tu nivel de cansancio!:
## Para empezar crearemos la estructura inicial para el módulo en la carpeta addons
### Usaremos el comando scaffold ( como root por si el usuario de Docker no tiene permisos):
'''' bash
docker exec -it -u 0 odoo18_app odoo scaffold calcular-cansancio /mnt/extra-addons
''''
### Para otergar los permisos usamos el siguiente comando:
docker exec -it odoo18_app chmod -R 777 /mnt/extra-addons/calcular-cansancio

## Ahora tenemos que configurar el archivo manifest, dentro de la carpeta addons calcular-cansancio:

