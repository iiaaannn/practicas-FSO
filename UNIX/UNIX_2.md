# Comandos de UNIX ||

## Entrada/Salida en la ejecución de comandos

### Entrada de un comando

En la propia línea de comandos

```
$: cat fichero.txt
```

### Entrada estándar 

`stdin`. Se identifica con el descriptor de archivos `0`

Entra por el teclado. Puede entrar también mediante redirectores (`<`), tuberías (`|`)...

### Salida de un comando

Es un número que indica el estado de la salida (si ha sido exitoso, fallido...) Puede verse con el comando `echo $?`

### Salida estándar

`stdout`. Se identifica con el descriptor de archivos `1`

Sale por la pantalla (terminal) durante la ejecución de un comando. Puede mandarse la salida a un archivo mediante redirectores (`>`), tuberías (`|`)...

---

### Salida de errores de un comando

Es el dispositivo hacia el que se envían los mensajes de error en caso de que se generen en un comando.

### Salida de errores estándar

`stderr`. Se identifica con el descriptor de archivos `2`

Es la pantalla por donde se muestra el error.

> [!NOTE]
> No todos los comandos tienen entrada y/o salida. Pero todos los comandos tienen salida estándar de errores

## Comandos avanzados

`sort fichero.txt` : muestra el contenido del fichero ordenado alfabéticamente.

`grep` : busca cadenas de caractéres en la salida estándar y las muestra por pantalla. En grep se suelen utilizar <u>**expresiones regulares**</u>. Algunas herramientas para generarlas son : 

- `.` : coincide con cualquier caracter
- `[]` : define un conjunto de caracteres (funciona como un OR). También se pueden establecer rangos, por ejemplo : `[ab]` significa "a" o "b", y `[a-z]` significa todas las minúsculas
- `*-` : significa la repetición de 0 o más veces el patrón anterior. Por ejemplo `a*` es .a repetición de la letra "a" 0 o más veces, `aa*` 1 o más veces...

> [!IMPORTANT]
> No confundir con el metacaracter `*`

- `^-` : busca las líneas que empiecen por ese caracter. Por ejemplo `^[aeiouAEIOU]` filtra por las líneas que empiecen por una vocal.

`uniq fichero.txt` : muestra todas las líneas una sola vez (las repetidas no las muestra)

> [!IMPORTANT]
> Las líneas deben estar ordenadas con `sort`, sino `uniq` no surte efecto

`head/tail` : muestran las N primeras/últimas líneas con el parámetro `-n`

`more fich.txt` : es similar al cat, pero si es un archivo muy grande, cat te muestra todo el contenido de golpe. More lo que hace es mostrártelo en formato página, para evitar precisamente lo que hace cat, y poder pasar páginas cómodamente.

`paste fich1.txt fich2.txt` : este comando une las líneas de varios archivos en paralelo pegándolas columna por columna y separándolas por un delimitador.

`wc fich.txt` : cuenta el número de líneas que tiene el archivo.

`tr char1 char2` : sustituye el char1 por el char2 (pueden ser conjuntos de caracteres también)

- `-d char1:` : elimina el char1

`cut fichero.txt` : permite seleccionar una columna de un fichero mediante el uso de un delimitador.

- `-d delimitador` : selecciona un delimitador para poder seleccionar la columna más cómodamente.
- `-c N-M` : permite seleccionar un caracter (o rangos de caracteres) en concreto de una línea.
- `-f N` : permite seleccionar la columna nº N una vez separadas mediante el delimitador especificado en `-d`

## Redirecciones

En UNIX podemos cambiar el dispositivo de entrada, salida y salida de comandos por otros (por lo general en ficheros). Esto es posible gracias a que en UNIX TODO son ficheros (ver `/dev`).

### Redirecciones de entrada

> [!IMPORTANT]
> El comando debe tener entrada estándar

Ejemplo :

```sh
tr a A < fich1.txt # sustituye todas las a's minúsculas por A's mayúsculas en el fichero
# fich1.txt y saca la salida por la salida estándar
```

### Redirecciones de salida

Ejemplo : 

```sh
ls -l /dev > fich.txt # manda la salida del comando  'ls -l' al archivo fich.txt
```

```sh
echo "Añadimos esto al final del archivo" >> fich.txt # a diferencia del redirector '>',
# '>>' añade al final del archivo la salida estándar, y no borra el contenido que ya
# tenía (a diferencia de '>')
```

### Redirección de salida de errores

La salida de errores estándar (`stderr`) se almacena en el descriptor de ficheros `2`. Por lo tanto si queremos mandar la salida de errores a un archivo, lo haremos  de la siguiente manera : 

```sh
grep -w "hola" fich.txt 2> errores # filtramos por la palabra "hola" en el archivo 
# fich.txt y los errores que puedan ocurrir los mandamos al fichero "errores"
```

```sh
find / -name "busqueda" -maxdepth 1 2>> errores # busca en la raíz del sistema un
# archivo llamado "busqueda" y los errores los añade al final del archivo "errores"
```

> [!NOTE]
> Podemos mandar la salida de errores estándar y la salida estándar juntas al mismo fichero

```sh
find /home -maxdepth 2 > salida.txt 2>&1
```

Aquí por ejemplo lo que hacemos es que la salida estándar la redirigimos al fichero "salida.txt" (`> salida.txt`). Y posteriormente lo que hacemos es algo como "quiero que la salida de errores me la mandes donde vaya la salida estándar" (`2>&1`, el 1 es el descriptor de archivos que identifica al `stdout` (salida estándar). Ver los descriptores de archivo al principio de este archivo), y por ende también va a "salida.txt".

## Tuberías

Se conocen también como ***pipelines***. Lo que hacen es concatenar ejecuciones de comandos de forma que la salida de uno es la entrada de otro y así sucesivamente... Las tuberías se identifican mediante el símbolo `|`

> [!IMPORTANT]
> El comando a la izquierda de la tubería debe tener salida estándar y el de la derecha de la tubería entrada estándar.

Ejemplo : 

```sh
grep “patron” fich | sort | uniq # filtra por la palabra "patron" en fich, las ordena
# alfabéticamente y elimina las repetidas
```

---

`scp origen destino` : ***Secure Copy Protocol*** es un comando de UNIX que nos permite la transferencia segura de archivos entre sistemas a través de SSH.

Ejemplo :

```sh
scp prueba.txt usuario_prueba@host_prueba:/home/usuario_prueba/ # estamos copiando el 
# archivo local y lo estamos dejando en el servidor host_prueba en la ruta 
# /home/usuario_prueba/
``` 

Y puede hacerse al revés, descargarse algo desde el host hacia la máquina local y cambiarle el nombre también. Para especificarle un puerto en concreto se usa el parámetro `-P`
