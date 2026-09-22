# Comandos de UNIX ||

## Entrada/Salida en la ejecución de comandos

### Entrada de un comando

En la propia línea de comandos

```
$: cat fichero.txt
```

### Entrada estándar 

`stdin`

Durante la ejecución de un comando. Puede entrar mediante redirectores (`<`), tuberías (`|`)...

### Salida de un comando

Es un número que indica el estado de la salida (si ha sido exitoso, fallido...) Puede verse con el comando `echo $?`

### Salida estándar

`stdout`

Durante la ejecución de un comando. Puede mandarse la salida a un archivo mediante redirectores (`>`), tuberías (`|`)...

---

### Salida de errores de un comando

Es el dispositivo hacia el que se envían los mensajes de error en caso de que se generen en un comando.

### Salida de errores estándar

`stderr`

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

`head/tail` : muestran las N primeras/últimas líneas con el parámetro `-n`