.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 22 - PGE 2021
===================
(Fecha: 2 de noviembre)


Función callback
^^^^^^^^^^^^^^^^

- Función que se llama a través de un puntero a función.
- Se puede utilizar como parámetro de otra función.
- Cuando la función que recibe este puntero a función hace uso de este, se dice que hace una retrollamada (callback).
- Si en la clase Listado deseamos que se ordenen los datos pero no queremos incluir (en Listado) la lógica de un método de ordenamiento, podemos pedir al programador que nos pase como parámetro un puntero a su propia función de ordenamiento.
- Se podría utilizar para una simple notificación o comunicación de dos vías (similar a las signals y slots).
- Cuando un diseñador de bibliotecas quiere notificar al programador sobre algún suceso, puede solicitar un puntero a función.

**Declaraciones de punteros a funciones:**

.. code-block:: c++

	void ( * fptr )();  
	// puntero a una función sin parámetros que devuelve void.

	void ( * fptr )( int );	
	// puntero a función que recibe int y devuelve void.

	int ( * fptr )( int, char );		
	// acepta int y char y devuelve un int.

	int * ( * fptr )();	
	// puntero a función, sin argumentos y devuelve puntero a int


**Declaraciones de punteros a funciones (o métodos) de clases:**

.. code-block:: c++

	void ( C::*puntero )( int );  // puntero a método de la clase C

	int ( C::*puntero )();

- Antes de usar un puntero a función es necesario definirlo (asignarle un valor).
- El valor es la dirección de memoria donde inicia una función concreta.

.. code-block:: c++

	char funcion( int );  // Declara una función concreta

	char ( * puntero_funcion )( int );  // Declara un puntero a función

	puntero_funcion = &funcion;  // Asigna al puntero la dirección de memoria de funcion(int)


**Luego de declarado y definido, podemos usarlo de dos formas:**

- Acceder (invocar), a la función que representa
- Usarlo como parámetro de otra función.

**Invocación**

.. code-block:: c++

	char funcion( int );  // Declara función concreta. Suponemos que está definida en otro lugar.

	char ( * puntero_funcion )( int );  // Declaramos puntero a función

	puntero_funcion = &funcion;  // Asigna la dirección de memoria

	int i = 10;
	char c;

	c = ( * puntero_funcion )( i );

**Ejemplo**

.. code-block:: c++

	#include <iostream>

	void funcion() {  std::cout << "Una funcion cualquiera" << std::endl; }
	void ( * puntero_funcion )() = &funcion; 

	int main ()  {      
	    funcion();     
	    ( * puntero_funcion )(); 
	    puntero_funcion();   

	    return 0;
	}

	// Salida:
	// Una funcion cualquiera
	// Una funcion cualquiera
	// Una funcion cualquiera

**Paso de funciones como argumento**

.. code-block:: c++

	void funcion( void ( * puntero_funcion )() ) {  
	    // Código de este método

	    ( * puntero_funcion )();  // Llama a la función apuntada
	}

Ejercicio:
==========

- Definir la siguiente clase:

.. code-block:: c++

	class Ordenador  {
	public:
	    void burbuja( int * v, int n )  {  /* código */  }
	    void insercion( int * v, int n )  {  /* código */  }
	    void seleccion( int * v, int n )  {  /* código */  }
	};

- Esta clase tendrá distintos métodos de ordenamiento.
- Cada método ordena un array de n cantidad de enteros
- Definir la clase ListaDeEnteros
	- Herede de QVector
	- Que no sea un template
	- Que sólo mantenga elementos del tipo int
	- Definir un método:
	
.. code-block:: c++	
		
	void ordenar( void ( Ordenador::*puntero_funcion )( int * v, int n ) );
	// Este método ordenará los elementos


**Array de punteros a función**

- Los punteros a funciones se pueden agrupar en arreglos

.. code-block:: c++	

	int ( * afptr[ 10 ] )( int );    // array de 10 punteros a función

- Los 10 punteros apuntan a funciones con el mismo prototipo
- Permiten muchas variantes para invocar funciones

.. code-block:: c++	

	int a = afptr[ n ]( x );
	
Ejercicio:
==========

- Con la misma idea del ejercicio anterior. Crear la clase genérica ListadoGenerico que herede de QVector<T>
- La clase ListaGenerico tendrá el siguiente método:
	
.. code-block:: c++	
		
	void ordenar( void ( Ordenador::*puntero_funcion )( T * v, int n ) )
	// Este método ordenará los elementos
	
- Notar que ordenar podrá ordenar elementos de cualquier tipo, siempre y cuando los objetos a ordenar sean de una clase que tenga sobrecargado el operador >
	

Ejercicio:
==========

- Modificar el ejercicio de la clase ListadoEnteros para usar funciones globales de ordenamiento, es decir, que no se encuentren dentro de Ordenador ni de ninguna clase.

.. code-block:: c++	

	class ListadoEnteros : public QVector<int>  {
	public:
	    void ordenar( void ( * pFuncionOrdenamiento )( int *, int ) )  {
	        ( *pFuncionOrdenamiento )( this->data(), this->size() );
	    }
	};

