# [iom!] Experimentos rítmicos en Pure Data: #1 Control base
(1a.ed, 2019)

¡Bienvenido! a la primera publicación de _[iom!] Experimentos rítmicos en Pure Data_.

Pure Data o Pd es un ambiente de programación para multimedia de plataforma abierta creado en 1996 por Miller Puckette, actualmente desarrollado por él y otros programadores alrededor del mundo. Permite a músicos, artistas, programadores e investigadores crear software gráficamente sin necesidad de escribir líneas de código. 

Pd es capaz de generar y procesar sonido, video, gráficos, eventos MIDI, comunicación serial, entre otros. Por estas características se convierte en una herramienta práctica para aprender a procesar datos básicos, así como realizar sistemas complejos para proyectos de gran escala.

Este fanzine contiene cuatro recetas que a usted lector le ayudarán a introducirse en el manejo del programa. 

Tres de las recetas están dedicadas al control de mensajes y una para el control de audio. En cada una de ellas se especifican los materiales y el software utilizado, además se señala la versión en la que fue desarrollada. Cabe destacar que para este número, todas las recetas son compatibles con la versión principal (vanilla) y la versión extendida de Pure Data, además encontrará ilustraciones que despliegan gráficamente los conceptos y pequeñas tareas para expandir la receta.
 
Ubique las diferentes secciones en este fanzine para agilizar la búsqueda de la información necesaria. Las recetas están distribuidas de tal manera que incrementan en complejidad conforme avance en su lectura y se le sugiere que se respete ese orden.
 
Aunque he escrito este material para ser de fácil comprensión, usted puede agregar cualquier comentario o referencia visual a la receta para aclarar su funcionamiento. 

¡Espero disfrute del material tanto como yo al crearlo! 

---

## Contenido

