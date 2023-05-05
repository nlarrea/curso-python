# ALIEN INVASION

<br><hr>
<hr><br>

# Antes de comenzar

En esta ocasión vamos a crear el más que conocido *'juego de los alienígenas'*.

Antes de comenzar a descargar paquetes, se va a hacer uso de un entorno virtual mediante `pipenv` para evitar posibles problemas posteriores, así como la instalación de *demasiados* recursos.

<br>

Por ello, en primer lugar y desde el directorio actual en la terminal, se ejecuta lo siguiente:

```bash
pipenv --python 3.10.7
```

<br>

De esta forma, se trabajará con esa versión de Python y se instalarán aquí únicamente aquellos paquetes necesarios para llevar a cabo este proyecto.


<br><hr><br>


## Descripción del proyecto

El proyecto consiste en crear un videojuego. En él, el usuario controlará una nave espacial que se moverá de forma horizontal en la parte inferior de la pantalla haciendo uso de las flechas de dirección. A su vez, al pusar la tecla de espacio, la nave disparará balas.

Cuando el juego comienza, una flota alienígena que se va moviendo por la pantalla aparece. El jugador debe disparar para destruir a los alienígenas.

Si el jugador destruye a todos los enemigos, una nueva flota aparece, una que se mueva más rápido. Si algún alien toca al jugador o llega a la parte inferior de la pantalla, el jugador pierde una nave. Si el jugador pierde tres naves, el juego termina.


<br><hr><br>


## Intalación de Pygame

Antes de comenzar a crear el programa, es necesario instalar `Pygame`, un paquete que permitirá trabajar de forma mucho más sencilla a lo largo de este proyecto.

Como hemos decidido usar `pipenv`, debemos ejecutar el siguiente comando desde el directorio de trabajo:

```bash
pipenv install pygame
```

<br>

> Si no se estuviera usando `pipenv`:
>
> ```bash
> python -m pip install --user pygame
> ```


<br><hr>
<hr><br>


# Creación del proyecto

Vamos a comenzar a crear el programa creando una ventana vacía de Pygame. Después, dibujaremos los elementos (*las naves, los aliens, etc.*).

Además, también será necesario crear código para poder leer la entrada del usuario.


<br><hr><br>


## Crear la ventana de Pygame y responder a la entrada de usuario

Vamos a crear el archivo del proyecto (`alien_invasion.py`), y en ella vamos a crear el siguiente código:

```python
# alien_invasion.py

import sys
import pygame

class AlienInvasion:
    """ Overall class to manage game assets and behavior. """
    
    def __init__(self):
        """ Initialize the game, and create game resources. """
        pygame.init()

        self.screen = pygame.display.set_mode((1200, 800))
        pygame.display.set_caption("Alien Invasion")

    def run_game(self):
        """ Start the main loop for the game. """
        while True:
            # Watch for keyboard and mouse events.
            for event in pygame.event.get():
                if event.type == pygame.QUIT:
                    sys.exit()
            
            # Make the most recently drawn screen visible
            pygame.display.flip()

if __name__ == '__main__':
    # Make a game instance, and run the game
    ai = AlienInvasion()
    ai.run_game()
```

<br>

En primer lugar, se importan los módulos `sys` y `pygame`. El módulo `sys` se usará para salir del juego cuando el usuario haga clic en el botón de cerrar. El módulo `pygame` contiene las funcionalidades necesarias para crear un juego.

<br>

A continuación, se crea la clase `AlienInvasion`. Esta clase representa el juego en sí. En el método `__init__()` se inicializan los recursos del juego. En este caso, se inicializa el fondo de la pantalla y se crea una instancia de `pygame.display`.

<br>

El método `run_game()` contiene un bucle `while` que contiene un bucle `for` para detectar los eventos del teclado y del ratón. El bucle `while` se ejecuta continuamente para mantener la ventana del juego abierta. El bucle `for` se ejecuta cada vez que el usuario realiza una acción, como pulsar una tecla o mover el ratón. El bucle `for` contiene una serie de declaraciones `if` para detectar eventos específicos y realizar las acciones apropiadas, en nuestro caso, por ahora solo hemos creado una acción: salir del juego.

La llamada a `pygame.display.flip()` actualiza la pantalla con cada ejecución del bucle `while` y dibuja una pantalla vacía en cada pasada del bucle, borrando la pantalla anterior para mostrar la nueva.