.. code-block:: c++		
	///// Desde main se puede utilizar así:

    void ( * ordenador )( int *, int ) = &burbuja;

    listado.ordenar( ordenador );

Ejercicio:
==========

- Modificar el ejercicio anterior usando también funciones globales de ordenamiento pero con la clase ListadoGenerico que sea un template:

.. code-block:: c++	

	template< class T > class ListadoGenerico : public QVector< T >  {
	public:
	    void ordenar( void ( * pFuncionOrdenamiento )( T *, int ) )  {
	        ( * pFuncionOrdenamiento )( this->data(), this->size() );
	    }
	};

Ejercicio:
==========

- Necesitamos conocer el rendimiento de cada algoritmo de ordenamiento midiendo su tiempo.
- Utilizar un array de punteros a funciones que apunte a cada función global de ordenamiento.
- Utilizar Archivador para almacenar los tiempos en un archivo.
- Utilizar un ListadoEnteros de 50.000 números generados con qrand()

.. code-block:: c++		

	///// Desde main se puede utilizar así:

    void ( * ordenador[ 2 ] )( int *, int );
    ordenador[ 0 ] = &burbuja;
    ordenador[ 1 ] = &insercion;

    listado.ordenar( ordenador[ 1 ] );

**Ejemplo del uso de callback en el mecanismo de SIGNAL y SLOT de Qt**

.. code-block:: c++

	#ifndef CONTADOR_H
	#define CONTADOR_H

	#include "ventana.h"
	#include <QDebug>
	#include <QThread>

	class Contador : public QThread  {

	public:
	    Contador() : contador( 0 ),
	                 hastaCuanto( 0 ),
	                 isRunning( false),
	                 puntero( nullptr ),
	                 ventana( nullptr )
	    {

	    }

	void setInterval( unsigned int hastaCuanto )  {
	    this->hastaCuanto = hastaCuanto;
	}

	void run()  {
	    if ( ! puntero || ! hastaCuanto )
	        return;

	    isRunning = true;

	    while( isRunning )  {
	        while ( contador < hastaCuanto )
	            contador-=-1;

	        contador = 0;
	        ( ventana->*puntero )();  // Esto es emitir la signal
	    }
	}

	void conectar( Ventana * ventana, void ( Ventana::*puntero )() )  {
	    this->puntero = puntero;
	    this->ventana = ventana;
	}

	void stop()  {
	    isRunning = false;
	}

	private:
	    unsigned int contador;
	    unsigned int hastaCuanto;
	    bool isRunning;
	    void ( Ventana::*puntero )();
	    Ventana * ventana;
	};

	#endif // CONTADOR_H

.. code-block:: c++

	#ifndef VENTANA_H
	#define VENTANA_H

	#include <QWidget>

	class Contador;

	class Ventana : public QWidget  {
	    Q_OBJECT

	public:
	    Ventana( QWidget * parent = nullptr );
	    ~Ventana();

	    void slot_sinSerSlot();

	private:
	    Contador * contador;
	};

	#endif // VENTANA_H

.. code-block:: c++

	#include "ventana.h"
	#include "contador.h"
	#include <QDebug>

	Ventana::Ventana( QWidget * parent ) : QWidget( parent ),
	                                       contador( new Contador )
	{
	    // Con setInterval se define hasta que numero debera contar 
	    // para realizar la retrollamada (o devolucion de llamada)
	    contador->setInterval( ( unsigned int )500000000 );

	    // Para conectar se puede definir un puntero a funcion y apuntarlo al metodo
	    //    void ( Ventana::*puntero )() = &Ventana::slot_sinSerSlot;
	    //    contador->conectar( this, puntero );

	    // O se puede apuntar al metodo sin declarar un puntero a funcion
	    contador->conectar( this, &Ventana::slot_sinSerSlot );

	    // También las siguientes expresiones son equivalentes:
	    //    connect( sender, SIGNAL( valueChanged( QString, QString ) ), 
	    //             receiver, SLOT( updateValue( QString ) ) );
	    //
	    //    connect( sender, &Sender::valueChanged, 
	    //             receiver, &Receiver::updateValue );

	    contador->start();
	}

	Ventana::~Ventana()  {
	    contador->stop();
	}


	void Ventana::slot_sinSerSlot()  {
	    qDebug() << "timeout";

	    // Tener en cuenta que Contador tiene un metodo stop para finalizar el contador
	    //    contador->stop();
	}

.. code-block:: c++

	#include "ventana.h"
	#include <QApplication>

	int main( int argc, char ** argv )  {
	    QApplication a( argc, argv );

	    Ventana w;
	    w.show();

	    return a.exec();
	}



Entregable Clase 22
===================

- Punto de partida: Proyecto vacío y realizar alguno de los ejercicios de esta clase.
- Entrar al siguiente `link para ver el registro de los entregables <https://docs.google.com/spreadsheets/d/1xbj6brqzdn3R9sfjDEP0LEjg6CwMNMOb8dBEYGmxhTw/edit?usp=sharing>`_ 
- El link de Youtube se comparte con el docente por mensaje privado de Teams.
- En caso de requerir más tiempo para la entrega, escribir por WhatsApp al docente antes de medianoche.