1. [Para iniciar](#para-iniciar)
2. [Reloj](#reloj)
3. [Contador](#contador)
4. [Selección de índices](#selección-de-índices)
5. [Percusión](#percusión)
6. [Para finalizar](#para-finalizar)

---

[^^^](#contenido)

## Para iniciar

Aquí están recogidos términos y expresiones para brindar mayor claridad a los textos de esta publicación. Se escribirán en inglés los nombres de aquellos que no cuenten con una traducción al español. Algunas definiciones coinciden con objetos en Pure Data, que se indicarán después del nombre y estarán entre corchetes.

* **Argumento**. Normalmente números, aunque también pueden ser variables del tipo símbolo, que preparan al objeto para operar. Los argumentos escritos después del nombre del objeto son llamados argumentos de creación. Si se envían mensajes o señales de audio por la entrada indicada, éstos sustituyen el argumento de creación.


* **Bang** [bang] || [bng]. Envía un impulso corto para activar operaciones o enviar mensajes. La interfaz gráfica de usuario para el mensaje “bang” es [bng].


* **DSP**. Procesador digital de señal que permite transformar señales análogas a digitales y viceversa. Al trabajar con audio, asegúrese de que el procesador se encuentre activo en la ventana Pd. 


* **Número entero** [int] || [i]. Tipo de variable que aloja un número entero positivo o negativo sin punto decimal. 


* **Entrada** (inlet) [inlet]. Las entradas de los objetos se encuentran indicadas por líneas negras gruesas en su parte superior. La entrada extrema izquierda del objeto es llamada “caliente”, puesto que cualquier mensaje recibido por aquí creará una salida. En objetos que alojan variables, un valor compatible enviado por la entrada caliente actualizará el argumento de creación.  
Normalmente el resto de las entradas serán “frías”, por lo que no efectuarán la operación determinada por el objeto, sin embargo, pueden actualizar el argumento de creación. Todos los objetos tienen entradas excepto [pd], que crea “subpatches”. Se usa el objeto [inlet] para agregar entradas a [pd]. 


* **Número con punto decimal** [float] || [f]. Tipo de variable que aloja un número positivo o negativo con punto decimal.


* **Función**. Estructura que realiza operaciones complejas en un objeto. Estas operaciones pueden ser aplicadas indistintamente entre objetos.


* **Lista** [list] || [l]. Representación de una secuencia de datos en la que cada elemento contenido ocupa un lugar único incluso si algún dato se encuentra repetido.


* **Mensaje**. Cualquier dato que puede ser interpretado por un objeto.


* **Método**. Estructura que realiza operaciones complejas en un objeto. Éstas operaciones no pueden ser aplicadas indistintamente entre objetos.


* **Modo de edición**. Para agregar mensajes, objetos, interfaces gráficas y realizar conexiones entre ellos necesita estar en modo activo. Presione ctrl+e (Windows / Linux) o cmd+e (OS X) y mueva su cursor. Si el puntero se asemeja a una mano, el modo de edición está activo. Si el puntero tiene forma de flecha, el modo de edición está inactivo y puede manipular interfaces gráficas de usuario o activar mensajes a través de clics.


* **Objeto**. Interfaz que contiene código estructurado en métodos y funciones. Dicho código no necesariamente debe ser mostrado para operar el objeto.


* **Salida** (outlet) [outlet]. Las salidas de los objetos se encuentran indicadas por líneas negras gruesas en su parte inferior. A diferencia de las entradas, no hay salidas calientes ni frías. Todos los objetos tienen salidas excepto [pd]. Se usa el objeto [outlet] para agregar salidas a [pd].


* **Signo de dólar** $. Permite configurar mensajes y objetos con argumentos variables. Requiere de un número entero inmediatamente después para indicar el sitio donde se alojará cada variable, por ejemplo $1, $2, $3, etc. En el objeto [expr] es necesario indicar el tipo de variable de entrada antes del número entero, por ejemplo [expr $f1+$f2].


* **Símbolo** [symbol]. Tipo de variable que funciona como identificador de las propiedades de un objeto.

[^^^](#contenido)

### Materiales

Conocer de antemano los elementos que posee y/o requiere su espacio le permitirá seguir las recetas sin grandes complicaciones y disfrutar de sus resultados y experimentos.

_Fuera de Pure Data_

* **Conexión a Internet**. A través de la conexión a Internet podrá descargar las diferentes versiones del programa, publicar sus resultados en línea e investigar a su gusto sobre los temas aquí tratados.


* **Software Pure Data**. Puede descargarlo en [puredata.info](http://puredata.info/) 


* **Computadora personal**. Los sistemas operativos Windows, OS X y Linux son útiles para la realización de las recetas. Considere el sistema operativo que utiliza su ordenador para descargar la versión adecuada de Pure Data.


* **Audífonos y/o bocina**. Serán necesarios para escuchar los resultados de su trabajo.


* **Interfaz de sonido**. Aunque no es de extrema importancia para el uso de este recetario, una tarjeta de sonido puede brindarle una mayor resolución en la reproducción y síntesis de audio.


* **Escritorio**. Importante para mantener una buena postura y evitar fatiga y/o lesiones.


* **Buena iluminación**. No trabaje en este recetario si no cuenta con buena iluminación, a la larga esto puede dañar sus ojos. De preferencia trabaje con luz natural en un espacio interior. 


* **Lápiz y goma**. Eventualmente querrá hacer anotaciones sobre estas hojas, por ello se recomiendan herramientas con las que pueda corregir fácilmente.

_Dentro de Pure Data_

Estos materiales los puede obtener a través del menú “Poner” o “Put”.

* **Objetos**. Estructuras diseñadas con funciones específicas para crear, filtrar y transformar eventos de control o señales de audio. Normalmente requieren de argumentos para su operación, por ejemplo: [metro 500], donde la cifra 500 es el argumento del objeto. Para procesar señales de audio, los objetos se diferencian de los objetos de control usando el símbolo de tilde ~, por ejemplo [osc~ 440].


* **Interfaces gráficas de usuario**. Con estas interfaces gráficas de usuario puede interactuar con el programa a través de clics. También facilitan la visualización de datos y eventos. 


* **Matrices**. Colecciones de valores representados en dos dimensiones (eje x, eje y). Son útiles para visualizar archivos de audio, listas de datos, envolventes, funciones matemáticas y para editar formas de onda.


* **Mensajes**. Los objetos en Pure Data transportan información de diferentes tipos, como números enteros, números con punto decimal, símbolos y listas, entre otros. A través de cajas de mensaje puede enviar información específica para interactuar con los objetos.

---

[^^^](#contenido)

## Reloj
_Pure Data 0.49.1_

Materiales:

* 1 [/]
* 1 [expr]
* 2 [==]
* 2 [metro]
* 2 [spigot]
* 1 [bng]
* 1 [toggle]
* 1 [hradio]
* 1 Caja de número

En Pure Data, el objeto [metro] le permite especificar la frecuencia en milisegundos con la que se ejecuta un evento. Sin embargo, en ciertas aplicaciones musicales es más conveniente especificar esta distancia en beats por minuto (bpm) en lugar de milisegundos.

Para ello analice lo siguiente: Si cada segundo equivale a 1000 milisegundos y en un minuto existen 60 segundos, entonces un minuto equivale también a 60,000 milisegundos. La distancia en fracciones de segundo que existe entre un beat y otro puede calcularse al dividir 60,000 milisegundos entre la velocidad requerida en beats.

Active el modo de edición y cree el objeto [expr]. Con él podrá evaluar expresiones formadas con datos de control. Ingrese como argumento la expresión descrita para transformar los bpm a milisegundos de la siguiente manera: [expr 60000/$f1]

$f1 será la variable en la cual asignará los bpm que desea transformar.

Conecte la caja de número a la entrada de [expr]. Por la salida del objeto [expr] obtendrá el resultado de la conversión. Conecte esta salida a la entrada derecha de un objeto [metro] y agregue a éste un [toggle] por la entrada izquierda. Conecte de la salida de [metro] un [bng].

Ahora inactive el modo de edición y coloque su puntero sobre la caja de número, dé clic y escriba la cifra _120_. Presione la tecla Enter en su teclado. Posiciónese sobre [toggle] y dé clic. Verá parpadear el objeto [bng]. 

Esta es la forma más sencilla de construir un reloj en el que exprese el tiempo en bpm. Sin embargo, probablemente quiera contar con la posibilidad de alternar entre dos o más subdivisiones del tiempo. Al añadir el resto de los materiales logrará cambiar entre cuartos y octavos rápidamente.

Cree el objeto [/] con el argumento _2_ y conéctelo entre la salida de [expr] y la entrada derecha del nuevo objeto [metro]. Ahora la expresión _60000/$f1_ pasará hacia dos lugares distintos y asignará a cada objeto [metro] un valor que conserva una distancia simétrica respecto del otro.

Como la idea de este reloj es que usted alterne entre las diferentes frecuencias con la que se enciende un solo [bng], no deberá conectar ambos objetos [metro] al mismo punto, sino que tendrá que evaluar cuándo cierta frecuencia afecta el final de la cadena. Para ello agregue el objeto [hradio]. Dé clic derecho sobre de él, vaya a propiedades y ajuste el número de celdas a 2. [hradio] producirá los valores _0_ y _1_ dependiendo de la casilla sobre la cual haga clic. A sabiendas de esto, puede evaluar [hradio] para abrir o cerrar el paso de mensajes de los objetos [metro]. Coloque dos operadores binarios [==], uno con argumento _0_ y otro con argumento _1_. Conecte la salida de [hradio] a las entradas izquierdas de los operadores. Agregue ahora dos objetos [spigot] con argumento _0_ y conecte la salida de un operador a la entrada izquierda de un objeto [spigot]. Haga lo mismo con el operador y [spigot] restantes.

Los operadores [==] definen si el mensaje que pasa por ellos es igual a su argumento. Cuando esto es cierto, producen el mensaje _1_, en caso contrario _0_. 

[spigot] dejará pasar los mensajes que llegan por su entrada izquierda, sólo si su argumento cambia a _1_, lo cual sucede cuando [hradio] produce un mensaje igual al argumento de algún operador [==].

Para finalizar conecte la salida de un objeto [metro] a la entrada izquierda de un objeto [spigot]. Haga lo mismo con el otro par y conecte ambas salidas de los objetos [spigot] a [bng]. Inactive el modo de edición y encienda el reloj a través de [toggle], dé clic en las diferentes casillas de [hradio] y verá en [bng] el cambio de la frecuencia con la que parpadea.

Utilice la caja de número para monitorear cada mensaje en el sistema. Extienda este reloj agregando más subdivisiones, por ejemplo semicorcheas o fusas.

---

[^^^](#contenido)

## Contador
_Pure Data 0.49.1_

Materiales:

* 1 [+]
* 1 [- ]
* 1 [select]
* 2 [float]
* 2 [bng]
* 2 cajas de mensaje
* 3 cajas de número

A cada segundo en un minuto le corresponde un único número en ese lapso de tiempo. Al terminarse el minuto, la cuenta vuelve a empezar en 1, o bien en 60 si se va al revés. Para tocar cualquier ritmo es necesario saber dónde empezó la cuenta y dónde termina. La siguiente receta corresponde a la creación de un contador, que le permitirá ajustar eventos sonoros a cada número en una cuenta.

¿Cuál es entonces la lógica para desarrollar un contador que vaya de uno en uno y al llegar al límite regrese al valor mínimo inicial? Lo dividiremos por partes:

Primero inicie en un valor, por ejemplo: 0. A este valor será necesario sumarle de uno en uno. Para ello conecte el primer [bng] a la entrada izquierda de un objeto [float] con el argumento _0_, recuerde que el modo de edición debe estar activo. Después conecte un operador aritmético binario [+] con el argumento _1_. Envíe la salida del operador [+ 1] a la entrada izquierda de [float 0] para actualizar su argumento y prepararlo para el siguiente clic. Termine con una caja de número después del operador [+ 1].

Al dar clic sobre [bng] con el modo de edición inactivo, la caja de número mostrará un resultado al que se le agregará 1 por cada clic. No importa cuántos clics se realicen, el número seguirá incrementando sin ningún límite. Deberá hacer algunos cambios para lograr que el contador vuelva al valor mínimo después de alcanzar un valor máximo. Active el modo de edición.

Agregue el objeto [select], el cual le permitirá evaluar una variable en relación a su argumento. Este objeto será el filtro con el que se decidirá si la operación debe continuar escalando de 1 en 1 o bien, si deberá regresar al valor inicial.

[select] cuenta con dos salidas: la izquierda producirá un bang cada vez que el valor enviado por su entrada izquierda coincida con su argumento; mientras que la derecha pasará el valor evaluado sin ningún cambio cada vez que esta condición no se cumpla. Elija un número límite para el contador y escríbalo en el objeto [select] como argumento, por ejemplo 7. Conecte la salida de [+ 1] a la entrada izquierda de [select 7].

Cuando el resultado del operador coincida con el argumento del filtro seleccionador, el contador se reiniciará. Cuando no lo sea, continuará con la suma.

El reinicio del contador es sencillo de aplicar. Primero agregue la caja de mensaje y escriba dentro _0_. Conecte según la lógica planteada las salidas de [select 7] a la caja de mensaje y al objeto [float 0].

Este algoritmo ya funciona muy bien como un contador que vuelve al valor mínimo después de alcanzar un valor máximo. Note que nunca obtiene 0 como resultado, sino 1, debido a que, aunque reinicie el contador a 0, siempre se le sumará 1.

Ahora bien, ¿qué pasa si la próxima vez que usted quiera contar números de uno en uno, desee que la cuenta cubra otro rango? ¿O si después de alguna condición adicional considere necesario volver al valor mínimo? Las párrafos siguientes responderán a estas preguntas y completarán la receta.

Para ajustar el límite máximo dentro de este algoritmo, deberá conectar una caja de número a la entrada izquierda del objeto [select 7], esto actualizará su argumento cada vez que ingrese un valor en la caja de número. Para ajustar el límite mínimo, conecte una nueva caja de número al operador [-] por la entrada izquierda. Agregue el argumento _1_ al operador. 

Redefinir el límite mínimo requiere algunos pasos más que cuando se asigna de nuevo el valor máximo, ya que los mensajes en Pure Data sólo pueden ser modificados a través de otros mensajes. Por lo que deberá conectar debajo del operador [- 1] una nueva caja de mensaje. 

Escriba dentro _set $1_ y conéctela a la caja de mensaje que actualiza [float 0]. Inactive el modo de edición, ajuste los límites y dé clic sobre [bng]. Note que los límites ajustables a través de las cajas de número están incluidos en el intervalo de valores que produce el contador.
Gracias al operador [- 1], es innecesario preocuparse por no ver reflejado en la cuenta el valor mínimo que ingresa por la caja de número.

El detalle final, regresar al valor mínimo tras cumplirse algún otro condicional, es muy fácil de agregar. Lo único que debe hacer es capturar el resultado del operador [- 1] en un nuevo objeto [float] a través de su entrada derecha, conectar un [bng] por la entrada izquierda y enviar la salida del nuevo [float] a la entrada izquierda de [float 0].

Con el modo de edición desactivado, ajuste de nuevo los límites, comience a contar y a mitad del ciclo dé clic en el [bng] que reinicia el contador. Al continuar con la cuenta verá que el algoritmo ha regresado al valor mínimo.

---

[^^^](#contenido)

<span id="selección-de-índices">

## Selección de índices </span>
_Pure Data 0.49.1_

Materiales:

* 1 [<]
* 1 [route]
* 1 [list prepend]
* 2 [trigger]
* 2 [list split]
* 1 caja de mensaje
* 2 cajas de número

Para obtener un valor localizado en un índice específico de una lista se requiere dividir esa lista a partir del índice a elegir. Considere que los índices en una lista comienzan en 0, de tal forma que en la lista _1 2 3 4 5 6_, el índice 0 corresponde a _1_, el índice 1 corresponde a _2_ y el índice 3 corresponde a _4_.

Al dividir la lista obtendrá otras dos listas, una derecha, cuyos elementos siempre formarán parte de un intervalo cerrado y su inicio será el índice que pretende ubicar; mientras que la lista izquierda, tendrá un intervalo abierto donde el índice que busca se escapará.

Después deberá dividir la lista derecha a partir del índice 1. La lista izquierda tendrá el valor que decidió extraer desde un principio.

Como ejemplo, se obtiene aquí el índice 2 de la lista _1 2 3 4 5 6_. Primero la lista es dividida a partir del índice 2: 

Se utiliza la parte de la derecha y es dividida a partir del índice 1: 

Se toma la parte izquierda. El índice 2 de la lista _1 2 3 4 5 6_ es efectivamente 3.

En Pure Data se ve de la siguiente forma:

Note los argumentos de los objetos [list], ambos están inicializados con _split_, una función que le permite dividir a las listas en dos. Sólo uno de esos objetos tiene también como argumento el valor _1_. Esto indica el índice por el cual la lista será nuevamente dividida. Hay tres salidas del objeto [list split], la izquierda corresponde a la parte izquierda de la lista dividida, mientras que la de en medio a la parte derecha. Por la tercera salida pasará la lista entera si hay menos índices de los que el argumento indica.

El primer objeto [list split] requiere que indique el índice por el cual el objeto dividirá la lista, por ello debe conectar [trigger] con los argumentos _b_ y _f_ a [list split] por su entrada derecha y a la caja de mensaje que contiene la lista a evaluar.

Los argumentos _b_ y _f_ se traducen a _bang_ y _float_. Cuando ingrese un valor por la caja de número superior, el objeto [trigger] pasará su valor primero como float y milisegundos después como bang, de esta forma permitirá ordenar los eventos y evitar un comportamiento errado.

Los valores 0, 1, 2, 3, 4 y 5 recuperan un dato específico en la lista _1 2 3 4 5 6_. Al ingresar el valor 6 a la caja de número superior, no se obtiene ningún dato de la lista porque el índice 6 no existe. En caso de ingresar un número negativo, el sistema lo considerará como 0 y regresará el dato _1_. 

Para que cualquier número negativo no sea considerado, de la misma forma que los números positivos al rebasar los índices disponibles, deberá añadir una etiqueta a la lista que indique que el valor ingresado no es negativo y sólo dejar pasar la lista si esa condición se cumple.

Para ello agregue el resto de los ingredientes de la siguiente manera:

El operador binario [< 0] generará 1 sólo si el valor ingresado por su entrada izquierda es menor a 0, es decir, negativo. 

El objeto [list prepend] se encargará de agregar el resultado de la evaluación de [< 0] al principio de lista. Quedará _0 1 2 3 4 5 6_ cuando el valor ingresado por la caja de número sea mayor o igual a 0, o bien, _1 1 2 3 4 5 6_ cuando el valor sea menor a 0. Note que ahora cuenta con siete índices. Esto no resulta problemático gracias a [route 0], que dejará pasar la lista por su salida izquierda sólo cuando ésta empiece en 0, eliminando a la vez el primer dato. 

Esta receta opera muy bien en conjunto con el contador para generar secuencias percusivas si los valores en la lista son binarios, por ejemplo ceros y unos, o secuencias melódicas si los valores en la lista son frecuencias. Muy pronto será introducido a voces afinadas, por el momento intente conectar ambas recetas juntas de manera que el contador avance tras cada clic un índice.

---

[^^^](#contenido)

<span id="percusión">

## Percusión </span>

_Pure Data 0.49.1_

Materiales:

* 1 [mtof]
* 1 [delay]
* 1 [trigger]
* 1 [*~]
* 1 [osc~]
* 1 [line~]
* 1 [dac~]
* 1 [bng]
* 3 cajas de mensaje

Las tres recetas anteriores se basan en estructuras de control que le permitirán ejecutar rítmica y melódicamente la sección de audio en Pure Data. Para adentrarse en este nuevo terreno, identifique en los materiales los objetos con el signo “~”. 

Estos son objetos de audio, que una vez inicializados en el programa se encontrarán activos, a diferencia de los objetos de control, los cuáles sólo envían mensajes cuando se cumple cierta condición
o se interactúa con ellos. 

Las conexiones entre objetos de audio también son diferentes y verá que los cables que los unen son más gruesos. Los objetos de control no siempre son compatibles con los de audio, sin embargo, esto no complica el flujo de trabajo. Las señales de audio en Pure Data serán siempre calibradas en valores de _-1_ a _1_. Al amplificar una señal de audio, _0_ representará el silencio porque la distancia entre las crestas o valles de una onda y el punto de equilibrio es nula.

Sea precavido al experimentar con los volúmenes de esta receta y de preferencia no use audífonos inmediatamente.

Comience por encender el DSP en Pure Data. Después cree el oscilador [osc~] y asígnele una frecuencia en nota MIDI: Use una caja de mensaje con el valor _69_ y conéctela al objeto [mtof], el cual transformará la nota 69 a Hertz. 

Conecte [mtof] a la entrada izquierda de [osc~]. Inactive el modo de edición y dé clic sobre el mensaje. Si además agrega una caja de número después de [mtof], al dar clic sobre el mensaje con el modo de edición inactivo, verá que la nota MIDI 69 representa 440 Hz., es decir A4. 

Trabajar con notas MIDI es más fácil porque las notas están representadas de forma lineal, mientras que en Hertz las notas están distribuidas de forma exponencial. Un semitono arriba de A4, será en MIDI la nota 70, es decir, A#4 o Bb4.

Después de [osc~] añada el objeto [*~] con argumento _0.5_. Disminuya el volumen de salida de su ordenador o interfaz de audio y dirija la salida de [*~ 0.5] a cada entrada del objeto [dac~]. Incremente el volumen de salida de su ordenador o interfaz de audio hasta un nivel confortable.

El objeto [*~ 0.5] funciona como un amplificador. El argumento _0.5_ indica que la salida del oscilador está ajustada a un 50% del nivel máximo. El objeto [dac~] es el convertidor de audio digital a análogo que le permite escuchar lo que sucede en Pure Data a través de sus bocinas.

El sonido creado por el oscilador será continuo a menos que desconecte el amplificador de [dac~] u [osc~] del amplificador. Para moldear el sonido de percusión necesitará crear una envolvente que controle el amplificador. 

Active el modo de edición, borre los argumentos de [*~] y conecte por la entrada derecha el objeto [line~]. Este objeto creará una rampa entre dos valores en un determinado tiempo. Para iniciar una rampa debe ingresar un valor meta y el tiempo en que desea que el objeto llegue a ese valor. Conecte por la entrada izquierda de [line~] una caja de mensaje con los valores _0.5_ y _1_. Donde 0.5 será el punto meta y 1 el tiempo en milisegundos. Al dar clic en esta caja de mensaje, el amplificador incrementará su volumen de 0 a 0.5 en 1 milisegundo. Lo cual puede corresponder al golpe de una baqueta sobre un tambor. Sin embargo, el tambor no permanece activado infinitamente, sino que decae hasta el silencio. Para lograr este segundo momento, añada la última caja de mensaje con los valores _0_ y _80_. Esto quiere decir que al dar clic en el nuevo mensaje, [line~] volverá a 0 en 80 milisegundos y el sonido se detendrá. 

Como estos dos eventos acontecen inmediatamente uno después el otro, deberá simplificarlo todo para que sucedan ambos eventos con sólo un clic. Conecte [bng] al objeto [trigger] con los argumentos _b_ y _b_. La salida derecha de [trigger] conéctela al mensaje de apertura de la envolvente, mientras que la salida izquierda deberá conectarla a un objeto [delay] con argumento _2_ y éste al mensaje de cierre de la envolvente.

Al dar clic sobre [bng], el objeto [trigger] activará primero el mensaje que abre la envolvente e inmediatamente después activará el retraso de [delay] que dura tan sólo 1 milisegundo más que la primera rampa que hace [line~], para cerrar la envolvente y regresar el amplificador a 0.

Experimente con frecuencias más bajas y diferentes tiempos de duración de la envolvente.

---

[^^^](#contenido)

## Para finalizar

Conecte las cuatro recetas entre sí, de manera tal que el reloj opere sobre el avance del contador y éste seleccione cada índice en una lista. Los índices en la lista los puede especificar manualmente al asignar notas MIDI. Por ejemplo: _60_, _42_, _65_ y _68_, que musicalmente se traducen a C4, F#2, F4 y G#4. Cada índice deberá ser asignado primero a la frecuencia del sintetizador de percusión y después deberá activar la envolvente. Use el objeto [t b f] para este procedimiento.

Para reiniciar el contador cuando detenga el reloj, necesitará un objeto [select 0]. Conéctelo entre
el [toggle] del reloj y el [bng] que prepara al contador para regresar al valor mínimo. 

Al juntar todas las recetas, encontrará que puede hacer mejoras para que el sistema sea más práctico.
Le recomiendo investigar sobre los objetos [loadbang], [random], [inlet], [outlet], [pd] e implementarlos.

Espero que esta publicación le haya sido útil y que también haya pasado un buen momento al leerla, creando apuntes y modificaciones a su gusto. 

Disfrute su día.

¡Hasta la próxima!

---

[^^^](#contenido) 

**[iom!] Experimentos rítmicos en Pure Data: #1 Control base**

**Edición:** Isaac Medina y Paola Sandoval

**Textos:** Isaac Medina

**Diseño editorial:** Paola Sandoval

---

This work is licensed under the Creative Commons Attribution 4.0 International License. To view a copy of this license, visit http://creativecommons.org/licenses/by/4.0/ or send a letter to Creative Commons, PO Box 1866, Mountain View, CA 94042, USA.