<br>

Por último, se crea una instancia de `AlienInvasion` y se llama al método `run_game()` para ejecutar el juego.


<br><hr><br>


## Crear un fondo para el juego

Por defecto, Pygame genera una pantalla de color negro. Vamos a configurar esto para colocar el color que deseemos, lo haremos dentro del método `__init__()`:

```python
# alien_invasion.py

import sys
import pygame

class AlienInvasion:
    """ Overall class to manage game assets and behavior. """
    
    def __init__(self):
        """ Initialize the game, and create game resources. """
        pygame.init()

        self.screen = pygame.display.set_mode((1200, 800))
        pygame.display.set_caption("Alien Invasion")

        # Set the background color.
        self.bg_color = (230, 230, 230)

    def run_game(self):
        """ Start the main loop for the game. """
        while True:
            # Watch for keyboard and mouse events.
            for event in pygame.event.get():
                if event.type == pygame.QUIT:
                    sys.exit()

            # Redraw the screen during each pass through the loop.
            self.screen.fill(self.bg_color)
            
            # Make the most recently drawn screen visible
            pygame.display.flip()

if __name__ == '__main__':
    # Make a game instance, and run the game
    ai = AlienInvasion()
    ai.run_game()
```

<br>

Los colores en Pygame se especifican en formato RGB. En este caso, se ha elegido un gris claro para el fondo, concretamente el color `(230, 230, 230)`.

Se ha asignado este color a `self.bg_color` y se ha usado para llenar la pantalla en cada pasada del bucle `while`.


<br><hr><br>


## Crear una clase 'Settings'

Cada vez que introduzcamos nuevas funcionalidades al programa, vamos a necesitar generar una serie de configuraciones.

Con el objetivo principal de mantener dichas configuraciones en un único lugar, vamos a crear una clase llamada `Settings` dentro de un nuevo archivo, al que llamaremos `settings.py`. De esta forma, si necesitamos cambiar alguna configuración, solo tendremos que modificar el archivo `settings.py` en lugar de tener que buscar en todo el código.

<br>

He aquí el código de la clase `Settings`:

```python
# settings.py

class Settings:
    """ A class to store all settings for Alien Invasion. """

    def __init__(self):
        """ Initialize the game's settings. """
        # screen settings
        self.screen_width = 1200
        self.screen_height = 700
        self.bg_color = (230, 230, 230)
```

<br>

Ahora, para crear una instancia de la clase `Settings` y usarla en `alien_invasion.py`, debemos importarla y modificar el código del archivo de tal forma que las configuraciones especificadas en la clase sustituyan a las que teníamos anteriormente:

```python
# alien_invasion.py

import sys
import pygame

from settings import Settings

class AlienInvasion:
    """ Overall class to manage game assets and behavior. """
    
    def __init__(self):
        """ Initialize the game, and create game resources. """
        pygame.init()
        self.settings = Settings()

        self.screen = pygame.display.set_mode(
            (self.settings.screen_width, self.settings.screen_height))
        pygame.display.set_caption("Alien Invasion")

    def run_game(self):
        """ Start the main loop for the game. """
        while True:
            # Watch for keyboard and mouse events.
            for event in pygame.event.get():
                if event.type == pygame.QUIT:
                    sys.exit()

            # Redraw the screen during each pass through the loop.
            self.screen.fill(self.settings.bg_color)
            
            # Make the most recently drawn screen visible
            pygame.display.flip()

if __name__ == '__main__':
    # Make a game instance, and run the game
    ai = AlienInvasion()
    ai.run_game()
```

<br>

Los cambios realizados son los siguientes:

* Se ha importado la clase `Settings` desde el módulo `settings`.
* Se ha creado una instancia de `Settings` en `__init__()`.
* Se han sustituido los valores hardcodeados en `pygame.display.set_mode()` por las variables `self.settings.screen_width` y `self.settings.screen_height`.
* Se ha eliminado el color de fondo definido en `__init__()` y se ha utilizado el valor de `self.settings.bg_color`.

<br>

Si ejecutamos los cambios (*recordatorio: estamos usando `pipenv`*), veremos que el juego sigue funcionando exactamente igual que antes, pero ahora tenemos una clase `Settings` que podemos usar para almacenar todas las configuraciones del juego.


