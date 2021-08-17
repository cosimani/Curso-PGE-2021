.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 04 - PGE 2020
===================
(Fecha: 19 de agosto)


Sobrecarga de operadores 
========================

- Supongamos los siguientes objetos de la clase Poste:

.. code-block:: c

	Poste p1;  // Su único miembro dato es un float para la altura del Poste
	Poste p2;

- Necesitamos unir estos Postes para obtener un único Poste con sus alturas sumadas.
- ¿Podemos hacer lo siguiente?

.. code-block:: c

	Poste unidos = p1 + p2;

**Otro ejemplo**

.. code-block:: c

	class Cliente  {
	private:
	    int saldo;

	public:
	    Cliente() : saldo( 0 )  {
	    }

	    void operator+( int sumando )  {
	        this->saldo += sumando;
	    }

	    void operator-( int sustraendo )  {
	        this->saldo -= sustraendo;
	    }

	    bool operator<( Cliente otroCliente )  {
	        if ( this->saldo < otroCliente.saldo )
	            return true;
	        return false;
	    }
	};

	int main( int argc, char ** argv )  {
	    Cliente juan;

	    Cliente carlos;

	    juan + 50;  // Suma 50 a su cuenta

	    carlos + 100;  // Quita 100 a carlos

	    if ( juan < carlos )  {
	        qDebug() << "juan tiene menos";
	    }

	    return 0;
	}



**Sobrecargando operadores en la clase genérica Listado**

- Modificar la clase Listado sobrecargando ``operator+`` de tal forma que al sumar dos listados se obtenga un nuevo objeto Listado con los elementos consecutivos.

.. code-block:: c

	template < class T > class Listado  {
	private:
	    int cantidad;
	    T * v;
	    int libre;

	public:
	    Listado( int cantidad = 10 ) : cantidad( cantidad ), v( new T[ cantidad ] ), libre( 0 )  {  }

	    T get( int i ) const {  return v[ i ];  }
	    bool add( T contenido );
	    int getCantidad() const {  return this->cantidad;  }
	    int size() const {  return libre;  }
	    Listado< T > operator+( const Listado< T > otro );
	};

	template < class T > bool Listado< T >::add( T contenido )  {
	    if ( cantidad <= libre )
	        return false;

	    v[ libre ] = contenido;
	    libre++;
	    return true;
	}

	template< class T > Listado< T > Listado< T >::operator+( const Listado< T > otro )  {
		T vAux[ this->length() + otro.length() ];

		int contador = 0;

		for ( ; contador < this->length() ; contador++ )
			vAux[ contador ] = this->get( contador );

		for ( int i = 0 ; contador < ( this->length() + otro.length() ) ; contador++, i++ )
			vAux[ contador ] = otro.get( i );

		Listado< T > res( this->length() + otro.length() );

		for ( int j = 0 ; j < contador ; j++ )
			res.add( vAux[ j ] );

		return res;
	}


Entregable Clase 04
===================

- Punto de partida: Utilizar el código fuente del proyecto que se creó durante esta clase que tiene definido el ``operator+``.
- Sobrecargar el ``void operator+( int cuantasNuevasCeldas )`` de tal manera permita agregar nuevas celdas vacías al final del Listado. Este operador no deberá modificar el contenido que ya tenga el Listado.
- En la función main crear un ``Listado< QString >`` para 5 elementos como máximo y agregar 3 cadenas.
- Utilizar el operador definido en este entregable para aumentar a 10 la cantidad de celdas disponibles.
- Agregar 3 cadenas nuevas al Listado y por último recorrerlo con un for mostrando los elementos.
- Explicar a medida que se vaya haciendo el ejercicio
- Entrar al siguiente `link para ver el registro de los entregables <https://docs.google.com/spreadsheets/d/1xbj6brqzdn3R9sfjDEP0LEjg6CwMNMOb8dBEYGmxhTw/edit?usp=sharing>`_ 
- El link de Youtube se comparte con el docente por mensaje privado de Teams.
- `Mesas de trabajo en Discord <https://discord.gg/TFKzMXrNCV>`_ 
