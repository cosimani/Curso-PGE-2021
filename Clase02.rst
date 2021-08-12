.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 02 - PGE 2021
===================
(Fecha: 12 de agosto)


Introducción
============

**Programación Genérica**: Generalizar las funciones para que puedan ser utilizadas en varios casos.

Ventajas:
    - Reutilización de código.
    - Fácil mantenimiento de código.
    - Nos concentramos más en la lógica del sistema.

Desventajas:
    - Pérdida de interés para los amantes de la programación a bajo nivel.
    - En C++ requiere el uso de Templates y sobrecarga de operadores, que es dificultoso y poco legible.

**Programación Orientada a Eventos**: La ejecución está determinada por los sucesos que ocurran.
    - Generalmente el usuario es el que dirige la ejecución del programa.
    - Básicamente el programa queda sin actividad hasta producirse un evento.
    - Es la base de la interfaz de usuario.

*Ventajas*:
    - Mejoras en las interfaces de usuario.
    - Uso del mouse (o sea, hace tiempo estamos orientados a eventos)

*Desventajas*:
    - El hilo de ejecución se pierde de vista.
    - Es un tanto abstracto (abstracto se refiere al ¿qué hace? y no tanto en ¿cómo lo hace?, como si fuera una caja negra), se maneja generalmente a alto nivel. 
    - Complicado para manejar los eventos a bajo nivel.
    
Plantillas
==========
- Separa la estructura del contenido.
- Permite construir un diseño predefinido
- Facilita el trabajo de realizar copias idénticas de la estructura.

- Utilización de tipos como parámetros
- Teniendo la función ``ordena( v )``. Dependerá del tipo de v para generar la función.

.. code-block::

    template< class T > void ordena( T v[] )  {
    
    }

- Mecanismo que permite usar un tipo como parámetro en una clase o función.
- Clases genéricas: Es un “constructor” (o creador) de clases (no confundir con el constructor de una clase).
- Para el diseño de una clase genérica es aconsejable ir de lo particular a lo general.
- Primero diseñar y depurar una clase referido a un tipo concreto.
- Libro: El lenguaje de programación C++ de Stroustrup - 13.1 - 13.2 - 13.2.1 - 13.2.2


Clase genérica Listado
======================

- Una plantilla genera la definición de una clase. 
- A la instancia concreta de la clase generada, se la denomina especialización.

- La definición de la clase genérica Listado es la siguiente:

.. code-block::

    template< class T > class Listado  {
    private:
        int cantidad;
        int libre;
        T * v;
    
    public:
        Listado( int n = 10 ) : cantidad( n ), libre( 0 ), v( new T[ n ] )  {  }
        bool add( T nuevo );
        T get( int i )  {  return v[ i ];  }
        int length()  {  return libre;  }
    };
    
    template< class T > bool Listado< T >::add( T nuevo )  {
        if ( libre < cantidad )  {
            v[ libre ] = nuevo;
            libre++;
            return true;
        }
        return false;
    }


- Observar que la definición de ``add()`` se realiza off-line con la sintaxis de una función genérica.

- Miembros de clases genéricas definidas off-line deben ser declaradas como funciones genéricas.

.. code-block::

    template< class T > bool Listado< T >::add( T nuevo )  {

        ////////////

    }


Entregable Clase 02
===================

- Punto de partida: Other Project -> Empty qmake Project
- Copiar y pegar este código de la clase Listado en un archivo de cabecera listado.h
- Se recomienda que la definición de clases genéricas se realice en un único archivo .h (no usar el .cpp)
- Dentro de la función main en el archivo main.cpp instanciar un objeto Listado que contenga int
- Agregar 5 datos del tipo int en ese objeto Listado
- Recorrerlo con un for mostrando en consola estos 5 valores
- Explicar a medida que se vaya haciendo el ejercicio
- Entrar al siguiente `link para ver el registro de los entregables <https://docs.google.com/spreadsheets/d/1xbj6brqzdn3R9sfjDEP0LEjg6CwMNMOb8dBEYGmxhTw/edit?usp=sharing>`_ 
- El link de Youtube se comparte con el docente or mensjae privado de Teams.