<br><hr><br>


## Crear la nave espacial

### Seleccionar una imagen

Se pueden utilizar prácticamente imágenes de cualquier formato, sin embargo, el formato más sencillo con el que trabajar en este caso será con los archivos bitmap (`.bmp`), puesto que Pygame carga las imágenes en este formato por defecto.

<br>

Vamos a acceder a cualquier página que nos permita descargar imágenes para utilizarlas en nuestros proyectos o a crear la nuestra propia.

En este caso, la imagen seleccionada para utilizarla como nave espacial en nuestro videojuego es la siguiente:

![ship](./images/ship.bmp)


<br><br>


### Crear una clase 'Ship'

Después de seleccionar la nave, tenemos que hacer que aparezca en la pantalla. Para ello, vamos a crear una clase en otro archivo llamado `ship.py`, donde definiremos todos los aspectos de la nave espacial que manejará el jugador.

He aquí el código escrito en dicho archivo:

```python
# ship.py

import pygame

class Ship:
    """ A class to manage the ship. """

    def __init__(self, ai_game):
        """ Initialize the ship and set its starting position. """
        self.screen = ai_game.screen
        self.screen_rect = ai_game.screen.get_rect()

        # Load the ship image and get its rect
        self.image = pygame.image.load('./images/')
        self.rect = self.image.get_rect()
        # Start each new ship at the botom center of the screen
        self.rect.midbottom = self.screen_rect.midbottom

    def blitme(self):
        """ Draw the ship at its current location. """
        self.screen.blit(self.image, self.rect)
```

<br>

Una gran ventaja de Pygame es que permite tratar a todos los elementos del juego como si fueran rectángulos (`rect`), lo que facilita muchísimo la geometría.

<br>

En primer lugar, se importa la librería correspondiente (`pygame`) para poder trabajar, y, a continuación, se genera la clase `Ship` que definirá nuestra nave espacial.

El método `__init__()` recibe dos parámetros:

* La referencia `self`
* La referencia a la instancia actual de la clase `AlienInvasion`.

<br>

Con el objetivo de facilitar el acceso a la pantalla del juego, creamos un atributo `self.screen`. Como vamos a trabajar con *rectángulos*, obtenemos el rectángulo de la ventana de juego con `ai_game.screen.get_rect()`.

Para cargar la imagen de la nave usamos `pygame.image.load()`, a continuación, usamos `get_rect()` para acceder al atributo `rect` de la imagen y, finalmente, establecemos la posición de la nave en la parte inferior central de la pantalla.

<br>

Finalmente, creamos el método `blitme()` que dibuja la imagen de la nave en la pantalla en la posición especificada por `self.rect`.


<br><br>


### Mostrar la nave en la pantalla

Para mostrar la nave en la pantalla, tenemos que modificar el archivo `alien_invasion.py` de la siguiente forma:

```python
# alien_invasion.py

import sys
import pygame

from settings import Settings
from ship import Ship

class AlienInvasion:
    """ Overall class to manage game assets and behavior. """
    
    def __init__(self):
        """ Initialize the game, and create game resources. """
        pygame.init()
        self.settings = Settings()

        self.screen = pygame.display.set_mode(
            (self.settings.screen_width, self.settings.screen_height))
        pygame.display.set_caption("Alien Invasion")

        self.ship = Ship(self)

    def run_game(self):
        """ Start the main loop for the game. """
        while True:
            # Watch for keyboard and mouse events.
            for event in pygame.event.get():
                if event.type == pygame.QUIT:
                    sys.exit()

            # Redraw the screen during each pass through the loop.
            self.screen.fill(self.settings.bg_color)
            self.ship.blitme()  # to draw the ship
            
            # Make the most recently drawn screen visible
            pygame.display.flip()

if __name__ == '__main__':
    # Make a game instance, and run the game
    ai = AlienInvasion()
    ai.run_game()
```

<br>

Los cambios realizados son los siguientes:

* Se ha importado la clase `Ship` desde el módulo `ship`.
* Se ha creado una instancia de `Ship` en `__init__()`.
* Se ha añadido el método `self.ship.blitme()` en el bucle `while` para dibujar la nave en la pantalla.


<br><hr><br>


## Refactorizar métodos: check_events() y update_screen()
