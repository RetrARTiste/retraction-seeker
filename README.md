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

##Que va a generar este codigo?

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

###Distancia de retraccion:
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

###Velocidad de retraccion:
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


###Temperatura del nozzle:
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
- Descargar este codigo https://github.com/shawe/retraction-seeker/archive/master.zip
  - Para los menos experimentados, descomprimirlo directamente en C:, la explicacion se asumira como hecha seguida asi.
  - Escoger de **C:\retraction-seeker-master\config_sample** el archivo json mas adecuado, y copiarlo en **C:\retraction-seeker-master\settings.json**
    - Importante respetar el nombre del archivo, ya que ya se incluye uno de ejemplo con dicho nombre y hay que reemplazarlo.
- Abrir un terminal (Powershell en Windows 10, por ejemplo) y ejecutar:
  - cd C:\retraction-seeker-master
  - py.exe -u retraction-seeker.py > test_retracciones.gcode
    - Si genera un archivo vacio (0 bytes), algo se ha hecho mal en los pasos anteriores. Releer con atencion los puntos anteriores. 
- Copiar a la SD el archivo generado test_retracciones.gcode
- Imprimir test_retracciones.gcode
- Abrir el test_retracciones.gcode con un editor de texto tipo Bloc de Notas de Windows y buscar la tabla para asegurar los valores de los datos que buscamos.

## Requisitos para Linux
- Instalar Python3
- ./retraction-seeker.py > test_retracciones.gcode
