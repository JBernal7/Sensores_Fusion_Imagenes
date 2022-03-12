Sensores: preprocesado, corrección y fusión de imágenes (I)

# FUSIÓN DE IMÁGENES LANDSAT 8

Jessica Bernal Borrego
26/02/22





##### En esta tarea se va a llevar a cabo un proceso de fusión de imágenes de un algoritmo no implementado en QGIS, indicando el desarrollo del algoritmo de fusión paso a paso para que podamos comprender cómo se extrae la información espacial de la imagen pancromática y cómo se fusiona con la información espectral de la imagen multiespectral.
##### Para ello, se empleará un proceso de pansharpening con el método Á TROUS. La fusión se va a aplicar sobre un corte de imagen actual de Landsat 8 que contiene la zona donde en septiembre de 2021 se produjo el incendio de Sierra Bermeja, que afectó a casi 10.000 ha de terreno de la provincia malagueña. Se utilizará la imagen multiespectral formada por 7 bandas con un tamaño de píxel de 30 m y la imagen pancromática de una única banda con  un tamaño de píxel de 15m.


### 1. Preparación
##### Cargamos las bandas que vamos a utilizar: las 7 bandas multiespectrales (bandas B1-B7) y la banda pancromática (B8) del satélite Landsat 8 (Colección 2, nivel 1).

 
![image](https://user-images.githubusercontent.com/100314590/158029689-ca337aec-952a-4cf5-b763-5a860f4a7429.png)




### 2. Remuestreo 
##### Necesitamos que las imágenes se encuentren en la misma resolución espacial, concretamente en la resolución que buscamos con el proceso de fusión. Esto implica el remuestreo de las bandas multiespectrales (30 m) al tamaño de la pancromática (15 m). 
##### Para este paso utilizamos la calculadora ráster (*Ráster → Calculadora ráster*), indicando en la extensión de la capa seleccionada el número de columnas y filas de la capa de salida, de forma que este coincida con la capa B8. En este caso, nuestra capa B8 contiene 15621 columnas y 15881 filas.
 

![image](https://user-images.githubusercontent.com/100314590/158029705-93835de6-ac58-4b42-b41c-9864e62940b1.png)


### 3. Comprobación
##### Comprobamos (1) que los resultados son los esperados, 
![image](https://user-images.githubusercontent.com/100314590/158029714-ae806b40-cae8-462e-9981-0cbf35e4fed6.png)
![image](https://user-images.githubusercontent.com/100314590/158029721-c9f3453b-03d2-4b94-a9e0-efbb0e88bd4d.png)


##### Y (2) que no existe desplazamiento de las imágenes remuestreadas respecto a la pancromática. Para ello simplemente tomamos unos puntos de referencia en la pancromática y vamos superponiendo las multiespectrales remuestradas.

![image](https://user-images.githubusercontent.com/100314590/158029724-97b21fb7-4566-47d5-a21e-7443c5d77787.png)


 

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


> ![image](https://user-images.githubusercontent.com/100314590/158029732-17f30522-16c0-45a5-948e-fc127f45f676.png)


###### Donde, 

> ![image](https://user-images.githubusercontent.com/100314590/158029737-b91b0216-fb09-4cbe-b823-8a5ff0586953.png)


##### Por tanto, apoyándonos en una hoja de cálculo, calculamos los coeficientes a y b a partir de la media y desviación típica de cada banda:

![image](https://user-images.githubusercontent.com/100314590/158029740-3dffc02b-6723-4766-bf31-ae3f44d891f3.png)



##### Y procedemos al matcheado de las imágenes (*Ráster --> Calculadora ráster*). 

![image](https://user-images.githubusercontent.com/100314590/158029746-db843342-6af4-47e3-a3c6-69fbab7cba70.png)



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

> ![image](https://user-images.githubusercontent.com/100314590/158029758-6cde8846-3aed-4fd3-9334-db43225ab826.png)


##### En nuestro ejercicio el filtro suavizado de primer nivel (2:1) es el siguiente: 

![image](https://user-images.githubusercontent.com/100314590/158029760-54d06efd-6a92-4389-bb1f-856509c82a75.png)


##### Ejecutamos esta operación a través de la herramienta *r.mfilter* de GRASS:
 
![image](https://user-images.githubusercontent.com/100314590/158029765-93d27988-dea4-4f15-8cfc-ae605d23d3ff.png)

 
![image](https://user-images.githubusercontent.com/100314590/158029769-6114325e-3d85-4fb7-8327-fe5afae171c5.png)

![image](https://user-images.githubusercontent.com/100314590/158029773-78f9f95c-b4cd-4556-b787-a75d2d1ce0ac.png)

  
###### Pancromática Matcheada (izq) y Pancromática Filtrada (der)


##### A continuación, restamos los valores obtenidos (imagen pancromática filtrada) a la imagen original (pancromática matcheada), obteniendo así el detalle de cada banda.
> ![image](https://user-images.githubusercontent.com/100314590/158029776-0773829d-a37f-4bdf-9ac1-a5ead22fba06.png)

![image](https://user-images.githubusercontent.com/100314590/158029777-be2b2a90-bf22-43a3-a4e8-81834b8b9e25.png)

![image](https://user-images.githubusercontent.com/100314590/158029781-bcc05ac5-500e-4d2c-8550-7d836bd59e7c.png)

###### Pancromática Matcheada (izq) y Pancromática Detalle (der)

![image](https://user-images.githubusercontent.com/100314590/158029788-ee8afe27-eb26-4b55-94a1-37975ea3518e.png)

  
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


> ![image](https://user-images.githubusercontent.com/100314590/158029797-15c23be9-eaca-4271-8dcc-1cdca878fe9b.png)


![image](https://user-images.githubusercontent.com/100314590/158029799-4221eb45-3e6e-4873-9760-1b4c860eab7a.png)

 

### 6. Resultado
##### Construimos dos ráster virtuales para comparar el grado de detalle que la imagen fusionada incluye a la imagen multiespectral. Este detalle es el equivalente al que ofrece la imagen pancromática, que se encuentra con un tamaño de píxel de 15 m.

![image](https://user-images.githubusercontent.com/100314590/158029808-1e11e0ce-0cee-4219-9238-e221909f5660.png)

 
###### Imagen multiespectral de origen

![image](https://user-images.githubusercontent.com/100314590/158029814-d80d7b89-d8cc-46c6-99e0-9ed9438c2e2f.png)

###### Imagen multiespectral fusionada
