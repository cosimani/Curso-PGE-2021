.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 03 - PGE 2020
===================
(Fecha: 1 de septiembre)




Ejercicio 1:
============

- En un archivo de cabecera definir la clase Listado con todos sus métodos off-line
- Agregar un método que inserte un elemento en la posición i desplazando los otros

.. code-block::

	bool insert( int I, T elemento )

- Agregar método que elimine todos los elementos

.. code-block::

	void clear()

- Método que elimine una cantidad x de elementos. Los últimos o los primeros según el bool.

.. code-block::
	
	void erase( int x, bool front_or_back )
	
- Método que elimine el elemento de la posición i.

.. code-block::
	
	void borrar( int i )





Herencia con clases genéricas
=============================

.. code-block::

    template< class T > class Lista : public Listado< T >  {
 
        //////////

    };


Sobrecarga de operadores 
========================

.. figure:: images/clase02/sobrecarga_operadores.png


Ejemplo:
========

.. code-block::

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



Ejemplo:
========

- Modificar la clase genérica Listado sobrecargando ``operator+`` de tal forma que al sumar dos listados se obtenga un nuevo objeto Listado con los elementos consecutivos.

.. code-block::

	template< class T > class Listado  {
	public:
		Listado( int n = 10 );
		bool add( T nuevo );
		T get( int i ) const;
		int length() const;
		Listado< T > operator+( const Listado< T > otro );

	private:
		int cantidad;
		int libre;
		T *v;
	};


	template< class T > Listado< T >::Listado( int n ) : cantidad( n ), libre( 0 ), v( new T[ n ] )  {  

	}

	template< class T > bool Listado< T >::add( T nuevo )  {
		if ( libre < cantidad )  {
			v[ libre ] = nuevo;
			libre++;
			return true;
		}
		return false;
	}


	template< class T > T Listado< T >::get( int i ) const  {  return v[ i ];  }

	template< class T > int Listado< T >::length() const  {  return libre;  }

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


Ejercicio 2:
============

- Sobrecargar el ``operador++`` para que duplique la cantidad máxima de elementos que puede contener el Listado y que también duplique los elementos que ya existían. 


Ejercicio 3:
============

- Sobrecargar el ``operador+`` para que al recibir un nuevo elemento, que inserte una nueva celda (incrementando en 1 la cantidad máxima de elementos que puede contener) y que agregue ese nuevo elemento en la útima celda vacía.


