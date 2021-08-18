.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 06 - PGE 2020
===================
(Fecha: 10 de septiembre)
	



Ejercicio 2:
============

- Sobrecargar el ``operador++`` para que duplique la cantidad máxima de elementos que puede contener el Listado y que también duplique los elementos que ya existían. 


Ejercicio 3:
============

- Sobrecargar el ``operador+`` para que al recibir un nuevo elemento, que inserte una nueva celda (incrementando en 1 la cantidad máxima de elementos que puede contener) y que agregue ese nuevo elemento en la útima celda vacía.


Herencia con clases genéricas
=============================

.. code-block::

    template< typename T > class Lista : public Listado< T >  {
 
        //////////

    };

.. code-block::

    class ListaDeEnteros : public Listado< int >  {
 
        //////////

    };


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

Ejercicio 4:
============

- Agregar la características a Mundo para que pueda heredar de cualquier clase, no sólo de Real y Virtual
- Que compile y deje ejecutar la aplicación pero el toString publicar que el Mundo creado no es posible.

Ejemplos:
=========

.. code-block::

	class Cliente : public Persona< int >  {
	private:
	    int cantDolares;

	public:
	    Cliente() : Persona( 1001 ), cantDolares( 10 )  {
	    }
	};

	// Se puede instanciar con:    Cliente cliente;


.. code-block::

	template< class T > class Cliente : public Persona< T >  {
	private:
	    int cantPesos;

	public:
	    Cliente( T id ) : Persona< T >( id ), cantDolares( 600 )  {
	    }
	};

	// Se puede instanciar con:    Cliente< QString > cliente( "Algun nombre" );

.. code-block::

	struct Credencial  {
	    int dni;
	    QString nombre;
	};

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

	int main( int argc, char ** argv )  {
	    Persona< int > juan( 36242 );

	    Persona< QString > carlos( "Carlos" );	 
	    
	    Credencial credencial1;
	    credencial1.dni = 44123456;
	    credencial1.nombre = "Lucas";

	    Persona< Credencial > lucas( credencial1 );	 

	    return 0;
	}


static
======

**Variables estáticas**

- Al salir de su ámbito no pierde su valor
- Sólo son conocidas dentro de su ámbito (pero igual "recuerdan su valor")
- Se inicializan sólo la primera vez

.. code-block:: c

	#include <QApplication>
	#include <QDebug>

	int funcion( int a = 2 )  {
	    static int suma = 0;
	    return ( suma += a );
	}

	int main( int argc, char ** argv )  {
	    QApplication a( argc, argv );

	    qDebug() << funcion();	    // 2
	    qDebug() << funcion( 10 );	// 12
	    qDebug() << funcion();	    // 14

	    return 0;
	}

**Miembros estáticos**

- Para cada instancia de una clase existe una copia de los miembros no-estáticos.
- Pero hay una única copia de los estáticos para todas las instancias.
- Pueden ser accedidas sin referencia a ninguna instancia concreta de la clase.
- Los miembros estáticos no dependen de ninguna instancia para su existencia.
- Existen incluso antes que la primera instancia de una clase.

**¿Qué problema tiene este código?**

.. code-block:: c

	#include <QApplication>
	#include <QDebug>

	class A  {
	public:
	    static int x;
	};

	int main( int argc, char ** argv )  {
	    QApplication a( argc, argv );

	    A a1;
	    qDebug() << a1.x;		// No reconoce x

	    return 0;
	}

**¿Qué se publica?**

.. code-block:: c

	#include <QApplication>
	#include <QDebug>

	class A  {
	public:
	    static int x;
	};

	int A::x = 5;

	int main( int argc, char ** argv )  {
	    QApplication a( argc, argv );

	    A a1, a2;
	    qDebug() << a1.x;		// 5
	    qDebug() << a2.x;		// 5

	    a1.x = 9;
	    qDebug() << a1.x;		// 9
	    qDebug() << a2.x;		// 9

	    return 0;
	}

- La modificación del valor x en el objeto a1 cambia dicha propiedad x en a2.
- La definición int A::x = 5; solo son permitidas para miembros estáticos.

**¿Qué error tiene el siguiente código?**

.. code-block:: c

	class B  {
	    static const char* p1;        // privado por defecto

	public:
	    static const char* p2;        // declaración
	    const char* p3;
	};

	const char* B::p1 = "Adios";     // Ok.  Definición
	const char* B::p2 = "mundo";     // Ok
	const char* B::p3 = "cruel";     // Error. No es estática. No se puede definir así.


- No significa que las propiedades estáticas (privadas o protegidas) puedan ser accedidas directamente desde el exterior. Depende del modificador de acceso:

.. code-block:: c

	int main( int argc, char ** argv ) {
	    QApplication a( argc, argv );

	    qDebug() << B::p1;    // Error: no accesible!
	    qDebug() << B::p2;    // Ok: -> "mundo"

	    return 0;
	}

