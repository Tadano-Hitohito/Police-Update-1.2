# Comandos del servidor

npm run dev
## Cerrar todas las instancias de Node.js

Get-Process node -ErrorAction SilentlyContinue | Stop-Process -Force

## Reiniciar el servidor

Cierra todas las instancias y vuelve a arrancar el servidor:

```powershell
Get-Process node -ErrorAction SilentlyContinue | Stop-Process -Force; npm run dev
```

Para reiniciar solamente la ejecución activa de `npm run dev`, pulsa `Ctrl+C` y después ejecuta:

```powershell
npm run dev
```

## Pendientes




12. crear un mdulo llamado academico (nivel de estudio, grado academico, lugar de estudio, especialidades, estatus de estudio,si tiene mas que bachiller y tiene mas carreras desplegar, titulo de formacion pdf, nombre de la institucion, poner fecha de ingreso y si hay fecha de egreso)

13. tipo de vivienda (por seleccion el tipo)

14. gravedad del inmueble (seleccion de la condicion del inmueble)

15. seleccion de cantidad de hijos, personas mayor a su cargo y personas con condiciones medicas (seleccion simple y poner cantidad)

16. personas en la vivienda poner en numero (nucleo familiar cantidad de familiar que en la vivienda poner un ejemplo y que se llene en numeros)

17. condiciones medicas una descripcion de la condicion de las personas con condiciones medicas a su cargo

18. en contacto de emergencia hacer una encuesta simple de si quiere poner a otros mas como contactos de emergencia (repite los mismos datos que aparecen en ese modulo)

19. 
























































