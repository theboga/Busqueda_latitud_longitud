# Algoritmo para encontrar elementos geográficos cercanos: Guía paso a paso con SQL y funciones trigonométricas
Búsqueda por Latitud Longitud - Buscar por posición espacial


Ejemplo de Búsqueda por coordenadas a partir de un punto dado
[http://www.bogarin.com.mx/tiendita](http://bogarin.com.mx/tiendita/)

basándome en la fórmula del círculo de la Tierra para calcular distancias en kilómetros y en la ayuda de Pablo Blanco



Este algoritmo resuelve un problema común en la actualidad: determinar qué elementos se encuentran dentro de un radio de acción determinado, dada nuestra ubicación geográfica actual y la ubicación geográfica de esos elementos.

Imaginemos que tenemos la información de la ubicación geográfica (latitud y longitud) de varios elementos, como personas, comercios o poblaciones. Queremos saber qué elementos se encuentran dentro de un círculo de radio determinado, con nuestro punto de ubicación como centro.

Para resolver esto, el algoritmo utiliza una base de datos MySQL, donde los elementos están almacenados en una tabla llamada "mitabla". Esta tabla contiene las coordenadas geográficas de cada elemento, representadas por los campos "lat" y "lon". Además, también hay un campo llamado "nombre" que almacena el nombre del elemento.

Supongamos que el radio en el que queremos buscar es de 2 kilómetros, y nuestra ubicación se encuentra en el centro de la ciudad de Guadalajara, con las siguientes coordenadas geográficas:

Centro (latitud, longitud) = (20.673519, -103.354912)

La sentencia SQL que se utiliza para obtener los elementos deseados es la siguiente:

SELECT nombre, lat, lon, (6371 * acos(cos(radians(20.673519)) * cos(radians(lat)) * cos(radians(lon) - radians(-103.354912)) + sin(radians(20.673519)) * sin(radians(lat)))) AS distance FROM mitabla HAVING distance < 2 ORDER BY distance;

Ahora, analicemos cómo funciona esta sentencia SQL:

    La parte "SELECT nombre, lat, lon," indica qué columnas queremos obtener en el resultado. En este caso, se obtendrá el nombre del elemento, su latitud, longitud y también la distancia calculada.

    La parte "(6371 * acos(cos(radians(20.673519)) * cos(radians(lat)) * cos(radians(lon) - radians(-103.354912)) + sin(radians(20.673519)) * sin(radians(lat))))" calcula la distancia entre nuestra ubicación y cada elemento de la tabla utilizando funciones trigonométricas. Esta fórmula se basa en la fórmula del círculo de la Tierra para calcular distancias en kilómetros.

    La parte "AS distance" asigna un nombre a la columna que almacena la distancia calculada.

    La parte "FROM mitabla" indica que estamos seleccionando los elementos de la tabla llamada "mitabla".

    La parte "HAVING distance < 2" establece el criterio de búsqueda, en este caso, se seleccionan solo los elementos cuya distancia sea menor a 2 kilómetros.

    La parte "ORDER BY distance" ordena los resultados según la distancia, de menor a mayor.

En resumen, este algoritmo utiliza una sentencia SQL para calcular la distancia entre nuestra ubicación y cada elemento de la tabla, utilizando funciones trigonométricas. Luego, filtra los elementos que se encuentran dentro del radio deseado y los ordena según su distancia. Esto nos permite obtener los elementos deseados que se encuentran dentro del radio de acción especificado.


Mi Posición : Latitud -> 20.673519, Longitud -> -103.354912

50YNM DEPORTIVO INDEPENDENCIA GDL SIETE COLINAS 20.69453503 -103.33288743
CADENA COMERCIAL OXXO MARTIN MACIAS 20.72763470 -103.32526888
COMERCIO AL POR MENOR EN MINISÚPER PLUTARCO ELÍAS CALLES 20.68828484 -103.29063168
GASOLINERA OXXO SIERRA NEVADA 20.68623177 -103.33553017
OXXO ISLA ANTIGUA 20.63560773 -103.38641934
OXXO ESTADIO 20.66332885 -103.34797279
OXXO GAS SUC. 5 DE FEBRERO 5 DE FEBRERO 20.66217271 -103.34589365
OXXO GAS SUC. CRISTO REY GDL Gobernador Luis G. Curiel 20.64454984 -103.35223196
OXXO GAS SUC. RIVAS GUILLEN GDM CIRCUNVALACIÓN OBLATOS 20.69031071 -103.30374890
OXXO SUC 500N6 JUAN ZUBARAN DIVISIÓN DEL NORTE 20.70245688 -103.34184194
OXXO SUC 501A0 LAFAYETTE GDL FRANCISCO JAVIER GAMBOA 20.67267004 -103.37716381
OXXO SUC 501J8 HOTEL ONE GDL LÓPEZ MATEOS SUR 20.65756157 -103.39509984
OXXO SUC 5022B MIGUEL OROZCO GDL JESÚS REYES HEROLES 20.62139355 -103.37942684
OXXO SUC 502TB HDA DE TALA GDL JOAQUÍN AMARO 20.69309816 -103.29236146
