La sesión de noviembre de 2015 de [#periodismodatos](http://medialab-prado.es/article/periodismo_de_datos_-_grupo_de_trabajo) acoge la primera actividad de [@JournocodersMAD](https://twitter.com/journocodersmad) y del recién creado [Madrid D3.js](http://www.meetup.com/es/Madrid-D3-js/). El formato del encuentro de *Journocoders* es el de un encuentro de periodistas interesadxs en aprender programación mientras comparten trucos y consejos prácticos de código y siguen y actualizan pequeños tutoriales. Para esta ocasión nos acompañarán lxs miembrxs de *Madrid D3.js*

Para este primer encuentro, [Adrián Blanco Ramos](https://github.com/adrianblanco/), creador y dinamizador de *JournocodersMAD* ha realizado un [tutorial](https://github.com/adrianblanco/tutoriald3journocoders), en el que se basa este texto, que sigue el tutorial [Let's make a bar chart](http://bost.ocks.org/mike/bar/) (hagamos un gráfico de barras) del creador de [D3.js](http://www.d3js.org), [Mike Bostock](http://bost.ocks.org/mike/), con el que se consigue un gráfico de barras muy sencillo conjugando *D3*, *HTML* y *CSS*.

Hemos contado, además, con la ayuda experta de lxs promotorxs del recién creado [meetup MADRID D3.js](http://www.meetup.com/es/Madrid-D3-js/), Fernando Blat ([@ferblape](https://www.twitter.com/ferblape)) y Beatriz Martínez ([@maritrinez](https://www.twitter.com/maritrinez)).

Adrián creo un [hackpad](https://journocodersmadrid.hackpad.com/JournocodersMad-noviembre-2015-Q9TmZSpvzlN) para que compartir notas sobre el evento

# Consideraciones previas

-   ¿Tienes un buen editor de texto? Es el programa que se utiliza para editar el *HTML*, *CSS* y *JS* que producirá el gráfico de barras. Si no lo tienes, mira este [artículo](http://s.coop/1wxqj).
-   ¿Tienes un servidor web? Es el programa que permitirá que no tengas que subir tu trabajo a un servidor web de la Web, que puedas comprobar que funciona en tu propio ordenador.

## Servidor Web

Hay varias opciones:

-   Si utilizas *Windows* o *Mac OS*, instalarte un programa como [MAMP](https://mamp.info) que instala en el ordenador un servidor web *Apache*, un servidor de bases de datos *MySQL* y el soporte para utilizar *PHP*.
-   Si utilizas *Mac OS*, puedes seguir esta [guía](https://discussions.apple.com/docs/DOC-3083) para configurar *Apache* de forma manual.
-   Si utilizas *GNU/Linux*, probablemente ya uses *Apache*
-   *NodeJS*, con el paquete *node-static*

## Servidor web con *NodeJS*

### Descarga

<https://nodejs.org/en/>

### Instalación de *NodeJS*

Según la plataforma

### Instalación de *node-static*

Abrimos la terminal y tecleamos:

    sudo npm install -g node-static

La opción `-g` es para hacerlo de forma global.

### Configuración

Para lanzar el servidor web debemos ir con la terminal en el mismo directorio en el que está el documento de trabajo:

    cd /ruta-absoluta-o-relativa-a-directorio/

Para ver en qué directorio estamos, podemos utilizar el comando `pwd`:

    pwd

Entonces lanzamos el servidor web con `static`:

    static

Si en el navegador ponemos `http://localhost:8080` veremos el `index.html` que tenga el directorio. Es decir, se abre un servidor web para este directorio accesible a través del puerto **8080**

Atención: no cierres esta ventana de la terminal o cerrarás el servidor web.

# Cómo generar un gráfico de barras en D3

## Crear esqueleto *HTML*

Primero vamos a crear un documento *HTML* base que posteriormente modificaremos.

    <!DOCTYPE html>
    <html>
    
      <meta charset="utf-8">
      <script src=""></script>
    
      <head>
    
        <style type="text/css">
        </style>
    
      </head>
    
      <body>
    
        <div>Hola</div>
    
        <script>
        </script>
    
      </body>
    
    </html>

Ahora podemos comprobar que funciona el servidor web que hemos lanzado, para lo cual hemos de escribir en el navegador favorito `http://localhost:8080`, que nos mostrará un texto que diga `Hola`

## Ruta *JS*

Para actuar con la librería *D3js* debemos enlazarla desde el documento *HTML*. Se puede hacer de dos maneras:

1.  Descargar la librería y enlazar la librería con una ruta local. Esto nos permite conocer cómo actúa la librería y además no necesitamos conexión a Internet para ver los resultados. Para ello nos descargamos [el zip](https://github.com/mbostock/d3/releases/download/v3.5.8/d3.zip) desde la página de [D3js](https://d3js.org), lo descomprimimos en el directorio de trabajo o en otro que sepamos y lo enlazamos.

    <script src="ruta/d3.min.js"></script>

1.  Enlazar la librería desde Internet. En este caso no tenemos que descargarla y nos facilita el trabajo y funcionará siempre que esté conectado a Internet:

    <script src="http://d3js.org/d3.v3.min.js"></script>

En el manual original, la ruta no lleva `http:`, lo cual hace que puede que no funcione, mejor especificar el protocolo utilizado en la *URL*.

En ambos casos este código se inserta en la cabecera del documento *HTML*, en el elemento `head`. Se introduce la ruta a la librería *D3.js* en el atributo `src` del elemento `script`.

## Estilos

Para darle una forma a las barras, este ejemplo utiliza *CSS*, por lo que debemos enlazar el siguiente código:

    .chart div {
      font: 10px sans-serif;
      background-color: steelblue;
      text-align: right;
      padding: 3px;
      margin: 1px;
      color: white;
      }

Al igual que con el código *JS*, se puede operar de dos maneras:

1.  Incrustar el código en el *HTML* a través del elemento `style` dentro del elemento `head`:

    <style type="text/css">
          .chart div {
         font: 10px sans-serif;
         background-color: steelblue;
         text-align: right;
         padding: 10px;
         margin: 1px;
         color: white;
         }
       </style>

1.  Crear un archivo *CSS* y enlazarlo desde el elemento `link` dentro del elemento `head`:

    <link rel="stylesheet" href="ruta/archivo.css">

Se puede personalizar el estilo:

-   Cambia el tamaño o el tipo de la fuente a través de la propiedad `font`.
-   Modifica el color del gráfico con la propiedad `background-color`, bien con su nombre, como por ejemplo `steelblue` o con el valor hexadecimal, como por ejemplo `#BAD4F6`. Se puede consultar la lista completa en [W3Schools](http://www.w3schools.com/html/html_colornames.asp).
-   Cambia el ancho de las barras con las propiedades `padding` o `margin`.

Por ejemplo:

    .chart div {
      font: 14px sans-serif;
      background-color: #8B008B
      text-align: right;
      padding: 14px;
      margin: 2px;
      color: white;
      }

## Script con datos

Los datos los introducimos a través de *JS*, con el elemento `script` dentro de `body`:

     <script>
    
        var data = [4, 8, 15, 16, 23, 42];
    
    </script>

Además de los datos, se declara el dominio con `.domain()` y el rango con `.range()`

    <script>
    
       var x = d3.scale.linear()
         .domain([0, d3.max(data)])
         .range([0, 420]);
    
       d3.select(".chart")
         .selectAll("div")
         .data(data)
         .enter().append("div")
         .style("width", function(d) { return x(d) + "px"; })
         .text(function(d) { return d; });
    
     </script>

## El código final

    <!DOCTYPE html>
    <html>
    
      <meta charset="utf-8">
      <script src="d3.min.js"></script>
    
      <head>
    
        <style type="text/css">
           .chart div {
          font: 10px sans-serif;
          background-color: steelblue;
          text-align: right;
          padding: 10px;
          margin: 1px;
          color: white;
          }
        </style>
    
      </head>
    
      <body>
    
        <div class="chart"></div>
    
        <script>
          var data = [4, 8, 15, 16, 23, 42];
          var x = d3.scale.linear()
    	.domain([0, d3.max(data)])
    	.range([0, 420]);
    
          d3.select(".chart")
    	.selectAll("div")
    	.data(data)
    	.enter().append("div")
    	.style("width", function(d) { return x(d) + "px"; })
          .text(function(d) { return d; });
    
        </script>
    
      </body>
    
    </html>

Puedes ver el resultado en este link: <http://bl.ocks.org/adrianblanco/da9fed51f09ee1ec0724>

# Próximos pasos

-   Ésta es sólo la primera parte para generar un gráfico de barras en D3.
-   Puedes continuar con [Let's Make a Bar Chart II](http://bost.ocks.org/mike/bar/2/), donde se introducen los *SVG*, un concepto clave en D3.js, y [Let's Make a Bar Chart III](http://bost.ocks.org/mike/bar/3/).
