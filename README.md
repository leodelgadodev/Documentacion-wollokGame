# Documentacion-wollokGame (actualizado a la versión 1.7.6)


[Wollok](https://www.wollok.org/) es un lenguaje y un entorno de programación orientado a objetos que se creó con el propósito de usarlo como herramienta educativa. Dentro del mismo tenemos la capacidad de, entre otras cosas, crear nuestros propios juegos mediante las funcionalidades de wollokGame.


Esta guía pretende extender [la documentación ya existente de wollokGame](https://www.wollok.org/documentacion/conceptos/) y mostrar algunas cosas que se pueden hacer con él. Se recomienda leer esto después de ver la documentación oficial. <strong>Esta guía NO busca reemplazar las clases presenciales en las que se expliquen conceptos de las materias, ni enseñar sobre el paradigma de objetos ni sobre patrones de diseño.</strong> Vale decir que <strong>esta documentacion NO es oficial</strong>.


Dentro del entorno también se incluye una documentación (en inglés) con todos los métodos disponibles. Para encontrarlo, basta con apretar <code>CTRL + Shift + F3</code> y en la ventana que sale buscar el archivo <code>game.wlk</code>.

## Position


Se representa el juego mediante un tablero con casilleros. Si queremos agregar un objeto en el mismo, aparte de definir la parte visual (dándole una imagen), debemos definir una posición.


Las posiciones son instancias de la clase <code>Position</code>. Cuando colocamos un objeto en el tablero usando <code>game.addVisualIn(obj,pos)</code>, estamos creando un nuevo objeto <code>Position</code> que es referenciado por el objeto que agregamos en el tablero (osea, cuando definimos la posición del objeto). 

Cuando usamos, por ejemplo, game.at(3,5), estamos creando un nuevo objeto Position con los valores 3 (para x) y 5 (para y).

Cuando usamos los métodos <code>pos.up(num), pos.down(num), pos.left(num) y pos.right(num)</code> para, por ejemplo, mover el objeto que representa al jugador, estamos retornando nuevas posiciones con alguno de sus valores modificado.

Se pueden comparar dos posiciones por igualdad (dos posiciones son iguales si sus valores x e y son iguales). basta con hacer <code>posicion1 == posicion2</code>.

También puedo, si quiero, tomar sólo el valor x o el valor y de una posición, usando <code>posicion.x()</code> o <code>posicion.y()</code>.

Puedo calcular la distancia entre dos posiciones usando el [teorema de Pitágoras](https://es.wikipedia.org/wiki/Teorema_de_Pit%C3%A1goras) y el método <code>posicion1.distance(posicion2)</code>.

La implementación completa de la clase Position se encuentra disponible en el archivo game.wlk.

## Sonidos

WIP.

## Colisiones

Dos objetos colisionan si tienen la misma posición. Existe un método que ejecuta código cuando dos objetos chocan (el jugador con una cosa, por ejemplo). Este método es <code>game.whenCollideDo( objetoQueEsColisionado, {..bloque de código..} )</code>.

<code>game.whenCollideDo(pepita, { comida => pepita.comer(comida) })</code> - En este ejemplo, cualquier objeto que colisione con Pepita ocasionará que a la misma se le envíe el mensaje comer, con el objeto que colisionó como parámetro.

<strong>Disclaimer:</strong> En el ejemplo de arriba, se ejecuta cada vez que pepita colisiona con algún objeto. Tendría sentido si en el tablero los únicos objetos que tengo fueran Pepita y comidas que puedan ser consumidas por Pepita. Si tuviera, por ejemplo, otras aves en el tablero, y a Pepita se le permite colisionar con dichas aves, se las va a intentar comer :scream: :scream: :scream:

En el bloque de código definimos <code>comida</code> como un parámetro que recibe el bloque, pero en realidad, puede ser cualquier objeto. Nosotros, en el momento, decidimos elegir la palabra comida para que quede más legible, pero wollokGame no sabe que los objetos son comida. Una descripción más literal del ejemplo (pero menos descriptiva) sería:

<code>game.whenCollideDo(pepita, { unObjeto => pepita.comer(unObjeto) })</code> - Puede pasar que justo, el objeto con el que pepita colisiona, es otra ave. O algún muro. O alguna persona.

Una solución es, si sabemos que Pepita es el único objeto que se mueve por el tablero (la mueve el jugador), en lugar de consultar si Pepita colisiona con cualquier objeto (que podría ser comida o no), verificamos si la comida colisiona con cualquier objeto (que en este caso, el único objeto que podría llegar a colisionar, es Pepita).

<code>game.whenCollideDo(manzana, { unAve => unAve.comer(manzana) })</code> 

<code>game.whenCollideDo(alpiste, { unAve => unAve.comer(alpiste) })</code> 

También podríamos, en lugar de decirle al ave que colisiona, enviarles mensajes a las comidas para que evalúen según quién las colisionó:

<code>game.whenCollideDo(manzana, { unAve => manzana.colisionasteCon(unAve) })</code> 

<code>game.whenCollideDo(alpiste, { unAve => alpiste.colisionasteCon(unAve) })</code> 


## Conocer los objetos que tengo en una posición determinada (por ejemplo, en la posición en la que se encuentra el jugador, o en la casilla adyacente a la derecha al jugador)

Es útil conocer objetos en una posición determinada. Me sirve para saber si el objeto que tengo enfrente lo puedo atravesar o no, si puedo interactuar con él, cómo interactúo con él según el objeto que tengo debajo, etc.

Existen tres formas de conocer los objetos en una posición. El primero es usando el método <code>posicion.allElements()</code> que me devuelve una lista con todos los objetos que existen en una posición determinada. Vale decir que cualquier posición entiende este mensaje, así que podría hacer <code>game.at(12,9).allElements()</code>, <code>jugador.position().allElements()</code> (si se la definí), <code>game.origin().allElements()</code>, etc. 

El segundo es utilizando el método <code>game.colliders(unObjeto)</code>, que denota una lista con todos los objetos que colisionan con el objeto que le pasamos como parámetro (osea, los objetos que estén en la misma posición). 

Por ejemplo, dentro del objeto que representa al jugador, quiero un método que me diga los objetos con los que estoy colisionando ahora mismo. Podría construirlo de la siguiente forma:

<code>object Jugador {

  method objetosDebajoDeMi() {
    return game.colliders(self)
  }
  
}</code>

* colliders, getObjectsIn, position.allElements()

## Hacer que un objeto no pueda ser colisionado por el jugador

Recordemos que una colisión es cuando dos objetos tienen la misma posición. 

* onTick, removeTickEvent

* Animacion del personaje (que cambie su orientacion)

* Animacion de objetos

* Generar muros y modificar tamaño del "tablero"

* Generar objetos aleatoriamente en el tablero

* combinaciones de teclas (a lo Street Fighter)

* Barra de status / menu interfaz
