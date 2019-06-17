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

El segundo es usando el método <code>game.getObjectsIn(posición)</code>, que funciona exactamente igual al anterior.

El tercero es utilizando el método <code>game.colliders(unObjeto)</code>, que denota una lista con todos los objetos que colisionan con el objeto que le pasamos como parámetro (osea, los objetos que estén en la misma posición). 

Por ejemplo, dentro del objeto que representa al jugador, quiero un método que me diga los objetos con los que estoy colisionando ahora mismo. Podría construirlo de la siguiente forma:

object jugador {

  method objetosDebajoDeMi() {
    return game.colliders(self)
  } 
}




## Hacer que un objeto no pueda ser colisionado por el jugador

Este problema tiene muchas soluciones posibles. A continuación se muestra una de ellas.

Supongamos que queremos contener al jugador dentro del tablero usando muros que no puedan ser atravesados y que el jugador sólo puede moverse un casillero hacia arriba, abajo, a la izquierda o a la derecha. Existiría un objeto que representa al jugador y otro objeto o clase que representa los muros.

Bastaría con hacer que, cada vez que se intente mover, compruebe si el objeto que tiene delante lo puede pasar por encima. Esto se consigue utilizando <code>getObjectsIn()</code> o <code>allElements()</code> para comprobar el objeto que tenga delante y preguntar si puede colisionar con él o no.

La cuestión es, ¿cómo puedo saber en qué dirección "estoy mirando"? No le puedo pasar todas las 4 direcciones posibles del jugador, porque no estoy intentando moverme en las cuatro direcciones y no debería evaluar todas ellas, estoy intentando moverme en una sola.

Hay dos soluciones: o cuando intento moverme compruebo en cuál de las cuatro direcciones estoy intentando moverme, y evaluar acorde a esa dirección, o le paso alguna orientación como parámetro en el momento en que intenta moverse, y evalúo a partir de ahí. Como la segunda resulta útil por si después quisiera cambiar la imagen del personaje según la dirección en la que está mirando, vamos a explorar esa.

```ruby
object jugador {

  method position() = game.at(4,8)
  method image() = "jugador.png"
  
  method mover( posicion, unaOrientacion ) { 
    if( self.puedeMoverAl( unaOrientacion ) { 
      ..etc..
    } else {
        ..no se mueve.. 
        }
  }
}

object muro {
  method image() = "muro.png"
  method esColisionable() = false
}
```

¿De dónde sale "unaOrientación" y por dónde se la puedo pasar? La obtengo de los métodos que uso para mover el personaje, dentro del programa .wpgm:

```ruby
  keyboard.up().onPressDo { personaje.mover(personaje.position().up(1),arriba) }
  keyboard.down().onPressDo { personaje.mover(personaje.position().down(1),abajo) }
  keyboard.left().onPressDo { personaje.mover(personaje.position().left(1),izquierda) }
  keyboard.right().onPressDo { personaje.mover(personaje.position().right(1),derecha) }
```

Esta orientación también podría representarse con strings, pero elegí representarla con objetos para poder consultar las direcciones en las que se puede mover mediante polimorfismo.

Para implementar el método <code>puedeMoverAl</code> se encuentran disponibles los métodos para verificar los objetos en una posición, <code>allElements()</code> y <code>getObjectsIn()</code>. Bastaría con enviarle un mensaje preguntándole a él o los objetos si los puedo pasar por encima (osea, <code>esColisionable()</code>).

También, para simplificar un poco el código, me puedo guardar un método dentro de la orientación que me devuelvan la posición de la celda adyacente al jugador.

```ruby
method puedeMoverAl( unaOrientacion ) {
  return 
    game.getObjectsIn( unaOrientacion.posicionEnEsaDireccion() ).all { unObj => unObj.esColisionable() }
}

object arriba {
  method posicionEnEsaDireccion() = jugador.position().up(1)
}

object abajo {
  method posicionEnEsaDireccion() = jugador.position().down(1)
}

object izquierda {
  method posicionEnEsaDireccion() = jugador.position().left(1)
}

object derecha {
  method posicionEnEsaDireccion() = jugador.position().right(1)
}
```

<strong>Disclaimer:</strong> para que esto funcione, todos los objetos del tablero que el jugador puede intentar colisionar deben entender el mensaje <code>esColisionable()</code>.



## Hacer que el personaje cambie de imagen según la orientación en la que se intenta mover

Aprovecharemos el código de la sección anterior para poder resolver esto.

Basta con extender la implementación para que el jugador se guarde su orientación, dentro de cada orientación nos guardemos la imagen correspondiente al jugador, e incluir un método para que actualice su imagen cada vez que se intente mover.

