# retraction-seeker

Basado completamente en https://github.com/volca02/retraction-seeker

![Retracciones 1](https://cdn.thingiverse.com/renders/10/b3/ff/01/dc/cDSC_0050_preview_featured.jpg)
![Retracciones 2](https://cdn.thingiverse.com/renders/5c/51/01/9b/33/1355deb5276418003ca1f1376e1173a4_preview_featured.jpg)

No sabes si necesitas ajustar las retracciones? Intenta a [imprimir este test](https://www.thingiverse.com/thing:909901) y si sale "llena de pelitos" la respuesta es que si.

La idea de este test es encontrar de forma rapida los parametros de retracciones adecuadas, generando un test de forma masiva con las combinaciones de interes que queremos revisar.

### Para Ender 3

Generando el archivo *settings.json* adecuado, el generador es valido para cualquier impresora. La idea de este fork, es simplemente recopilar una pequeña lista de configuraciones basicas enfocadas a la Ender3/Pro y solo tener que modificar los parametros para el test en si tal y como lo queramos probar.

A falta de encontrar valores que puedan resultar mas adecuados, lo unico que es necesario ajustar para cada test debido a las personalizaciones individuales de cada uno, partiendo del ejemplo *config_sample/settings-ender3.json* es lo que se explica a continuacion.

**A tener en cuenta:** unas retracciones demasiado altas, pueden terminar causando un atasco, asi que si tienes dudas de que valores son mas adecuados, inicialmente no los toques y ya tienes un punto del que partir.

## GCodes preparados

No quieres complicaciones? Se incluye 1 GCode de ejemplo para una Ender3/Pro stock y otra con extrusion directa, tal y como se ve en las fotos de la carpeta screenshots.
Puedes usar directamente el que mejor se ajuste a tipo de extrusion para tener un punto de partida, y una vez visto en que rangos da mejores resultados, aprovechar y hacer la prueba lo mas concreta posible, haciendo unas pruebas nuevas con un incremento menor de velocidad y de distancia asi como partir de de unos valores iniciales mas altos.   

## Que va a generar este codigo?

Una impresion de torres de este estilo:

```
60.00 mm/s :  [ 1] .. 4.0 mm .. [ 2] .. 4.5 mm .. [ 3] .. 5.0 mm .. [ 4] .. 5.5 mm .. [ 5] .. 6.0 mm .. [ 6] .. 6.5 mm .. [ 7]
50.00 mm/s :  [ 1] .. 4.0 mm .. [ 2] .. 4.5 mm .. [ 3] .. 5.0 mm .. [ 4] .. 5.5 mm .. [ 5] .. 6.0 mm .. [ 6] .. 6.5 mm .. [ 7]
40.00 mm/s :  [ 1] .. 4.0 mm .. [ 2] .. 4.5 mm .. [ 3] .. 5.0 mm .. [ 4] .. 5.5 mm .. [ 5] .. 6.0 mm .. [ 6] .. 6.5 mm .. [ 7]
30.00 mm/s :  [ 1] .. 4.0 mm .. [ 2] .. 4.5 mm .. [ 3] .. 5.0 mm .. [ 4] .. 5.5 mm .. [ 5] .. 6.0 mm .. [ 6] .. 6.5 mm .. [ 7]
20.00 mm/s :  [ 1] .. 4.0 mm .. [ 2] .. 4.5 mm .. [ 3] .. 5.0 mm .. [ 4] .. 5.5 mm .. [ 5] .. 6.0 mm .. [ 6] .. 6.5 mm .. [ 7]
```

Las [ X] representan las torres que se generaran y los valores lo que se esta probando en dicha posicion.

Donde lo que interesa modificar a la mayoria es las velocidades de retraccion y la distancia de retraccion.

Estos datos de ejemplo utilizados antes, se generan al inicio del gcode resultante, asi que con abrir el archivo generado con un editor de texto como puede ser NotePad++, podemos verlo sin necesidad de entender que ni como se ha generado la prueba.  

### Distancia de retraccion:
- **steps_x**: 7
- **ret_d_start**: 4.0
- **ret_d_step**: 0.5

Generara **steps_x** columnas en X, en las que partiendo de **ret_d_start**, en cada cambio incrementara las retracciones en **ret_d_step**.

Lo mismo en palabras: Generara **7** columnas en X, en las que partiendo de **4.0**, en cada cambio incrementara las retracciones en **0.5**.

Y como vemos en el ejemplo anterior, tenemos:  

```
[ 1] .. 4.0 .. [ 2] .. 4.5 .. [ 3] .. 5.0 .. [ 4] .. 5.5 .. [ 5] .. 6.0 .. [ 6] .. 6.5 .. [ 7]

```

Si por ejemplo tenemos unas retracciones mas bajas, bastaria con hacer un cambio como el siguiente:
- **steps_x**: 10
- **ret_d_start**: 0.6
- **ret_d_step**: 0.1

Y ya estariamos generando 10 pruebas entre 0.6 y 1.6

### Velocidad de retraccion:
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

Lo mismo en palabras: Generara **5** columnas en Y, en las que hara las siguientes pruebas.

Por tanto, ya tenemos una rejilla/matriz, en la que cada fila, contiene las pruebas de retracciones para una velocidad de retraccion concreta.


### Temperatura del nozzle:
- **steps_z**: 1,
- **ret_temp_start**: 190,
- **ret_temp_step**: -5,

Generara **steps_z** pruebas cada 5mm de altura, partiendo de **ret_temp_start** y cambiando la temperatura en **ret_temp_step** grados.

Lo mismo en palabras: Generara **1** pruebas cada 5mm de altura, partiendo de **190** y cambiando la temperatura en **-5** grados.

En este caso concreto, como solo va a haber 1 piso, a los 5mm habra acabado la impresion.

Es posible hacer varias alturas? Si, pero una pila muy alta y estrecha, puede acabar derrumbandose.

En total genera **steps_x** x **steps_y** x **steps_z** en 1 impresion. Y a mas pruebas, obviamente, mas tardara. Pero si todo sale bien, sera tiempo bien invertido ;)

## Requisitos para Windows
- Instalar la ultima version 3.X de https://www.python.org/downloads/
  - Activar *Add Python 3.X to PATH* e instalar con normalidad
- Instalar la ultima version de Notepad++ de https://notepad-plus-plus.org/downloads/
- Descargar este codigo https://github.com/shawe/retraction-seeker/archive/master.zip
- Descomprimir el archivo
  - Escoger de la carpeta **retraction-seeker-master\config_sample** el archivo json mas adecuado, y copiarlo en **retraction-seeker-master\settings.json**
    - Importante respetar el nombre del archivo, ya que ya se incluye uno de ejemplo con dicho nombre y hay que reemplazarlo.
  - Ejecutar **generate-win.cmd**
- Copiar a la SD el archivo generado test_retracciones.gcode
- Imprimir test_retracciones.gcode
- Abrir el test_retracciones.gcode con Notepad++ y buscar en la tabla para asegurar los valores de los datos que buscamos.

## Requisitos para Linux
- Instalar Python3
- Ejecutar **generate-linux.sh**
o 
- ./retraction-seeker.py > test_retracciones.gcode

### Que es cada parametro



```
{
  "accel_e": 5000,                      # aceleracion maxima (mm/s^2)
  "accel_x": 500,                       # aceleracion maxima (mm/s^2)
  "accel_y": 500,                       # aceleracion maxima (mm/s^2)
  "accel_z": 100,                       # aceleracion maxima (mm/s^2)
  "bed_size_x": 235,                    # ancho de la cama (mm)
  "bed_size_y": 235,                    # largo de la cama (mm)
  "brim_width": 1,                      # ancho extra del ala
  "coord_z_hop": 0.24,                  # Se calcula al vuelo: layer_height + ret_z_hop
  "e_per_mm": 0.018671297535706604,     # Se calcula al vuelo: mm3_per_mm / filament_area
  "fan_spd_initial": 0,                 # velocidad del ventilador en la primera capa
  "fan_spd_other": 169,                 # velocidad del ventilador para otras capas [0-255]
  "feed_e": 25,                         # avance máximo para la extrusora (mm/s)
  "feed_print": 3000,                   # avance al imprimir (mm/min)
  "feed_print_first": 1200,             # avance al imprimir la primera capa (mm/min)
  "feed_print_outer": 3000,             # avance al imprimir la pared exterior (mm/min)
  "feed_travel": 9000,                  # avance cuando desplaza (mm/min) = 15cm*10*60 min
  "feed_x": 500,                        # (mm/s)
  "feed_y": 500,                        # (mm/s)
  "feed_z": 5,                          # (mm/s)
  "feed_z_m": 300,                      # avance para z en (mm/m)
  "feed_z_ret": 300,                    # avance para z en retraccion (mm/m)
  "filament_diam": 1.75,                # (mm)
  "gcode_intro_abl": "",                #
  "gcode_intro_prime": "\nG92 E0 ; Reset Extruder\nG1 Z2.0 F3000 ; Move Z Axis up little to prevent scratching of Heat Bed\nG1 X10.1 Y20 Z0.28 F5000.0 ; Move to start position\nG1 X10.1 Y200.0 Z0.28 F1500.0 E15 ; Draw the first line\nG1 X10.4 Y200.0 Z0.28 F5000.0 ; Move to side a little\nG1 X10.4 Y20 Z0.28 F1500.0 E30 ; Draw the second line\nG92 E0 ; Reset Extruder\nG1 Z2.0 F3000 ; Move Z Axis up little to prevent scratching of Heat Bed\n",
  "last_ret_d": 0,                      #
  "layer_height": 0.12,                 # (mm)
  "line_width": 0.4,                    # Se calcula al vuelo: nozzle_diam * width_multiplier
  "margin_x": 20,                       # (mm)
  "margin_y": 50,                       # (mm)
  "max_tile_span": 12,                  # limita la extensión de mosaico para pasos x/y para recuentos bajos de steps_x/steps_y (mm)
  "mm3_per_mm": 0.044909733552923256,   # Se calcula al vuelo: layer_height * (width - layer_height * (1. - 0.25 * pi))
  "nozzle_area": 0.12566370614359174,   # Se calcula al vuelo: pi * (nozzle_diam / 2)^2)
  "nozzle_diam": 0.4,                   # (mm)
  "ret_d": 4.0,                         # Se calcula al vuelo: ret_d_start + ret_d_step
  "ret_d_start": 4.0,                   # (mm)
  "ret_d_step": 0.5,                    # (mm)
  "ret_spd_start": 20,                  # (mm/s)
  "ret_spd_step": 10,                   # (mm/s)
  "ret_temp_start": 190,                # temperatura de impresion (Celsius)
  "ret_temp_step": -5,                  # cada incremento de z, agregamos esto a la temperatura
  "ret_temp_step_h": 41,                # No. de capas por cambio de temperatura: aproximadamente 5 mm aquí (5/layer_height)
  "ret_z_hop": 0.12,                    # combinar con la configuración de feed_z_ret (mm)
  "square_size": 3,                     # tamaño del lado del pilar cuadrado impreso (mm)
  "steps_x": 7,                         # X == dist, max = start + steps*step (ejemplo 10 pasos para distancia por defecto: 1.0 + 10*0.25 = 3.5)
  "steps_y": 5,                         # Y == spd
  "steps_z": 1,                         # en este caso es 190 C, si fuera 3, seria 190, 185, 180
  "temp_bed": 50,                       # temperatura de la capa
  "temp_nozzle": 190,                   # temperatura inicial del nozzle, se reasignara a cada capa en Z, debe ser la misma que ret_temp_start
  "tile_x_start": 60,                   # Se calcula al vuelo: margin_x
  "tile_x_step": 12,                    # Se calcula al vuelo: (bed_size_x - 2 * margin_x) / steps_x
  "tile_y_start": 50,                   # Se calcula al vuelo: margin_y
  "tile_y_step": 12,                    # Se calcula al vuelo: (bed_size_y - 2 * margin_y) / steps_y
  "width_multiplier": 1.0               # line_width = nozzle_diam * extrusion_multiplier
}
```
