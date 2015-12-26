# mkcal

El script `mkcal` genera un calendario en formato PDF con la siguiente estructura:

* Una tapa o carátula en la primera página, opcional.
* Una página para cada mes, donde se muestran los días del mes y de una a cuatro fotos, automáticamente compaginadas.

## Modo de uso

`mkcal` trabaja con el concepto de *proyectos*. Un proyecto es simplemente un directorio conteniendo:

1. Un subdirectorio para cada mes.
2. Una imagen de tapa (opcional).
3. Un subdirectorio de plantillas.

Puede crear un nuevo proyecto vacío con el siguiente comando:

```
mkcal -c <proyecto>
```

Una vez que se tiene el proyecto armado, con todas las fotos ubicadas, sólo hay que ejecutar:

```
mkcal <proyecto>
```

donde `<proyecto>` es el nombre del directorio con el proyecto de calendario.
Este comando creará el archivo `<proyecto>/calendario_<proyecto>.pdf` con el resultado final.

Asegúrese de que `mkcal` esté ubicado en un directorio incluido en la variable de entorno `PATH`.

## Cómo funciona

`mkcal` recorrerá, para cada mes, el subdirectorio correspondiente. Tomará hasta cuatro fotos de este subdirectorio y las combinará con la plantilla de ese mes para crear una página del calendario. Finalmente, todo se unirá en un único documento PDF con 12 o 13 páginas, dependiendo si se incluyó una imagen para la tapa.

### Disposición de las imágenes en cada mes

Para cada mes `mkcal` usará 1, 2, 3 o 4 imágenes, dependiendo de cuántas haya disponibles en el subdirectorio del mes.

* Si hay una sola foto, ésta ocupará todo el espacio disponible para imágenes.
* Si has dos fotos, éstas se colocarán una bajo la otra o una al lado de la otra, dependiendo si se eligió disposición horizontal o vertical para este mes (ver siguiente sección).
* Si hay tres fotos, la primera (en orden alfabético) será la principal, y las siguientes dos las menores. La principal ocupará toda la sección superior o izquierda, dependiendo de qué disposición se haya elegido: horizontal o vertical.
* Si hay cuatro fotos, las mismas se dispondrán en una grilla de 2x2 en el lugar disponible.

#### Una imagen

```
+---------+---------+
|                   |
|                   |
|                   |
|                   |
|                   |
+         1         +
|                   |
|                   |
|                   |
|                   |
|                   |
+---------+---------+
```

#### Dos imágenes

```
       Vertical               Horizontal
+-------------------+    +-------------------+
|         |         |    |                   |
|         |         |    |                   |
|         |         |    |         1         |
|         |         |    |                   |
|         |         |    |                   |
|    1    |    2    |    +-------------------+
|         |         |    |                   |
|         |         |    |                   |
|         |         |    |         2         |
|         |         |    |                   |
|         |         |    |                   |
+-------------------+    +-------------------+
```

#### Tres imágenes

```
       Vertical               Horizontal
+---------+---------+    +-------------------+
|         |         |    |                   |
|         |         |    |                   |
|         |    2    |    |         1         |
|         |         |    |                   |
|         |         |    |                   |
|    1    +---------+    +---------+---------+
|         |         |    |         |         |
|         |         |    |         |         |
|         |    3    |    |    2    |    3    |
|         |         |    |         |         |
|         |         |    |         |         |
+---------+---------+    +---------+---------+
```

#### Cuatro imágenes

```
+---------+---------+
|         |         |
|         |         |
|    1    |    2    |
|         |         |
|         |         |
+---------+---------+
|         |         |
|         |         |
|    3    |    4    |
|         |         |
|         |         |
+---------+---------+
```

### Los subdirectorios para cada mes

Los subdirectorios para cada mes simplemente tienen como nombre el número de mes. Para los meses con número de una cifra (de enero a septiembre), el nombre del directorio puede contener un cero a la izquierda, pero no es necesario. Esto ayuda a que los meses se listen en el orden correcto.

Además, el nombre de cada uno de estos directorios puede terminar con una `h` o una `v`, para indicar si se desea una disposición horizontal o vertical de la imagen principal. Ver la sección anterior, acerca de la disposición de las imágenes.

### La imagen de tapa

Si `mkcal` encuentra un archivo cuyo nombre cumple con la expresión regular `tapa.*\.(pdf|png|gif)` (ignorando diferencias por mayúsculas), entonces usará esa foto como la imagen de tapa para la primera página.

### Las plantillas

Cada página de mes está pre fabricada. Se pueden utilizar las plantillas disponibles aquí, o generarlas uno mismo. Estas plantillas fueron creadas usando [Gimp](http://www.gimp.org/) y el script `sg-calendar`, que puede ser encontrado [aquí](http://chiselapp.com/user/saulgoode/repository/script-fu/wiki?name=sg-calendar) y [aquí](http://gimpscripts.com/2012/01/calendar/).

### Requerimientos para las imágenes

Para que las imágenes se ajusten correctamente al espacio disponible, las mismas debe cumplir los siguientes requerimientos de proporción:

| Imagen               | Proporción                |
| -------------------- | -------------------------:|
| Tapa                 | Hoja A4 alto/ancho = 1.41 |
| única para el mes    | alto/ancho = 1.21         |
| Menores              | alto/ancho = 1.21         |
| Principal horizontal | ancho/alto = 1.65         |
| Principal vertical   | alto/ancho = 2.42         |

NOTAS:
* La cantidad de pixeles no es importante, aunque cuanto mayor definición, mejor. Lo importante es la relación entre el ancho y el alto de las fotos, como se describe en la tabla anterior.
* Dadas las plantillas ofrecidas aquí, las imágenes deberían tener una calidad de 72 píxeles por pulgada (ppi).

## Ejemplo

EL directorio `ejemplo` contiene un proyecto de ejemplo completo, junto con el archivo `calendario_ejemplo.pdf`, que es el resultado de ejecutar `mkcal ejemplo`.

## Plantillas

El directorio `plantillas` contiene plantillas para el año 2016 pre-armadas en formato PNG, y también los archivos originales en formato XCF (Gimp) para su fácil edición. A partir de estos archivos es muy simple generar las plantillas para otros años.

## Dependencias

### Para ejecutar mkcal

1. [GNU Bash](https://www.gnu.org/software/bash/), el shell por omosión en la mayoría de las distribuciones GNU/Linux y en Mac OS X.
2. [ImageMagick](http://imagemagick.org), que se puede instalar usando el gestor de paquetes de la distribución GNU/Linux que estés usando; o con [Homebrew](http://brew.sh/) en Max OS X.

### Para editar las plantillas

1. [Gimp](http://www.gimp.org/)
2. Script [sg-calendar](http://gimpscripts.com/2012/01/calendar/) para Gimp.