```ruby
object jugador {
  
  var orientacion = derecha //debemos inicializarla con la orientación a la que empieza viendo el jugador.
                            // Podría resolverse sin guardar la orientación en una variable, 
			    //y pasándole la orientación como parámetro al método actualizarImagen().

  method position() = game.at(4,8)
  method image() = orientacion.imagenDelJugador()
  
  method mover( posicion, unaOrientacion ) { 
    
    orientacion = unaOrientacion 
    self.actualizarImagen() 
    
    if( self.puedeMoverAl( unaOrientacion ) { 
      ..etc..
    } else {
        ..no se mueve.. 
        }
  }
  
  method actualizarImagen() {
    imagen = orientacion.imagenDelJugador()
		game.addVisual(self)
  }
}

object arriba {
  method imagenDelJugador() = "jugador-up.png"
  method posicionEnEsaDireccion() = jugador.position().up(1)
}

object abajo {
  method imagenDelJugador() = "jugador-down.png"
  method posicionEnEsaDireccion() = jugador.position().down(1)
}

object izquierda {
  method imagenDelJugador() = "jugador-left.png"
  method posicionEnEsaDireccion() = jugador.position().left(1)
}

object derecha {
  method imagenDelJugador() = "jugador-right.png"
  method posicionEnEsaDireccion() = jugador.position().right(1)
}
```

## Animaciones y eventos en el tiempo (onTick)

Existe una forma para ejecutar código después de una cierta cantidad de tiempo: el método <code>game.onTick(tiempo,"nombre", {..bloque de código..}</code>.

El tiempo se expresa en milisegundos (un segundo son 1000, 60 segundos son 60000, etc).

Con él se puede crear un cronómetro, generar enemigos cada cierto tiempo, hacer que se muevan cada cierto tiempo, o cambiar la imagen de los objetos cada cierto tiempo (osea, animaciones!). Incluso puedo hacer que tengan una animación sólo por una cierta cantidad de tiempo y luego vuelva a quedar estático.

Un ejemplo de una animación simple:

```ruby
object enemigo() {
  var property image = "enemigo_1.png"

  method bailar() {
      if (image = "enemigo_1.png") {
	 image = "enemigo_2.png"
	 } else {
	   image = "enemigo_1.png"
	 }
  }
}
```

Y con esto, agregando <code>game.onTick(500,"enemigo bailando",{enemigo.bailar()})</code> en el programa .wpgm, ya tiene una animación.

<strong>Disclaimer:</strong> remover el objeto <strong>NO</strong> remueve el onTick correspondiente (aunque no tira ningún error si borramos el objeto). Para remover el onTick se puede utilizar el método <code>game.removeTickEvent("nombre")</code>. Podemos decir, por ejemplo, que cuando aprete cualquier tecla la animación del enemigo se detenga.

```ruby
  keyboard.any.onPressDo {game.removeTickEvent("enemigo bailando")}
```

Usando otro <code>onTick</code> en conjunto con <code>removeTickEvent</code> puedo decirle que sólo baile durante ocho segundos:

```ruby
  game.onTick(8000,"remover baile enemigo", {
    game.removeTickEvent("enemigo bailando")
    game.removeTickEvent("remover baile enemigo")
  })
```

<strong>Otro disclaimer:</strong> puedo usar <code>game.addVisual</code>, <code>game.removeVisual</code>, <code>game.removeTickEvent</code>, <code>game.height()</code>, y la mayoría de los mensajes que entiende el objeto game en cualquier archivo, pero el onTick sólo lo puedo usar dentro del programa .wpgm.


## Generar muros alrededor del tablero

Este problema tiene muchas soluciones posibles. A continuación se muestra una de ellas.

Suponiendo que tengo un objeto muro con su visual correspondiente, puedo crear un método que me genere bordes en el tablero a partir de una colección de posiciones. Esta colección de posiciones la puedo generar utilizando un <code>forEach</code> por cada lado del tablero, intentando abarcar sus lados que van desde el origen hasta su posición más alta, desde el origen hasta su posición más ancha, desde su posición más ancha hasta su posición más alta y desde su posición más alta y ancha (el extremo opuesto a (0,0)) hasta sólo su posición más alta.

Para obtener todos estos puntos, podemos usar los métodos <code>game.width()</code> y <code>game.height()</code> que nos dicen el ancho y el alto del tablero, y enviarles rangos a todos los <code>forEach</code> para generar las posiciones una por una.

Una vez completa la colección de posiciones, podemos generar todos los muros.

```ruby
method generarMuros() {
   const ancho = game.width() - 1 // Debemos restarles uno para .
   const alto = game.height() - 1 // que las posiciones se generen bien
   const posicionesParaGenerarMuros = []
		
   (0 .. ancho).forEach{ num => posicionesParaGenerarMuros.add(new Position(num, alto))} // lado superior
   (0 .. ancho).forEach{ num => posicionesParaGenerarMuros.add(new Position(num, 0))} // lado inferior
   (0 .. alto).forEach{ num => posicionesParaGenerarMuros.add(new Position(ancho, num))} // lado derecho
   (0 .. alto).forEach{ num => posicionesParaGenerarMuros.add(new Position(0, num))} // lado izquierdo
    
   posicionesParaGenerarMuros.forEach {posicion => game.addVisualIn(self,posicion)}
}
```


## Sonidos

WIP.

