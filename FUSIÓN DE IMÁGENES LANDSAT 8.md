Sensores: preprocesado, corrección y fusión de imágenes (I)

# FUSIÓN DE IMÁGENES LANDSAT 8

Jessica Bernal Borrego
26/02/22





##### En esta tarea se va a llevar a cabo un proceso de fusión de imágenes de un algoritmo no implementado en QGIS, indicando el desarrollo del algoritmo de fusión paso a paso para que podamos comprender cómo se extrae la información espacial de la imagen pancromática y cómo se fusiona con la información espectral de la imagen multiespectral.
##### Para ello, se empleará un proceso de pansharpening con el método Á TROUS. La fusión se va a aplicar sobre un corte de imagen actual de Landsat 8 que contiene la zona donde en septiembre de 2021 se produjo el incendio de Sierra Bermeja, que afectó a casi 10.000 ha de terreno de la provincia malagueña. Se utilizará la imagen multiespectral formada por 7 bandas con un tamaño de píxel de 30 m y la imagen pancromática de una única banda con  un tamaño de píxel de 1 5m.


### 1. Preparación
##### Cargamos las bandas que vamos a utilizar: las 7 bandas multiespectrales (bandas B1-B7) y la banda pancromática (B8) del satélite Landsat 8 (Colección 2, nivel 1).

 
![](https://codimd.s3.shivering-isles.com/demo/uploads/55c84c826662ee144f2d26f13.png)




### 2. Remuestreo 
##### Necesitamos que las imágenes se encuentren en la misma resolución espacial, concretamente en la resolución que buscamos con el proceso de fusión. Esto implica el remuestreo de las bandas multiespectrales (30 m) al tamaño de la pancromática (15 m). 
##### Para este paso utilizamos la calculadora ráster (*Ráster → Calculadora ráster*), indicando en la extensión de la capa seleccionada el número de columnas y filas de la capa de salida, de forma que este coincida con la capa B8. En este caso, nuestra capa B8 contiene 15621 columnas y 15881 filas.
 

![](https://codimd.s3.shivering-isles.com/demo/uploads/55c84c826662ee144f2d26f14.png)


### 3. Comprobación
##### Comprobamos (1) que los resultados son los esperados, 

 ![](https://codimd.s3.shivering-isles.com/demo/uploads/55c84c826662ee144f2d26f15.png)

 ![](https://codimd.s3.shivering-isles.com/demo/uploads/55c84c826662ee144f2d26f16.png)


##### Y (2) que no existe desplazamiento de las imágenes remuestreadas respecto a la pancromática. Para ello simplemente tomamos unos puntos de referencia en la pancromática y vamos superponiendo las multiespectrales remuestradas.

 ![](https://codimd.s3.shivering-isles.com/demo/uploads/55c84c826662ee144f2d26f17.png)


 

### 4. Ajuste del histograma (matcheado) 
##### Buscando que la espectralidad de las bandas B1 a B7 disminuya lo mínimo en el proceso, ajustamos la misma en la imagen pancromática de forma que su histograma se adapte a las características espectrales de cada una de las bandas multiespectrales. 
##### Consultamos las estadísticas básicas las imágenes en Ráster → Miscelánea → Información del ráster

~~~
Driver: GTiff/GeoTIFF
Files: C:/Geodirectorio/Geodatos/Sierra_Bermeja/Landsat/Feb_22/LC08_L1TP_201035_20220216_20220223_02_T1_B8.TIF
       C:/Geodirectorio/Geodatos/Sierra_Bermeja/Landsat/Feb_22\LC08_L1TP_201035_20220216_20220223_02_T1_MTL.txt
Size is 15621, 15881
Coordinate System is:
PROJCRS["WGS 84 / UTM zone 30N",
    BASEGEOGCRS["WGS 84",
        DATUM["World Geodetic System 1984",
            ELLIPSOID["WGS 84",6378137,298.257223563,
                LENGTHUNIT["metre",1]]],
        PRIMEM["Greenwich",0,
            ANGLEUNIT["degree",0.0174532925199433]],
        ID["EPSG",4326]],
    CONVERSION["UTM zone 30N",
        METHOD["Transverse Mercator",
            ID["EPSG",9807]],
        PARAMETER["Latitude of natural origin",0,
            ANGLEUNIT["degree",0.0174532925199433],
            ID["EPSG",8801]],
        PARAMETER["Longitude of natural origin",-3,
            ANGLEUNIT["degree",0.0174532925199433],
            ID["EPSG",8802]],
        PARAMETER["Scale factor at natural origin",0.9996,
            SCALEUNIT["unity",1],
            ID["EPSG",8805]],
        PARAMETER["False easting",500000,
            LENGTHUNIT["metre",1],
            ID["EPSG",8806]],
        PARAMETER["False northing",0,
            LENGTHUNIT["metre",1],
            ID["EPSG",8807]]],
    CS[Cartesian,2],
        AXIS["(E)",east,
            ORDER[1],
            LENGTHUNIT["metre",1]],
        AXIS["(N)",north,
            ORDER[2],
            LENGTHUNIT["metre",1]],
    USAGE[
        SCOPE["Engineering survey, topographic mapping."],
        AREA["Between 6°W and 0°W, northern hemisphere between equator and 84°N, onshore and offshore. Algeria. Burkina Faso. Côte' Ivoire (Ivory Coast). Faroe Islands - offshore. France. Ghana. Gibraltar. Ireland - offshore Irish Sea. Mali. Mauritania. Morocco. Spain. United Kingdom (UK)."],
        BBOX[0,-6,84,0]],
    ID["EPSG",32630]]
Data axis to CRS axis mapping: 1,2
Origin = (193192.500000000000000,4109407.500000000000000)
Pixel Size = (15.000000000000000,-15.000000000000000)
Metadata:
  AREA_OR_POINT=Point
  METADATATYPE=ODL
Image Structure Metadata:
  COMPRESSION=DEFLATE
  INTERLEAVE=BAND
Corner Coordinates:
Upper Left  (  193192.500, 4109407.500) (  6d27' 4.62"W, 37d 4'50.76"N)
Lower Left  (  193192.500, 3871192.500) (  6d21'32.28"W, 34d56'12.62"N)
Upper Right (  427507.500, 4109407.500) (  3d48'57.98"W, 37d 7'41.57"N)
Lower Right (  427507.500, 3871192.500) (  3d47'39.22"W, 34d58'50.57"N)
Center      (  310350.000, 3990300.000) (  5d 6'18.44"W, 36d 2'19.61"N)
Band 1 Block=256x256 Type=UInt16, ColorInterp=Gray
  Minimum=5329.000, Maximum=65495.000, Mean=6908.955, StdDev=999.631
  NoData Value=0
  Overviews: 7811x7941, 3906x3971, 1953x1986, 977x993, 489x497, 245x249
  Metadata:
    STATISTICS_MAXIMUM=65495
    STATISTICS_MEAN=6908.9553091259
    STATISTICS_MINIMUM=5329
    STATISTICS_STDDEV=999.63095052652
    STATISTICS_VALID_PERCENT=67.03
~~~


##### Para igualar el histograma de imagen pancromática con los histogramas de las distintas bandas de la multiespectral se utiliza la siguiente expresión:


> ![](https://codimd.s3.shivering-isles.com/demo/uploads/55c84c826662ee144f2d26f18.png)


###### Donde, 

> ![](https://codimd.s3.shivering-isles.com/demo/uploads/55c84c826662ee144f2d26f19.png)


##### Por tanto, apoyándonos en una hoja de cálculo, calculamos los coeficientes a y b a partir de la media y desviación típica de cada banda:

 ![](https://codimd.s3.shivering-isles.com/demo/uploads/55c84c826662ee144f2d26f1a.png)



##### Y procedemos al matcheado de las imágenes (*Ráster --> Calculadora ráster*). 


 ![](https://codimd.s3.shivering-isles.com/demo/uploads/55c84c826662ee144f2d26f1b.png)



##### El resultado son 7 bandas pancromáticas con el histograma ajustado a cada una de las 7 bandas multiespectrales, como podemos comprobar consultando los estadísticos de estas nuevas bandas (imagen ejemplo con B1):

~~~
Files: C:\Geodirectorio\Geodatos\Sierra_Bermeja\Landsat\Feb_22\PAN\PAN_B1_m.tif

Metadata:
    STATISTICS_MAXIMUM=31606.73046875
    STATISTICS_MEAN=8798.1321107628
    STATISTICS_MINIMUM=8183.0234375
    STATISTICS_STDDEV=389.17927373001
    STATISTICS_VALID_PERCENT=67.02

Files: C:/Geodirectorio/Geodatos/Sierra_Bermeja/Landsat/Feb_22/LC08_L1TP_201035_20220216_20220223_02_T1_B1.TIF

Metadata:
    STATISTICS_MAXIMUM=42473
    STATISTICS_MEAN=8798.1264172807
    STATISTICS_MINIMUM=7721
    STATISTICS_STDDEV=389.17402433055
    STATISTICS_VALID_PERCENT=66.94


~~~

### 5. Filtro de paso bajo 
##### En el siguiente paso aplicamos una transformación lineal de los datos para maximizar el detalle de la información. Conviene usar el filtro cuya forma se adecúe mejor al tipo de señal con la que se trabaja, en nuestro caso utilizamos un filtro wavelet de nivel 1, dado que el ratio de cambio de las imágenes es 2:1 (de 30 m se pasa a 15 m).

> ![](https://codimd.s3.shivering-isles.com/demo/uploads/55c84c826662ee144f2d26f1c.png)


##### En nuestro ejercicio el filtro suavizado de primer nivel (2:1) es el siguiente: 

 ![](https://codimd.s3.shivering-isles.com/demo/uploads/55c84c826662ee144f2d26f1d.png)


##### Ejecutamos esta operación a través de la herramienta *r.mfilter* de GRASS:
 
![](https://codimd.s3.shivering-isles.com/demo/uploads/55c84c826662ee144f2d26f1e.png)

 
![](https://codimd.s3.shivering-isles.com/demo/uploads/55c84c826662ee144f2d26f1f.png)

![](https://codimd.s3.shivering-isles.com/demo/uploads/55c84c826662ee144f2d26f20.png)

  
###### Pancromática Matcheada (izq) y Pancromática Filtrada (der)


##### A continuación, restamos los valores obtenidos (imagen pancromática filtrada) a la imagen original (pancromática matcheada), obteniendo así el detalle de cada banda.
> ![](https://codimd.s3.shivering-isles.com/demo/uploads/55c84c826662ee144f2d26f21.png)

![](https://codimd.s3.shivering-isles.com/demo/uploads/55c84c826662ee144f2d26f22.png)

 ![](https://codimd.s3.shivering-isles.com/demo/uploads/55c84c826662ee144f2d26f23.png)

###### Pancromática Matcheada (izq) y Pancromática Detalle (der)

![](https://codimd.s3.shivering-isles.com/demo/uploads/55c84c826662ee144f2d26f24.png)

  
###### Pancromática Matcheada (izq) y Pancromática Detalle (der) con zoom.



##### Para comprobar que el proceso se ha realizado correctamente, consultamos que la media estadística de las bandas de detalle se aproxima al valor 0 (*Ráster → Miscelánea → Información del ráster*)

~~~
Files: C:\Geodirectorio\Geodatos\Sierra_Bermeja\Landsat\Feb_22\PAN\Detalle_B1.tif
STATISTICS_MEAN=-0.0089465014205069


C:\Geodirectorio\Geodatos\Sierra_Bermeja\Landsat\Feb_22\PAN\Detalle_B2.tif
STATISTICS_MEAN=-0.0082255279568767

C:\Geodirectorio\Geodatos\Sierra_Bermeja\Landsat\Feb_22\PAN\Detalle_B3.tif
STATISTICS_MEAN=-0.0074519925439527

C:\Geodirectorio\Geodatos\Sierra_Bermeja\Landsat\Feb_22\PAN\Detalle_B4.tif
STATISTICS_MEAN=-0.0071073709880201

C:\Geodirectorio\Geodatos\Sierra_Bermeja\Landsat\Feb_22\PAN\Detalle_B5.tif
STATISTICS_MEAN=-0.0097772677309411

C:\Geodirectorio\Geodatos\Sierra_Bermeja\Landsat\Feb_22\PAN\Detalle_B6.tif
STATISTICS_MEAN=-0.0087573083483644

C:\Geodirectorio\Geodatos\Sierra_Bermeja\Landsat\Feb_22\PAN\Detalle_B7.tif
STATISTICS_MEAN=-0.0074951656319066
~~~
	
### 6. Obtención de la imagen fusionada
##### Y ahora sí, procedemos a obtener la imagen fusionada sumando el detalle obtenido de la imagen pancromática matcheada a la imagen multiespectral remuestreada.


> ![](https://codimd.s3.shivering-isles.com/demo/uploads/55c84c826662ee144f2d26f25.png)


![](https://codimd.s3.shivering-isles.com/demo/uploads/55c84c826662ee144f2d26f26.png)

 

### 6. Resultado
##### Construimos dos ráster virtuales para comparar el grado de detalle que la imagen fusionada incluye a la imagen multiespectral. Este detalle es el equivalente al que ofrece la imagen pancromática, que se encuentra con un tamaño de píxel de 15 m.

![](https://codimd.s3.shivering-isles.com/demo/uploads/55c84c826662ee144f2d26f27.png)

 
###### Imagen multiespectral de origen

![](https://codimd.s3.shivering-isles.com/demo/uploads/55c84c826662ee144f2d26f28.png)

###### Imagen multiespectral fusionada
