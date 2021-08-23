.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 05 - PGE 2021
===================
(Fecha: 24 de agosto)


**Sobrecargando operadores en la clase genérica Listado**

- Modificar la clase Listado sobrecargando ``operator+`` de tal forma que al sumar dos listados se obtenga un nuevo objeto Listado con los elementos consecutivos y la cantidad de celdas disponibles acumuladas.

.. code-block:: c

	template < class T > class Listado  {
	private:
	    int maxSize;
	    T * v;
	    int libre;

	public:
	    Listado( int maxSize = 10 );

	    T get( int i ) const;
	    int getMaxSize() const;
	    int size() const;

	    bool add( T contenido );

	    void clear();
	    bool pop_back();
	    bool erase( int i );
	    bool insert( int i, T contenido );

	    Listado< T > operator+( const Listado< T > otro );
	};


	template < class T > Listado< T >::Listado( int maxSize ) : maxSize( maxSize ),
	                                                            v( new T[ maxSize ] ),
	                                                            libre( 0 )
	{

	}

	template < class T > int Listado< T >::getMaxSize() const {
	    return this->maxSize;
	}

	template < class T > int Listado< T >::size() const {
	    return libre;
	}

	template < class T > T Listado< T >::get( int i ) const {
	    return v[ i ];
	}

	template < class T > bool Listado< T >::add( T contenido )  {
	    if ( maxSize <= libre )
	        return false;

	    v[ libre ] = contenido;
	    libre++;
	    return true;
	}

	template< class T > Listado< T > Listado< T >::operator+( const Listado< T > otro )  {
		T vAux[ this->size() + otro.size() ];

		int contador = 0;

		for ( ; contador < this->size() ; contador++ )
			vAux[ contador ] = this->get( contador );

		for ( int i = 0 ; contador < ( this->size() + otro.size() ) ; contador++, i++ )
			vAux[ contador ] = otro.get( i );

		Listado< T > res( this->getMaxSize() + otro.getMaxSize() );

		for ( int j = 0 ; j < contador ; j++ )
			res.add( vAux[ j ] );

		return res;
	}




Entregable Clase 05
===================

- Punto de partida: Utilizar el código fuente del proyecto que se entrega durante esta clase que tiene definido el ``operator+``.
- Sobrecargar el ``void operator+( int cuantasNuevasCeldas )`` de tal manera permita agregar nuevas celdas vacías al final del Listado. Este operador no deberá modificar el contenido que ya tenga el Listado.
- También completar el código con las definiciones de los métodos ``clear``, ``pop_back``, ``erase`` e ``insert`` que ya hemos trabajado anteriormente.
- En esta oportunidad se pide respetar el punto de partida. Es decir, resolver el ejercicio previamente para luego poder grabar el video con el desarrollo de la solución. Igualmente si prefiere grabar un video con toda la búsqueda de la solución, no hay problema, no importa el tiempo de duración del video. (Recordar que debe tener verificado el canal de Youtube para poder subir videos largos).
- En la función main crear un ``Listado< QString >`` para 5 elementos como máximo y agregar 3 cadenas.
- Utilizar el operador definido en este entregable para aumentar a 10 la cantidad de celdas disponibles.
- Agregar 3 cadenas nuevas al Listado y por último recorrerlo con un for mostrando los elementos.
- Explicar a medida que se vaya haciendo el ejercicio
- Entrar al siguiente `link para ver el registro de los entregables <https://docs.google.com/spreadsheets/d/1xbj6brqzdn3R9sfjDEP0LEjg6CwMNMOb8dBEYGmxhTw/edit?usp=sharing>`_ 
- El link de Youtube se comparte con el docente por mensaje privado de Teams.
- `Mesas de trabajo en Discord <https://discord.gg/TFKzMXrNCV>`_ 


