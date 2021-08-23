.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 06 - PGE 2021
===================
(Fecha: 26 de agosto)
	
Diversas maneras de usar la Herencia con clases genéricas
=========================================================

.. code-block::

    template< typename T > class ListaGenerica : public Listado< T >  {
 
        //////////

    };

.. code-block::

    class ListaDeEnteros : public Listado< int >  {
 
        //////////

    };

.. code-block::

    template< class T > class ListadoStd : public std::vector< T >  {
 
        //////////

    };

.. code-block::

    template< class T > class ListadoQt : public QVector< T >  {
 
        //////////

    };

.. code-block::

    class ListaDePersonas : public QVector< Persona >  {
 
        //////////

    };

.. code-block::

    template< class T > class DerivadaDeCualquiera : public T  {
 
        //////////

    };


Ejemplo 1
=========

- Es posible también que una clase derive de una u otra según se requiera.

.. code-block::

	#include <QString>
	#include <QDebug>
	#include <typeinfo>

	class Real  {
	private:
    	    int colores;

	public:
    	    Real( int colores ) : colores( colores )  {  }
     	    int getDato()  {  return colores;  }
	};


	class Virtual {
	private:
    	    int bits;

	public:
    	    Virtual( int bits ) : bits( bits )  {  }
    	    int getDato()  {  return bits;  }
	};

	template< class T > class Mundo : public T  {
	private:
    	    QString nombre;

	public:
    	    Mundo( QString nombre, int dato ) : T( dato ), nombre( nombre )  {  }

    	    QString toString()  {
        	QString descripcion = "El mundo " + nombre + " es de ";
        	descripcion.append( QString::number( T::getDato() ) );

        	if ( typeid( T ) == typeid( Real ) )
            	    descripcion.append( " colores." );
        	if ( typeid( T ) == typeid( Virtual ) )
            	    descripcion.append( " bits." );

        	return descripcion;
    	    }
    	};

	int main( int, char ** )  {
    	    Mundo< Real > mundo1( "Tierra", 10000 );
    	    Mundo< Virtual > * mundo2 = new Mundo< Virtual >( "StarCraft", 64 );

    	    qDebug() << mundo1.toString();
    	    qDebug() << mundo2->toString();

	    return 0;
	}



Ejemplo 2
=========

- Las Personas se pueden identificar con lo que sea.

.. code-block:: c

	template< class T > class Persona  {
	private:
	    T id;
	    int edad;

	public:
	    Persona( T id ) : id( id ), edad( 0 )  {
	    }

	    T getId()  {
	        return id;
	    }
	};

.. code-block:: c

	class Cliente : public Persona< int >  {
	private:
	    int cantDolares;

	public:
	    Cliente() : Persona( 1001 ), cantDolares( 10 )  {
	    }
	};

	// Se puede instanciar con:    Cliente cliente;


.. code-block:: c

	template< class T > class Cliente : public Persona< T >  {
	private:
	    int cantPesos;

	public:
	    Cliente( T id ) : Persona< T >( id ), cantDolares( 600 )  {
	    }
	};

	// Se puede instanciar con:    Cliente< QString > cliente( "Algun nombre" );


.. code-block:: c

	struct Credencial  {
	    int dni;
	    QString nombre;
	};

	int main( int argc, char ** argv )  {
	    Persona< int > juan( 36242 );

	    Persona< QString > carlos( "Carlos" );	 
	    
	    Credencial credencial1;
	    credencial1.dni = 44123456;
	    credencial1.nombre = "Lucas";

	    Persona< Credencial > lucas( credencial1 );	 

	    return 0;
	}


Entregable Clase 06
===================

- Punto de partida: Utilizar el código fuente del proyecto Ejemplo 2.
- Sobrecargar en Persona el ``operator++`` de tal manera permita incrementar en uno su edad.
- Buscar la solución a este problema y explicar el resultado en el video. No es necesario desarrollar la solución en el video.
- Explicar los inconvenientes que tuvo para encontrar la solución y comente también con quiénes trabajó, si fue en Discord, o si encontró la solución fácilmente en internet.
- En la función main utilizar este operador. Debería utilizarse de la siguiente manera: ``carlos++;``.
- Entrar al siguiente `link para ver el registro de los entregables <https://docs.google.com/spreadsheets/d/1xbj6brqzdn3R9sfjDEP0LEjg6CwMNMOb8dBEYGmxhTw/edit?usp=sharing>`_ 
- El link de Youtube se comparte con el docente por mensaje privado de Teams.
- `Mesas de trabajo en Discord <https://discord.gg/TFKzMXrNCV>`_ 