**Definición de miembros estáticos**

- Si los miembros estáticos existen antes de cualquier instancia, entonces hay que definirlos. 
- Los métodos estáticos sólo pueden acceder a miembros estáticos.

**¿Qué problema tiene el siguiente código?**

.. code-block:: c

	class C  {
	    static int y;

	public: 
	    int x;
	    static int* p;
	    static const char* c;
	    static int getY()  {  return y;  }
	    static int getX()  {  return x;  }	// No compila. x no es estático.
	};

	int C::y = 1;          		// no se debe poner static
	int* C::p = &C::y;     		
	const char* C::c = "ABC";   

**El constructor y miembros estáticos**

- La inclusión de un constructor no evita tener que definir los miembros estáticos.
- Recordar que el constructor es invocado cuando se instancia.
- El constructor puede modificar los valores de los miembros estáticos pero no inicializarlos.

**¿El siguiente código compila?**

.. code-block:: c

	class D  {
	    static int y;

	public: 
	    int x;

	    // El constructor no puede modificar así los miembros estáticos
	    D() : y( 10 ), x( 20 )  {  }  
	};

	int D::y = 1;

- Se debería usar un constructor como el que sigue:

.. code-block:: c

	D() : x( 20 )  {
	    y = 10;
	}

**Particularidades de la notación**

- Los miembros estáticos pueden ser accedidos con :: con la notación C::miembro.
- No es necesario utilizar ninguna instancia concreta de la clase.

**¿Qué publicaría el siguiente código?**

.. code-block:: c

	#include <QApplication>
	#include <QDebug>

	class E  {
	public:
	    static int x;      // miembro estático
	    E( int i = 12 )  {  x = i;  }   

	};

	int E::x = 13;          // definición de miembro

	int main( int argc, char ** argv )  {
	    QApplication( argc, argv );

	    qDebug() << E::x;   // 13
	    E e1;
	    qDebug() << E::x;   // 12

	    return 0;
	}


Ejercicio 5:
============

- Utilizar la misma clase Poste para sobrecargar operator+ para que sume un objeto Poste con un int.


- Esa sobrecarga nos permitirá hacer Poste suma = p1 + 5;


- Qué pasa si queremos		 Poste suma = 5 + p1;
- Debemos hacerlo sobrecargando el operador global.


**Sobrecarga de operadores globales**

.. figure:: images/clase03/operadores_globales.png


Ejercicio 6:
============

- Utilice la clase genérica Listado dentro de un nuevo proyecto.
- En la función main crear un listado con 5 QWidget
- Al iniciar, usar un for para extraerlos y mostrarlos como ventanas independientes.
- Prestar atención en el problema, y trate de hacerlo funcionar.




Ejercicio 7:
============

- Definir una clase genérica ``MiVector`` que herede de ``QVector< T >`` y que permita ordenar de menor a mayor cualquier elemento.
- Considere que para ordenar de menor a mayor algún elemento se necesitará comparar un elemento con otro.
- Por lo tanto, si deseamos que ``MiVector`` pueda contener elementos de cualquier tipo, y además deseamos ordenarlos, entonces estos elementos deberan cumplir la caraccterística de ser "comparables". Esto es muy familiar cuando se trabaja en Java.
- Definir clases comparables eligiendo la/las característica/s que permitan comparar dichos objetos entre ellos.
- Entre las clases que se pueden definir como comparables están: ``Persona``, ``Poste``, ``Cliente``, ...
- Probar el funcionamiento del ordenamiento utilizando ``MiVector< Persona >``, ``MiVector< Poste >``, ``MiVector< int >``.
- Mostrar los resultados en un ``QTextEdit`` en lugar de visualizarlo directamente por consola.
- ¿Qué pasaría con el ordenamiento si se utilizan punteros como elementos, por ejemplo, ``MiVector< Persona * >``, ``MiVector<Poste*>``.


**Copiar objetos**

- El operador = supone la asignación del operando de la derecha en el izquierdo
- Si ``p1`` y ``p2`` son objetos de la clase ``Persona``

.. code-block:: c
	
	p1 = p2;  // Esta asignación copia los valores de p2 en p1

- El operador = supone la asignación miembro a miembro
- En este caso, el compilador define automáticamente el ``operator=``
- Realiza la asignación miembro a miembro
- Por otro lado, cuando se crea un nuevo objeto

.. code-block:: c
	
	Persona p1 = p2; 

- No implica ninguna asignación sino que se invoca a un constructor especial (constructor copia).
- Es equivalente a:

.. code-block:: c
	
	Persona p1( p2 );


**Constructor copia**

.. figure:: images/clase04/constructor_copia.png

**Operador de asignación**

.. figure:: images/clase04/operador_asignacion.png


