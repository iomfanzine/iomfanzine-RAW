# [iom!] Experimentos rítmicos en Pure Data: #2 Densidad
(1a.ed, 2019)

**¡HOLA DE NUEVO!**

Espero que esté tan emocionado/a por programar ritmos en Pure Data como yo lo estoy por compartir las recetas que he preparado para usted. En esta ocasión, generará secuencias rítmicas utilizando aleatoriedad, saturación, desorden y silencio. Puede trabajar de nueva cuenta con la versión principal de Pure Data (vanilla) o con la versión extendida sin ningún inconveniente. 

Las primeras dos recetas se encuentran íntimamente vinculadas y son parte de un mismo concepto. La tercera receta puede llevarse a cabo antes o después de las primeras dos; sin embargo, la lectura sugerida de este número del fanzine es de principio a fin, para poder disfrutar de cada nuevo paso.

[iom!] #2 Densidad está relacionado con el resto de los números de la colección. Para obtener un mayor provecho de las ideas expuestas aquí, aconsejo revisar el número anterior donde se exponen las recetas de Reloj, Contador, Selección de índices y Percusión. En caso de que no sea posible tener acceso al primer número, ¡no se preocupe! [iom!] #2 también puede ser disfrutado de manera independiente.

Lea con atención y tome todas las notas que considere necesarias. Y, sobre todas las cosas, ¡diviértase!

---

## Contenido

