.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 04 - PGE 2021
===================
(Fecha: 19 de agosto)


Herencia con clases genéricas
=============================

.. code-block::

    template< class T > class Lista : public Listado< T >  {
 
        //////////

    };

- Tener presente que si heredamos de una clase genérica ``QVector`` o ``std::vector`` ya no es necesario definir las características de almacenamiento en la clase derivada. Es decir, ya no debemos definir ``libre``, ``T * v``, ``cantidad``, ``get``, ``add`` o ``size``.


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


Entregable Clase 04
===================

- Punto de partida: Utilizar el código fuente del proyecto que se creó durante esta clase que tiene definida la clase Cliente y la clase Poste.
- Definir estos nuevos operadores en la clase Poste: ``float operator+( Poste poste )`` y ``Poste operator+( float altura )``.
- El primer operador suma la altura de los dos Postes y devuelve la altura total.
- El segundo operador le suma una altura particular a un Poste y devuelve un nuevo Poste con esa altura sumada.
- En la función main probar estos operadores.

- Explicar a medida que se vaya haciendo el ejercicio
- Entrar al siguiente `link para ver el registro de los entregables <https://docs.google.com/spreadsheets/d/1xbj6brqzdn3R9sfjDEP0LEjg6CwMNMOb8dBEYGmxhTw/edit?usp=sharing>`_ 
- El link de Youtube se comparte con el docente por mensaje privado de Teams.
- `Mesas de trabajo en Discord <https://discord.gg/TFKzMXrNCV>`_ 
