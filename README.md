# retraction-seeker

Basado completamente en https://github.com/volca02/retraction-seeker

La idea de este test es para agilizar encontrar las retracciones adecuadas de forma rapida, generando un test masivo con las combinaciones de interes a revisar.

### Para Ender 3

A falta de encontrar valores que puedan resultar mas adecuados, lo unico basico que es necesario ajustar para cada test debido a las personalizaciones individuales de cada uno, partiendo del ejemplo *config_sample/settings-ender3.json* es lo siguiente:

Distancia de retraccion:
- **steps_x**: 7
- **ret_d_start**: 4.0
- **ret_d_step**: 0.5

Generara **steps_x** columnas en X, en las que partiendo de **ret_d_start**, en cada cambio incrementara las retracciones en **ret_d_step**. 

```
[ 1] .. 4.0 .. [ 2] .. 4.5 .. [ 3] .. 5.0 .. [ 4] .. 5.5 .. [ 5] .. 6.0 .. [ 6] .. 6.5 .. [ 7]
```

Velocidad de retraccion:
- **steps_y**: 5,
- **ret_spd_start**: 20,
- **ret_spd_step**: 10,

```
[ 5] 60mm/s
[ 4] 50mm/s
[ 3] 40mm/s
[ 2] 30mm/s
[ 1] 20mm/s
```

Generara **steps_y** columnas en Y, en las que hara las siguientes pruebas

Temperatura del nozzle:
- **steps_z**: 1,
- **ret_temp_start**: 190,
- **ret_temp_step**: -5,

Generara **steps_z** pruebas cada 5mm de altura, partiendo de **ret_temp_start** y cambiando la temperatura en **ret_temp_step** grados.   

En total genera **steps_x** x **steps_y** x **steps_z** en 1 impresion.

```
[ 5] 60.00 mm/s :  [ 1] .. 4.0 mm .. [ 2] .. 4.5 mm .. [ 3] .. 5.0 mm .. [ 4] .. 5.5 mm .. [ 5] .. 6.0 mm .. [ 6] .. 6.5 mm .. [ 7]
[ 4] 50.00 mm/s :  [ 1] .. 4.0 mm .. [ 2] .. 4.5 mm .. [ 3] .. 5.0 mm .. [ 4] .. 5.5 mm .. [ 5] .. 6.0 mm .. [ 6] .. 6.5 mm .. [ 7]
[ 3] 40.00 mm/s :  [ 1] .. 4.0 mm .. [ 2] .. 4.5 mm .. [ 3] .. 5.0 mm .. [ 4] .. 5.5 mm .. [ 5] .. 6.0 mm .. [ 6] .. 6.5 mm .. [ 7]
[ 2] 30.00 mm/s :  [ 1] .. 4.0 mm .. [ 2] .. 4.5 mm .. [ 3] .. 5.0 mm .. [ 4] .. 5.5 mm .. [ 5] .. 6.0 mm .. [ 6] .. 6.5 mm .. [ 7]
[ 1] 20.00 mm/s :  [ 1] .. 4.0 mm .. [ 2] .. 4.5 mm .. [ 3] .. 5.0 mm .. [ 4] .. 5.5 mm .. [ 5] .. 6.0 mm .. [ 6] .. 6.5 mm .. [ 7]
```