1. [Para comenzar](#para-comenzar)
2. [Saturación](#saturación)
3. [Permutación aleatoria](#permutación-aleatoria)
4. [Bombo](#bombo)
5. [Para finalizar](#para-finalizar)

---

[^^^](#contenido)

## Para comenzar

En el mundo de las percusiones se suele afirmar que no es la nota lo que se toca, sino el silencio. Cada golpe que se da a un tambor podría ser cualquier nota, puesto que su valor  lo define el silencio que la rodea. Imagine un piano. Cuando alguien presiona una de sus teclas, el sonido comienza y no se detiene hasta soltarla o hasta que la cuerda pierda energía y deje de vibrar. En un tambor no hay manera de mantener la resonancia por un período de tiempo indefinido; sin embargo, puede medirse la distancia en pulsos que existen entre un golpe y otro.

Considere un compás de cuatro cuartos donde todas las notas son octavos. Cuente el compás como _1 (y) 2 (y) 3 (y) 4 (y)_ y aplauda conservando un mismo pulso.

En este caso, el ritmo indica que se debe aplaudir ocho veces de forma continua. Lea de la misma forma el compás siguiente:

En este ritmo, aplaudirá cuatro veces seguidas y luego permanecerá en silencio durante la cuenta de _3 (y) 4 (y)_.

Intente ahora marcar con aplausos el siguiente ritmo:

En este caso sólo aplaudirá tres veces: en el contratiempo del primer tiempo y en los tiempos fuertes _3_ y _4_. Note cómo la forma de las notas cambia. La representación gráfica del ritmo indica no sólo que se debe aplaudir tres veces, sino que se debe respetar la distancia entre cada aplauso. Los golpes y los silencios, juntos, forman el ritmo.

Estos mismos compases pueden ser interpretados a través de listas de ceros y unos, donde los unos representan el impulso de energía (o golpe) y los ceros, el silencio que existe entre cada uno.

Intente ahora escribir algunos ritmos en las siguientes casillas, considerando que se debe aplaudir la cantidad de veces que indicada. Como consejo, escriba primero en las casillas de forma aleatoria todos los unos que corresponden y rellene las demás con ceros.

Observe la forma gráfica que adquieren las listas cuando contienen una cantidad mayor de unos; esta cualidad será trasladada a Pure Data mediante la receta [Saturación](#saturación). A pesar de que varias de las listas que escribió cuentan con la misma cantidad de unos, cada una es diferente; esta segunda cualidad será presentada como [Permutación aleatoria](#permutación-aleatoria). Con estas dos primeras recetas ya será capaz de crear numerosas variaciones rítmicas; sin embargo, aún necesita de voces de percusión. Con la receta [Bombo](#bombo) desarrollará una percusión sintetizada que le servirá de base para generar sonidos cortos más complejos.

Hasta aquí la introducción, ¡prepárese para resolver estos emocionantes algoritmos en Pure Data!

---

[^^^](#contenido)

<span id="saturación">

## Saturación </span>
    
_Pure Data 0.49.1_

Materiales:

* 1 [+]
* 1 [-]
* 1 [==]
* 1 [mod]
* 1 [f]
* 10 [t]
* 5 [list]
* 1 [list store]
* 2 [until]
* 4 [print]
* 3 [spigot]
* 1 [bng]
* 1 [hradio]
* 3 cajas de mensaje
* 1 caja de número

En la introducción realizó un ejercicio en el que elaboró listas con la cantidad de unos correspondiente al número de aplausos, insertando los valores de forma aleatoria en las casillas disponibles y colocando la cantidad de ceros necesaria para completar los espacios restantes. Aunque para las personas las premisas de este ejercicio puedan resultar bastante claras, al programar se debe indicar cada paso con mucha claridad para obtener el resultado deseado sin errores. Por ello, el primer objetivo de esta receta será producir una lista con n cantidad de unos cuya longitud esté definida por el valor de _n_.

Active el modo de edición a través del menú _Edición/Modo de Edición_, tome una caja de mensaje desde el menú _Poner_, colóquela e ingrese en ella el dato _1_. Conecte la caja a un objeto [list] por su entrada izquierda. Añada el objeto [print] con el argumento _unos_ y conéctelo a la salida de [list]. 

Salga del modo de edición y haga clic sobre la caja de mensaje. Acceda a la consola de Pure Data a través del menú _Ventana_ y seleccione _Pure Data_. En esta sección verá toda la información que envíe a través de los objetos [print], así como los errores que se puedan producir al conectar de forma inadecuada algunos objetos entre sí, al definir argumentos inválidos asignados a los objetos, o bien al enviar mensajes que no refieren a ningún método sobre un objeto en específico, entre otros. 

Ahora haga clic sobre la caja de mensaje. Verá que en la consola aparece el argumento de [print] seguido de dos puntos y el dato impreso. ¿Qué sucede aquí? ¿Por qué no se imprime una lista más grande tras cada clic? Cada vez que un dato ingresa por la entrada izquierda de [list], el objeto se actualiza con el nuevo dato y elimina el anterior. La lista no puede crecer porque no consta de un camino mediante el cual se concatene a sí misma. Para ello, active el modo de edición y desconecte [print unos]. En su lugar conecte la salida de [list] a un objeto [t] con el argumento l. Conecte la salida de [t l] a la entrada derecha de [list]. Esto creará un retorno por el cual el dato pasará a [list], continuará a [t l] y regresará a [list], dejándolo preparado para el ingreso del siguiente dato. Conecte de nuevo [print unos] a la salida de [list], salga del modo de edición y haga clic sobre la caja de mensaje.

Observe la consola tras cada clic. Verá cómo ahora la lista crece con cada nuevo dato ingresado.

Para entender mejor de qué manera se agregan los datos a través de este retorno, elabore en un espacio aparte un contador que genere una lista con cada nuevo dato de la cuenta incluyendo todos los anteriores. Para ello, puede utilizar el contador revisado en _[iom!] #1 Control Base_, desarrollar uno de manera libre o utilizar el que se describe a continuación.

De la misma manera en que una lista crece a través del retorno [list]><[t l], un número cualquiera puede incrementar de forma lineal al sumarle un valor constante. Active el modo de edición y seleccione un objeto [f] y un objeto [+] con argumento _1_. Conecte la salida de [f] a la entrada de [+ 1] y la salida de éste a la entrada derecha de [f]. Agregue un [bng] por la entrada de izquierda [f], pero no salga del modo de edición ni haga clic sobre [bng]. De manera predeterminada, [f] está inicializado en _0_, si diera clic en [bng] sumaría 1 a 0 y para esta demostración necesita conservar este primer 0.

Piense en una división. Cuando divide un número entre otro, obtiene dos resultados: uno de ellos es el cociente, que corresponde a la cantidad de veces que cabe el divisor en el dividendo, y el otro es el residuo, que indica el espacio sobrante del dividendo. En Pure Data, para obtener el cociente de una división se utiliza el objeto [/], mientras que para obtener el residuo se utiliza el objeto [mod]. En este ejercicio será necesario evaluar el resultado del retorno de [f]><[+ 1] entre el número de pasos que tendrá el contador y guardar el residuo de la operación en un retorno de [list]><[t l]. Por lo tanto, conecte el objeto [mod] con argumento _8_ a la salida de [f]. Conecte un arreglo de [list]><[t l] a la salida de [mod 8] y al final agregue un objeto [print] con el argumento _demoConcatenación_ a la salida de [list]. Salga del modo de edición, haga clic ocho veces sobre [bng] y observe la consola.

La primera vez que imprima, obtendrá una lista con el elemento 0. En la segunda ocasión obtendrá la lista 1 0. En la tercera se imprimirá la lista 2 1 0. Y así sucesivamente. Observe que los datos ingresados avanzan una posición tras cada iteración, de manera que en la primera impresión el dato _0_ se encuentra en el índice 0 de la lista, pero en la segunda impresión el índice 0 es ocupado por _1_, dejando a _0_ en el índice 1. En la tercera impresión, el índice 0 es ocupado por _2_, el índice 1 por _1_ y el índice 2 por _0_.

Continúe dando clic en [bng]. Observe que la lista crece sin límite un índice con cada clic. En cambio, lo que se busca es obtener una lista finita con _n_ cantidad de unos. Vuelva a la idea principal y conserve la demostración anterior para futura referencia.

Para lograr la cantidad finita que busca necesitará una pequeña combinación de varios objetos incluyendo uno llamado [until]. Este objeto envía la cantidad que se le indique de _bangs_ de forma ultra rápida y resulta muy conveniente cuando se requieren varias iteraciones de un mismo proceso de manera inmediata. [until] puede ser iniciado con un argumento; sin embargo, en esta receta el argumento a utilizar será una variable indicada con la interfaz gráfica de usuario [hradio].

Con el modo de edición activo, agregue estos dos elementos conectando la salida de [until] a la caja de mensaje con el dato _1_. Haga clic derecho sobre [hradio] y vaya a _Propiedades_. En la ventana emergente cambie el número de celdas a 9. Ahora bien, agregar [until] a la receta sólo le permitirá prescindir de hacer clic manualmente en repetidas ocasiones, pero este objeto no va a modificar en gran medida lo programado anteriormente. Aún necesita que [list] sea vaciado en algún momento para que no acumule todas las listas que genere. Al final del proceso, sólo requiere imprimir la última lista que se genere de la iteración activada por [until]; de otra forma, obtendría la estructura piramidal que observó en la consola de Pure Data al comienzo de esta receta.

Agregue un objeto [t] y conéctelo a la salida de [hradio]. Desconecte [print unos] de [list]. Inserte otro objeto [list] y conecte la salida del primero a la entrada derecha del nuevo. Conecte a la salida del último objeto [list] el objeto [print unos]. El objeto [t] le ayudará a ordenar los diferentes eventos que deben suceder: el reinicio del arreglo [list]><[t l], el comienzo de disparos de [until] y la impresión de la última lista. El orden de los eventos será el mismo que se describe: primero un _bang_ a la entrada derecha del objeto [list] iterado para reiniciarlo, luego un _float_ a la entrada izquierda de [until] para que se efectúe la cantidad deseada de _bangs_, y al final un _bang_ a la entrada izquierda del último objeto [list] para imprimir la última lista guardada. Agregue los argumentos necesarios a [t] y conecte sus salidas a los objetos mencionados. Recuerde que el orden de los mensajes enviados por [t] sucede de derecha a izquierda.

Con esta programación obtendrá la cantidad de unos que especifique a través de [hradio], a excepción de cuando éste es 0. Al dar clic sobre la primera casilla de izquierda a derecha de [hradio] obtiene la última lista creada; esto se debe a que, aunque reinicia el arreglo [list]><[t l], [until] recibe 0 como argumento, por lo que nunca acciona ningún _bang_. En cambio, el último objeto [list] de la cadena sí recibe un mensaje _bang_, y debido a que [until] nunca iteró el arreglo [list]><[t l], la lista a imprimir sigue siendo la última creada. Esto puede ser solucionado al agregar una puerta que permita el paso del dato, siempre y cuando éste no sea cero.

Active el modo de edición y desconecte [hradio] de [t b f b]. Agregue un objeto [spigot] con argumento _0_. [spigot 0] no permite el paso de los datos ingresados por su entrada izquierda a menos que por su entrada derecha se le indique un valor que no sea cero. ¿Qué es lo que debe hacer primero? ¿Enviar la información y luego abrir la puerta, o al revés? 

Recuerde que los mensajes en Pure Data no son información continua, sino que se generan una sola vez a partir de un evento y se transforman al pasar de un objeto a otro o se detienen ante una barrera. No sucede lo mismo con los objetos de audio, que se mantienen activos desde el momento en que son colocados, mientras el procesador de señales digitales _DSP_ esté encendido. Usted está trabajando sólo con objetos de control de mensaje y puede darse cuenta de ello porque ninguno lleva en su nombre el signo de ~, como sería el caso de [osc~ 440]. 

La respuesta a la pregunta sobre lo que debe hacer primero es que deberá evaluar el dato para asegurarse de que no sea cero y sólo después lo enviará a intentar cruzar la puerta. Inserte entonces un objeto _trigger_ de la siguiente manera: [t f f], conecte las salidas derecha e izquierda a las entradas correspondientes de [spigot 0]. Por la entrada de [t f f] deberá enviar el dato que se genera al usar [hradio].

Recuerde el compás en el que aplaudía cuatro veces y se mantenía en silencio durante cuatro pulsos. La lista en ceros y unos se veía de la siguiente forma:

1 1 1 1 0 0 0 0

Si usted hace clic sobre [hardio] en la quinta casilla de izquierda a derecha con el modo de edición inactivo, obtendrá cuatro unos como en la lista de arriba, pero ningún 0. Si aplaudiera siguiendo esta lista de cuatro índices saturada de unos, no habría pulso en el que sus manos descansaran. De nueva cuenta, el ritmo adquiere sentido sólo cuando existen silencios. En los ejercicios introductorios cada lista escrita cuenta con ocho índices para mantener la referencia de un compás regular; por el momento conservaremos el número 8 como la longitud de las listas a crear, aunque más adelante usted podrá especificar otro valor.

El espacio total disponible en una lista corresponde a su longitud en índices, por lo que si desea crear una lista con ocho datos, la longitud de ésta deberá ser de ocho índices. Al generar una lista con cuatro unos, necesitará colocar también cuatro ceros para completar el espacio total disponible y obtener una lista con ocho índices. Por lo tanto, podemos afirmar que:

longitudTotal = longitudLista0 + longitudLista1

Donde _longitudLista0_ es igual al número de índices en la lista de ceros; _longitudLista1_ es a el número de índices en la lista de unos y _longitudTotal_ es el espacio total disponible. En este caso se conocen las variables _longitudTotal_ y _longitudLista1_. Para averiguar la longitud de la lista de ceros, habrá que restar a la longitud total la longitud de la lista de unos:

longitudLista0 = longitudTotal – longitudLista1

longitudLista0 = 8 – 4

longitudLista0 = 4

Suponga otro caso en el que la longitud total también es de ocho índices, pero la longitud de la lista de unos corresponde a sólo un índice:

longitudLista0 = longitudTotal – longitudLista1

longitudLista0 = 8 – 1

longitudLista0 = 7

En este segundo ejemplo se necesitan siete ceros para completar la lista de ocho índices ya que se indica una lista de sólo un dato correspondiente al valor _1_.

Programar en Pure Data la lista de ceros que se requiere a partir de la cantidad de unos especificada es bastante sencillo. Deberá insertar los objetos [t] y [-], incluyendo también una caja de mensaje y una caja de número. Con el objeto [-] usted restará al espacio disponible (que en este caso es 8) la cantidad de unos requerida a través de [hradio]. Escriba en la caja de mensaje el dato _8_ y conéctela a la entrada izquierda de [-]. Inicialice [t] con los argumentos _b_ y _f_. Conecte la salida del mensaje float que envía [t] a la entrada derecha de [-], y la salida izquierda de [t] a la caja de mensaje. Agregue la caja de número al final de la cadena y envíe la salida de [hradio] a [t b f]. Salga del modo de edición y elija diferentes valores usando [hradio]. Observe que en la caja de número obtiene efectivamente la cantidad de índices que necesita para elaborar una lista con longitud de 8 dada _n_ cantidad de unos.

Para obtener la nueva lista de ceros que se requieren active el modo de edición, seleccione todos los objetos que se encuentran conectados para crear la lista de unos a excepción de [hradio], cópielos y péguelos junto a lo que ya tenía programado. Conecte el resultado de la resta al objeto [t f f] que corresponde a la creación de la lista de ceros. En esta nueva cadena, borre de la caja de mensaje el dato _1_ y escriba en su lugar _0_, cambie también [print unos] a [print ceros].

Salga del modo de edición y haga clic sobre [hradio]. Observe cómo en la consola de Pure Data se imprimen las listas de unos y ceros con la proporción adecuada.

Lo único que falta para terminar de llevar a cabo esta receta es combinar ambas listas para preparar los datos a permutar. Para ello necesita un nuevo objeto [list], pero ahora con el argumento _store_. Este argumento inicializará el objeto [list] en la modalidad de concatenación de listas. Recuerde que normalmente en todos los objetos la entrada izquierda es caliente y el resto son frías. Aunque es posible guardar listas por ambas entradas de [list store], sólo la información que se introduzca por la entrada izquierda ocasionará una salida inmediata del contenido del objeto. Por lo tanto, deberá enviar primero una lista por la entrada derecha y luego por la izquierda. Para mantener cierto orden, la lista enviada a la entrada fría de [list store] será aquella que contiene los ceros. 

Imagine que elige 0 en [hradio], no requeriría ninguna cantidad de unos y por ello se crea una lista de ocho índices donde todos los valores son cero. Esta lista ingresa a [list store] por la entrada fría, mientras que ninguna lista de unos se introduce por la entrada caliente porque la iteración de unos no sucede. Si no ingresa ningún dato por la entrada izquierda de [list store], entonces este objeto no envía nada a través de su salida. ¿Cómo arreglar esto? Para lograr obtener esta lista desaturada de unos tendrá que enviar primero la lista de ceros y luego un mensaje _bang_ cada vez que [hradio] sea igual a cero.

Active el modo de edición, desconecte los objetos [print unos] y [print ceros], colóquelos en una sección aparte. Agregue el objeto [list store] y un objeto [t] con los argumentos _b_ y _l_. Conecte la salida del objeto [list] que captura la lista final de ceros a la entrada del nuevo [t b l]. Inserte un objeto [spigot] con el argumento _0_. Conecte la salida derecha de [t b l] a la entrada fría de [list store] y la salida izquierda a la entrada caliente de [spigot 0]. [spigot 0] necesita cambiar de argumento para permitir el paso de datos por su entrada izquierda, para ello agregue el objeto [==] con argumento _0_ y conecte su salida a la entrada fría de [spigot 0]. Localice el objeto [t b f] que ordena los datos para realizar la operación de resta para la lista de ceros, justo arriba de la caja de mensaje con el dato _8_. Conecte la salida derecha de [t b f] a la entrada caliente de [== 0]. Envíe la salida de [spigot 0] a la entrada izquierda de [list store]. A la entrada caliente de [list store] también deberá enviar la salida del objeto [list] que captura la lista final de unos.

Por último, desconecte [hradio] y añada el último objeto [t] con los argumentos _f_, _f_ y _b_. Conecte la salida de [hradio] a la entrada de este objeto [t f f b]. Lo primero que hará con este objeto será reiniciar [list store] con un mensaje _bang_; para ello, conecte la última salida de [t f f b] a la entrada fría de [list store]. Después enviará un _float_ para calcular la cantidad de ceros necesaria. Conecte la salida de enmedio de [t f f b] al objeto [t b f] que ubicó hace unos momentos y que se encuentra arriba de la caja de mensaje con el dato _8_. A continuación, conecte la salida izquierda de [t f f b] al objeto [t f f ] en la sección de creación de la lista de unos. Antes de salir del modo de edición agregue un objeto [print] con el argumento saturación y conéctelo a la salida de izquierda de [list store].

Desactive el modo de edición y haga clic sobre [hradio], recorriendo cada casilla mientras observa la consola de Pure Data. Tras cada impresión deberá obtener una lista con una longitud total de ocho índices. Cuando seleccione la primera casilla de [hradio], todos los índices deberán contener el dato _0_. Al hacer clic en la última casilla, todos los índices contendrán el dato _1_. Al elegir cualquier casilla en medio, deberá obtener una lista donde los primeros índices contienen el dato _1_ y el resto son ceros.

Con esta receta podrá generar listas con mayor o menor saturación de índices con el valor _1_. Experimente un poco con ella, trate de leerla índice por índice en Pure Data y asigne la lectura de los datos a un [tgl] para controlar la actividad de un objeto [metro]. Tome también
un pequeño descanso antes de continuar con la siguiente receta.


---

[^^^](#contenido)

<span id="permutación-aleatoria">
    
## Permutación aleatoria </span>

_Pure Data 0.49.1_

Materiales:

* 4 [bng]
* 5 [t]
* 3 cajas de mensaje
* 1 [until]
* 1 [random]
* 1 [<]
* 3 [list]
* 1 [list length]
* 1 [list prepend]
* 2 [list split]
* 1 [list store]
* 1 [route]
* 2 [print]

Imagine un saco opaco con ocho esferas dentro, cada una de distinto color. De manera aleatoria usted saca una esfera, la observa y toma nota de su color en una libreta. Devuelve la esfera al saco y repite el proceso. Esto es un sorteo sin reemplazo, en el que cada esfera retirada regresa al conjunto de donde provino. En un sorteo de esta naturaleza el conjunto siempre mantiene el mismo número de elementos. Ahora imagine que cada vez que saca una esfera apunta el color que obtuvo y no la vuelve a depositar en el saco; esto afecta el tamaño del conjunto y también la posibilidad de obtener el mismo color. A un sorteo con estas características se le conoce como sorteo con reemplazo.

En Pure Data es posible realizar sorteos sin reemplazo de manera muy rápida utilizando [random]. Este objeto crea un intervalo semiabierto de números enteros que comienza en 0 y termina en el valor indicado por su argumento. Por ejemplo, [random 8] puede describirse como el intervalo _\[0 -- 8)_. Cuando envía un _bang_ a [random 8] obtiene cualquier número entero mayor o igual a cero y menor a ocho. Al volver a enviar un _bang_, existe la posibilidad de obtener el mismo número que en la ocasión anterior.

También es posible realizar sorteos sin reemplazo a través del objeto [urn]; sin embargo, éste sólo se encuentra disponible en la versión extendida de Pure Data o en la distribución principal si ha descargado e instalado la librería adecuada. Por el interés de esta publicación en desarrollar conceptos con ayuda de los elementos más básicos disponibles en Pure Data, el algoritmo de permutación aleatoria será explicado en la versión vanilla y con los objetos que usted ya conoce.

Considere una lista llamada _L_ que contiene los números enteros del intervalo semiabierto _\[0 - 8)_ ordenados de menor a mayor:

L = 0 1 2 3 4 5 6 7

Esta forma es conveniente porque nos permite observar que cada elemento coincide con su índice _idx[n]_:

idx0 = 0

idx3 = 3

Ahora, se elige aleatoriamente un índice de esta lista, por ejemplo _idx6_,

y se apunta el dato en _listaP_.

L: 0 1 2 3 4 5 6 7  
idx\[random 8) = 6  
listaP = 6

Para lograr un sorteo sin reemplazo de _L_ no se regresa el elemento tomado a la lista de donde provino, pero se conserva en _listaP_ y ocupa en ella el índice 0. Si no se devuelve el elemento a _L_, dicha lista pierde un índice. Ahora el intervalo semiabierto que corresponde para elegir un índice de forma aleatoria es _\[0 - 7)_. Se repite el proceso:

L: 0 1 2 3 4 5 7  
idx\[random 7) = 3  
listaP = 3 6

El índice elegido fue _idx3_ y se ha anotado en _listaP_, a la izquierda del último dato guardado. Esta es la misma forma que se utiliza en Pure Data cuando se programa un retorno [list]><[t l], donde el primer dato ingresado ocupa el último índice. La lista _L_ se actualiza y el intervalo semiabierto se modifica a _\[0 - 6_). Se continúa con el proceso:

L: 0 1 2 4 5 7  
idx\[random 6) = 5  
listaP = 7 3 6

Se prosigue hasta vaciar _L_:

L: 0 1 2 4 5  
idx\[random 5) = 3  
listaP = 4 7 3 6

L: 0 1 2 5  
idx\[random 4) = 0  
listaP = 0 4 7 3 6

L: 1 2 5  
idx\[random 3) = 2  
listaP = 5 0 4 7 3 6

L: 1 2  
idx\[random 2) = 0  
listaP = 1 5 0 4 7 3 6

L: 2  
idx\[random 1) = 0  
listaP = 2 1 5 0 4 7 3 6

Cuando _L_ se encuentre vacía, _listaP_ estará completa y será una versión permutada de la lista con la que se comenzó. Observe que, aunque los índices que se eligen de forma aleatoria pueden repetirse, los datos elegidos son siempre distintos porque una vez apartados no pueden volver a ser seleccionados.

Esta es una variante del algoritmo para desordenar los elementos de una lista que los estadistas Ronald Fisher y Frank Yates describieron en 1938 en el libro _Statistical Tables for Biological, Agricultural and Medical Research_. Es un ejercicio muy fácil de desarrollar en papel; sugiero realizarlo un par de veces más para que el algoritmo le sea mucho más claro antes de programarlo en Pure Data.

Para comenzar, retome la receta Selección de índices de _[iom!] #1 Control Base_. Si no la tiene a la mano, puede consultarla aquí:

Esta receta le permite obtener por la salida izquierda de [list split 1] cualquier dato que elija a través de [hradio] de la lista que pasa por la entrada derecha de [list]. Active el modo de edición en Pure Data, agregue un objeto [print] con el argumento _datoRemovido_ y conéctelo

a la salida izquierda del objeto [list split 1]. Añada [list store] y conecte a su entrada caliente la salida de izquierda de [list split]; por la entrada fría conecte la segunda salida de [list split 1]. A través de estas conexiones recuperará el resto de la lista que no se había ocupado en la receta _Selección de índices_. Agregue otro objeto [print], esta vez con el argumento _datosRestantes_, y conéctelo a la salida izquierda de [list store].

Para activar la selección aleatoria de un índice deshágase de [hradio]. Conecte en su lugar un [bng] y diríjalo a la entrada caliente de un objeto [random 8] y éste al objeto [t b f]. Salga el modo de edición, haga clic sobre la caja de mensaje que contiene la lista de números y luego presione [bng]. Observe que en la consola de Pure Data se imprime el dato que corresponde al índice aleatorio elegido por [random 8], así como una lista con los demás datos.

Para que los números se sigan descartando debe actualizar la lista _L_ guardada en el objeto [list], así como el intervalo semiabierto de números enteros que dispone [random 8] a partir de la longitud de la lista que imprime por [list store]. Active el modo de edición y desconecte [print datosRestantes]. Coloque un objeto [t l l l] en su lugar. A la salida derecha de este objeto vuelva a conectar [print datosRestantes] y envíe la salida de enmedio a la entrada izquierda de [list] para actualizar los datos disponibles. Para conocer la longitud de cualquier lista invoque el objeto [list length] y conecte a su entrada la salida izquierda del objeto [t l l l]. [random 8] deberá recibir el resultado de [list length] por su entrada fría. Desactive el modo de edición y haga clic sobre [bng]; la lista datosRestantes se hará cada vez más pequeña en la consola y _datoRemovido_ nunca se repetirá. Cuando _datosRestantes_ imprima _bang_ será necesario restaurar el algoritmo. Haga esto añadiendo un [bng] y una caja de mensaje con el dato _8_. Entre al modo de edición, conecte este [bng] a la caja de mensaje recién agregada y también a la caja de mensaje que contiene la lista de números. Salga del modo de edición, dé clic en éste nuevo [bng] y luego en el que se dirige a [random 8].

Ahora los datos ya son removidos de manera aleatoria, pero aún no son guardados de tal manera que generen una lista. Para ello, realice las conexiones que ya conoce para concatenar dato por dato en una lista: active el modo de edición, agregue un retorno de los objetos [list]><[t l] y conecte también la salida de [list] a la entrada izquierda de otro objeto [list], añada un [bng] a la entrada caliente de éste último [list]. Desconecte [print datosRestantes], cambie su argumento a _listaP_ y conéctelo al último objeto [list] que agregó. Desconecte también [print datoRemovido] y deséchelo. Conecte la salida izquierda de [list split 1] a la entrada caliente de [list] en el arreglo [list]><[t l]. 

Para acelerar el proceso de selección de índices de manera aleatoria, desconecte el [bng] que va a la entrada caliente de [random 8]. Tome un objeto [until] y una caja de mensaje con el dato _8_. Esta caja de mensaje debe indicar cuántas veces es necesario iterar la selección aleatoria de índices. Conecte [bng] a la caja de mensaje, ésta a la entrada caliente de [until] y éste a la entrada izquierda de [random 8]. Salga del modo de edición, haga clic primero en el [bng] que reinicia [random 8] y prepara la lista, luego en el [bng] que activa los ocho disparos de [until] y por último en el [bng] que empuja la última listaP guardada.

Lo único que falta es sintetizar estos tres clics en uno solo que active todo el algoritmo y le permita permutar la lista de manera rápida las veces que se desee. Para ello, entre al modo de edición, agregue un último objeto [t] y un [bng]. Debido a que sólo se necesita ordenar tres bangs, los argumentos para [t] deben ser _b_, _b_ y _b_. Conecte [t b b b] respetando el orden de los eventos: reinicio, iteración e impresión. No olvide conectar [bng] a [t b b b]. Si lo desea, puede desechar los otros objetos [bng] anteriormente colocados, sólo mantenga el orden indicado. Desactive el modo de edición y haga clic sobre el [bng] superior. Observe en la consola cómo la lista es permutada tras cada _bang_.

Como ejercicio, sustituya la lista 0 1 2 3 4 5 6 7 por una escala musical en valores MIDI y envíe los datos de la lista permutada a un oscilador en Pure Data. Haga uso de un contador y un objeto [mtof].

---

[^^^](#contenido)

## Bombo
_Pure Data 0.49.1_

Materiales:

* 1 [bng]
* 1 caja de número
* 3 cajas de mensaje
* 1 [/]
* 1 [t]
* 1 [del]
* 2 [pack]
* 2 [mtof]
* 2 [*~]
* 1 [osc~]
* 1 [dac~]
* 2 [line~]

Esta última receta expande la percusión creada en _[iom!] #1 Control base_. En esta ocasión la envolvente que se utilizará no sólo es aplicada al amplificador, sino también a la frecuencia del oscilador. Disfrutará mucho de este sonido y le será muy útil para complementar cualquiera de las recetas. 

Active el DSP en la ventana de Pure Data, entre al modo de edición, tome un objeto [osc~] y conéctelo a la entrada izquierda de un objeto [\*~] con argumento _0.5_. Conecte [\*~ 0.5] a otro amplificador [\*~] y éste a ambas salidas del convertidor de audio digital a análogo [dac~]. No escuchará nada porque aunque el oscilador sinusoidal [osc~] está conectado al amplificador [\*~ 0.5], no posee ningún argumento que indique el número de ciclos de onda por segundo que debe realizar. A la par, al último objeto [\*~] no se le especificó ningún argumento. Ambos [osc~] y [\*~] están inicializados de manera predeterminada en _0_. 

Antes de amplificar el sonido, desarrolle una envolvente que afecte la frecuencia del oscilador utilizando el generador de rampas [line~] de la siguiente manera: Conecte un objeto [line~] a la entrada izquierda de [osc~], agregue dos cajas de mensaje y conecte debajo de cada una un objeto [mtof] para que pueda asignar valores MIDI en las cajas de mensaje. Envíe uno de los objetos [mtof] a la entrada caliente de [line~]. El otro objeto [mtof] debe ser conectado primero a [pack] con los argumentos _0_ y _10_ y éste a la entrada izquierda de [line~]. El objeto [pack 0 10] se encargará de entregar una lista a [line~] para ejecutar la rampa deseada. Cada argumento en [pack] crea una entrada con la que coincide en orden de izquierda a derecha, por lo tanto la entrada izquierda afectará el argumento _0_ y la entrada derecha afectará el argumento _10_. 

Como en el resto de los objetos, sólo la entrada izquierda será caliente, por lo que al enviar otro dato por la entrada derecha, [pack] no envía ningún mensaje, sólo cambia el argumento correspondiente.

La lista que entrega [pack] a [line~] consta de dos elementos, siendo el índice 0 la meta a la que debe llegar [line~] y el índice 1 el tiempo en milisegundos que dura la rampa. Por el momento el tiempo especificado es de 10 ms, usted puede ajustar este valor a su preferencia una vez que escuche el sonido generado. 

Complete la envolvente agregando un [bng] conectado a un objeto [t] con los argumentos _b_ y _b_. Envíe la salida derecha a la primera caja de mensaje que no es interceptada por [pack 0 10] y la salida izquierda a la segunda. Apunte dos valores MIDI en cada caja. Como ejemplo, se utilizan dos notas Sol distanciadas por cuatro octavas: _G2_ y _G6_ que corresponden en MIDI a _43_ y _91_ respectivamente. El valor 91 es anotado en la caja de mensaje que pasa a [line~] directamente de [mtof], mientras que el valor _43_ es anotado en la caja de mensaje restante. Esta envolvente tiene la finalidad de hacer que [osc~] tenga una frecuencia en Hz. correspondiente a _G6_ de forma inmediata y se deslice a _G2_ a través de una rampa de 10 milisegundos de duración.

Para escuchar el efecto, construya una envolvente rápida para el amplificador [\*~]. Agregue de nuevo una caja de mensaje con los datos _1_ y _1_; conéctela a un nuevo objeto [line~] por su entrada caliente. Agregue un objeto [pack] con los argumentos _0_ y _80_. Mande [pack 0 80] también a la entrada caliente de [line~]. Añada un objeto [del] con el argumento _2_ y conéctelo a la entrada izquierda de [pack 0 80]. Conecte [line~] a la entrada derecha del último amplificador [\*~] y [bng] a la caja de mensaje con la lista _1 1_ y al objeto [del 2]. Esta envolvente de amplitud permitirá abrir el amplificador a su mayor potencia en el corto de tiempo de 1 milisegundo. 2 ms después será indicado que vuelva a _0_ en un intervalo de 80 milisegundos. Salga del modo de edición, ajuste el volumen de su ordenador o interfaz de audio a un nivel bajo y haga clic sobre [bng]. Suba lentamente el nivel de su equipo hasta que sea confortable.

Una mejora que puede realizarse para que el sonido de este bombo sea más flexible y manipulable es agregar una caja de número y conectarla a la entrada izquierda de [pack 0 80], con ello afectará el tiempo de decaimiento de la percusión sintetizada. Añada también un objeto [/] con el argumento _20_, conecte su salida a la entrada derecha de [pack 0 10]. Por la entrada caliente de [/ 20] envíe la misma caja de número que envió a [pack 0 80]. De esta manera al afectar el tiempo de decaimiento de la envolvente de amplitud, también actualiza el tiempo de decaimiento de la envolvente de frecuencia, dándole más carácter al sonido sintetizado.

Con envolventes de este estilo usted puede complejizar los sonidos que los osciladores que Pure Data ofrecen. Experimente creando una envolvente que afecte la frecuencia de corte de un filtro pasa altos [hip~] con el que modula un generador de ruido blanco [noise~] y agregue una envolvente de amplitud. 

---

[^^^](#contenido)

## Para finalizar

Ha llegado el momento más emocionante: ¡Conectar todas las recetas juntas! Las listas que se generan al usar la receta [Saturación](#saturación) aparecen siempre ordenadas, reorganice de forma aleatoria los datos usando la receta [Permutación aleatoria](#permutación-aleatoria). Requerirá de un objeto [t] con los argumentos _b_ y _l_. Para leer las listas permutadas, necesitará la receta _Selección de índices_, que es el núcleo de [Permutación aleatoria](#permutación-aleatoria), y un contador, el cual realizó para entender cómo se concatenan las listas en el arreglo [list]><[t l]. Mande la lista permutada y el contador al selector de índices, de esta forma cada vez que avance la cuenta, se elegirá el índice correspondiente en la lista permutada. Permita que el contador avance en un intervalo de tiempo constante utilizando un objeto [metro], le sugiero que lo inicialice en 125 ms. Al final, envíe la salida de _Selección de índices_ a un objeto [sel] y active la percusión cada vez que el dato sea 1.

Ya expuestas las recetas que conforman este número, _[iom!] #2 Densidad_ llega a su fin. Mas no se d      etenga aquí, aplique las recetas a otros algoritmos que haya realizado y problematice la situación: ¿Qué pasaría si lee ambos unos y ceros con dos sonidos distintos? ¿O si genera listas permutadas a partir de los datos 0, 1 y 2?, ¿Y si estas listas son el ritmo que estructura una melodía?

Es un gusto enorme escribir para usted. ¡Disfrute y hasta la próxima!


---

[^^^](#contenido)

**[iom!] Experimentos rítmicos en Pure Data: #1 Control base**

**Edición:** Isaac Medina y Paola Sandoval

**Textos:** Isaac Medina

**Diseño editorial:** Paola Sandoval

---

This work is licensed under the Creative Commons Attribution 4.0 International License. To view a copy of this license, visit http://creativecommons.org/licenses/by/4.0/ or send a letter to Creative Commons, PO Box 1866, Mountain View, CA 94042, USA.
