# Algoritmo para encontrar elementos geográficos cercanos: Guía paso a paso con SQL y funciones trigonométricas
Búsqueda por Latitud Longitud - Buscar por posición espacial


Ejemplo de Búsqueda por coordenadas a partir de un punto dado en este enlace:<br>
`<link>` :<http://bogarin.com.mx/tiendita/>

<a href="http://bogarin.com.mx/tiendita/" ><img width="621" alt="imagen" src="https://github.com/theboga/Busqueda_latitud_longitud/assets/9596542/25af6796-7420-41f6-a8f4-da0198e8744f"></a>


basándome en la fórmula del círculo de la Tierra para calcular distancias en kilómetros y en la ayuda de Pablo Blanco



Este algoritmo resuelve un problema común en la actualidad: determinar qué elementos se encuentran dentro de un radio de acción determinado, dada nuestra ubicación geográfica actual y la ubicación geográfica de esos elementos.

Imaginemos que tenemos la información de la ubicación geográfica (latitud y longitud) de varios elementos, como personas, comercios o poblaciones. Queremos saber qué elementos se encuentran dentro de un círculo de radio determinado, con nuestro punto de ubicación como centro. <img width="214" alt="imagen" src="https://github.com/theboga/Busqueda_latitud_longitud/assets/9596542/bcec8b59-1ff5-4cda-89de-09010f33be79">


Para resolver esto, el algoritmo utiliza una base de datos MySQL, donde los elementos están almacenados en una tabla llamada "oxxos". Esta tabla contiene las coordenadas geográficas de cada elemento, representadas por los campos "lat" y "lon". Además, también hay un campo llamado "nombre" que almacena el nombre del elemento.
<img width="809" alt="imagen" src="https://github.com/theboga/Busqueda_latitud_longitud/assets/9596542/913bbed4-c44c-4d75-a8f0-3faf9dfd8e2f">


Supongamos que el radio en el que queremos buscar es de 2 kilómetros, y nuestra ubicación se encuentra en el centro de la ciudad de Guadalajara, con las siguientes coordenadas geográficas:

Centro (latitud, longitud) = (20.673519, -103.354912)

<img width="476" alt="imagen" src="https://github.com/theboga/Busqueda_latitud_longitud/assets/9596542/a281d5b8-85e3-41d9-990d-8e6aaffbcac7">



La sentencia SQL que se utiliza para obtener los elementos deseados es la siguiente:

```sql
SELECT nombre, lat, lon, (6371 * acos(cos(radians(20.673519)) * 
cos(radians(lat)) * cos(radians(lon) - radians(-103.354912)) + sin(radians(20.673519)) * 
sin(radians(lat)))) AS distance FROM oxxos HAVING distance < 2 ORDER BY distance;
```

<img width="1190" alt="imagen" src="https://github.com/theboga/Busqueda_latitud_longitud/assets/9596542/084437a7-c970-4a7e-b983-b176cb10821c">


Ahora, analicemos cómo funciona esta sentencia SQL:

    -La parte "SELECT nombre, lat, lon," indica qué columnas queremos obtener en el resultado. En este caso, se obtendrá el nombre del elemento, su latitud, longitud y también la distancia calculada.

    -La parte "(6371 * acos(cos(radians(20.673519)) * cos(radians(lat)) * cos(radians(lon) - radians(-103.354912)) + sin(radians(20.673519)) * sin(radians(lat))))" calcula la distancia entre nuestra ubicación y cada elemento de la tabla utilizando funciones trigonométricas. Esta fórmula se basa en la fórmula del círculo de la Tierra para calcular distancias en kilómetros.

    -El número 6371 es el radio medio de la tierra expresado en Km. Se usa el radio medio ya que no en todos lados se usa el mismo

    -La parte "AS distance" asigna un nombre a la columna que almacena la distancia calculada.

    -La parte "FROM oxxos" indica que estamos seleccionando los elementos de la tabla llamada "oxxos".

    -La parte "HAVING distance < 2" establece el criterio de búsqueda, en este caso, se seleccionan solo los elementos cuya distancia sea menor a 2 kilómetros.

    -La parte "ORDER BY distance" ordena los resultados según la distancia, de menor a mayor.


_* El valor 6371 utilizado en la fórmula del algoritmo representa el radio medio de la Tierra expresado en kilómetros. Es una aproximación comúnmente aceptada para el radio promedio de nuestro planeta. Al utilizar este valor, podemos calcular distancias en kilómetros de manera más precisa en el contexto del algoritmo._

En resumen, este algoritmo utiliza una sentencia SQL para calcular la distancia entre nuestra ubicación y cada elemento de la tabla, utilizando funciones trigonométricas. Luego, filtra los elementos que se encuentran dentro del radio deseado y los ordena según su distancia. Esto nos permite obtener los elementos deseados que se encuentran dentro del radio de acción especificado.


<img width="936" alt="imagen" src="https://github.com/theboga/Busqueda_latitud_longitud/assets/9596542/2d1e33b6-b07a-42b7-a9e3-38de553d8e6a">






