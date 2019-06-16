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

Dos objetos colisionan si tienen la misma posición. Existe un método que ejecuta código cuando dos objetos chocan (el jugador con una cosa, por ejemplo). Este método es <code>game.whenCollideDo( objeto, {..código..} )</code>. Por ejemplo:

<code>game.whenCollideDo(pepita, { comida => pepita.comer(comida) })</code>

Disclaimer:

## Conocer los objetos que tengo en una posición determinada (por ejemplo, en la posición en la que se encuentra el jugador, o en la casilla adyacente a la derecha al jugador)

Es útil conocer objetos en una posición determinada. Me sirve para saber si el objeto que tengo enfrente lo puedo atravesar o no, si puedo interactuar con él, 

* colliders, getObjectsIn, position.allElements()

* onTick, removeTickEvent

* Animacion del personaje (que cambie su orientacion)

* Animacion de objetos

* Generar muros y modificar tamaño del "tablero"

* Generar objetos aleatoriamente en el tablero

* combinaciones de teclas (a lo Street Fighter)

* Barra de status / menu interfaz
