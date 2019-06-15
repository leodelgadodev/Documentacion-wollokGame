# Documentacion-wollokGame

[Wollok](https://www.wollok.org/) es un lenguaje y un entorno de programación orientado a objetos que se creó con el propósito de usarlo como herramienta educativa. Dentro del mismo tenemos la capacidad de, entre otras cosas, crear nuestros propios juegos mediante las funcionalidades de wollokGame.

Esta guía pretende extender [la documentación ya existente de wollokGame](https://www.wollok.org/documentacion/conceptos/) y mostrar algunas cosas que se pueden hacer con él. Se recomienda leer esto después de ver la documentación oficial.

Dentro del entorno también se incluye una documentación (en inglés) con todos los métodos disponibles. Para encontrarlo, basta con apretar CTRL + Shift + F3 y en la ventana que sale buscar el archivo game.wlk.

## Position

Se representa el juego mediante un tablero con casilleros. Si queremos agregar un objeto en el mismo, aparte de definir la parte visual (dándole una imagen), debemos definir una posición.

Las posiciones son instancias de la clase Position. Cuando colocamos un objeto en el tablero usando game.addVisualIn(obj,pos), estamos creando un nuevo objeto Position que es referenciado por el objeto que agregamos en el tablero (osea, cuando definimos la posición del objeto). 

Cuando usamos, por ejemplo, game.at(3,5), estamos creando un nuevo objeto Position con los valores 3 (para x) y 5 (para y).

Se pueden comparar dos posiciones por igualdad (dos posiciones son iguales si sus valores x e y

También puedo, si quiero, tomar sólo el valor x o el valor y de una posición, usando <code>posicion.x()</code> o <code>posicion.y()</code>.



* position.distance(position)

* Sonidos

* colliders, getObjectsIn, position.allElements()

* onTick, removeTickEvent

* Animacion del personaje (que cambie su orientacion)

* Animacion de objetos

* Generar muros y modificar tamaño del "tablero"

* Generar objetos aleatoriamente en el tablero

* combinaciones de teclas (a lo Street Fighter)

* Barra de status / menu interfaz
