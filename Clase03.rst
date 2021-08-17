.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 03 - PGE 2020
===================
(Fecha: 1 de septiembre)


**La clase Listado quedó así:**

.. code-block:: c

	#ifndef LISTADO_H
	#define LISTADO_H

	template < class T > class Listado  {
	private:
	    int cantidad;
	    T * v;
	    int libre;

	public:
	    Listado( int cantidad = 10 ) : cantidad( cantidad ), v( new T[ cantidad ] ), libre( 0 )  {  }

	    T get( int i )  {  return v[ i ];  }
	    bool add( T contenido );
	    int getCantidad()  {  return this->cantidad;  }
	    int size()  {  return libre;  }
	};


	template < class T > bool Listado< T >::add( T contenido )  {
	    if ( cantidad <= libre )
	        return false;

	    v[ libre ] = contenido;
	    libre++;
	    return true;
	}

	#endif // LISTADO_H

**¿Qué otros métodos sería oportuno agregar?**

- Método que elimine todos los elementos, que vacíe el Listado

.. code-block::

	void clear()

- Método que elimine un elemento del final.

.. code-block::
	
	void pop_back()
	
- Método que elimine el elemento de la posición i.

.. code-block::
	
	void erase( int i )

- Método que inserte un elemento en la posición i desplazando los otros

.. code-block::

	bool insert( int i, T elemento )	

- Modificar listado.h para que todos sus métodos queden definidos de manera off-line


Herencia con clases genéricas
=============================

.. code-block::

    template< class T > class Lista : public Listado< T >  {
 
        //////////

    };


- Tener presente que si heredamos de una clase genérica ``QVector`` o ``std::vector`` ya no es necesario definir las características de almacenamiento en la clase derivada. Es decir, ya no debemos definir ``libre``, ``T * v``, ``cantidad``, ``get``, ``add`` o ``size``.

Entregable Clase 03
===================

- Punto de partida: Utilizar el código fuente del proyecto que se creó durante esta clase que tiene definidos los métodos nuevos.
- Agregar los siguientes dos métodos: ``borrar_del_final( int cuantos )`` y ``borrar_del_principio( int cuantos )``. 
- Tener en cuenta que tenemos ya definidos métodos que borran elementos, entonces, utilizarlos para ahorrar tiempo de desarrollo.
- En la función main crear un ``Listado< str::string >`` y agregar 8 cadenas
- Borrar 2 elemnetos del final y borar 2 elementos del principio
- Recorrer el Listado con un for y mostrar los elementos que quedan
- Explicar a medida que se vaya haciendo el ejercicio
- Entrar al siguiente `link para ver el registro de los entregables <https://docs.google.com/spreadsheets/d/1xbj6brqzdn3R9sfjDEP0LEjg6CwMNMOb8dBEYGmxhTw/edit?usp=sharing>`_ 
- El link de Youtube se comparte con el docente por mensaje privado de Teams.
- `Mesas de trabajo en Discord <https://discord.gg/TFKzMXrNCV>`_ 
