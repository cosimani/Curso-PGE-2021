.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 09 - PGE 2020
===================
(Fecha: 24 de septiembre)



**Una opción para la creación de la clase LineaDeTexto:**

.. code-block::

	#include <QApplication>
	#include <QLineEdit>
	#include <QString>

	class LineaDeTexto : public QLineEdit  {
	    // Q_OBJECT 
	    // Si usamos Q_OBJECT sin separar la definicion de esta clase en su .h y .cpp puede no compilar
	    // Recordar que sin el Q_OBJECT no podremos definir signals ni slots en esta clase

	public:
	    LineaDeTexto( QString texto = "" ) : QLineEdit( texto )  {  }

	    // El constructor copia debe invocar explicitamente al constructor de 
	    // la clase base para que el compilador no tire un warning
	    LineaDeTexto( const LineaDeTexto & linea ) : QLineEdit()  {
	        this->setText( linea.text() );
	    }

	    LineaDeTexto& operator=( const LineaDeTexto & linea )  {
	        this->setText( linea.text() );
	        return *this;
	    }

	    LineaDeTexto operator+( const LineaDeTexto & linea )  {
	        return LineaDeTexto( this->text() + linea.text() );
	    }
	};

	int main( int argc, char *argv[] )  {
	    QApplication a( argc, argv );
	    LineaDeTexto linea1( "Hola " );
	    LineaDeTexto linea2( "che" );
	    LineaDeTexto total;

	    total = linea1 + linea2;
	    total.show();

	    return a.exec();
	}




Captura de eventos con eventFilter
==================================

- En un QWidget dibujar una imagen de 50x50.
- Con un QTimer hacer rotar la imagen cada 20 mseg.
- Permitir desplazar la imagen con las teclas y usando eventFilter.
- Utilizar los métodos rotate y translate de QPainter.

.. code-block::

	#include <QWidget>

	class QTimer;
	class QImage;

	namespace Ui  {
	    class Principal;
	}

	class Principal : public QWidget  {
	    Q_OBJECT

	public:
	    explicit Principal( QWidget * parent = 0 );
	    ~Principal();

	private:
	    Ui::Principal *ui;
	    QTimer* timer;
	    int rotacion, transX, transY;
	    QImage * image;

	protected:
	    void paintEvent( QPaintEvent * e );
	    bool eventFilter( QObject *, QEvent * );

	private slots:
	    void slot_ciclos();
	};


	#include "principal.h"
	#include "ui_principal.h"
	#include <QTimer>
	#include <QPainter>
	#include <QKeyEvent>
	#include <QImage>



	Principal::Principal( QWidget * parent ) : QWidget( parent ), 
	                                           ui( new Ui::Principal ),
	                                           rotacion( 0 ), 
	                                           transX( 0 ), 
	                                           transY( 0 )
	{
	    ui->setupUi( this );
	    this->installEventFilter( this );
	    image = new QImage( ":/User.png" );
	    timer = new QTimer;
	    timer->setInterval( 20 );
	    connect( timer, SIGNAL( timeout() ), SLOT( slot_ciclos() ) );
	    timer->start();
	}

	Principal::~Principal()  {
	    delete ui;
	}

	void Principal::paintEvent( QPaintEvent * e )  {
	    QPainter* p = new QPainter( this );
	    p->translate( this->width() / 2 + transX, this->height() / 2 + transY );
	    p->rotate( 10*++rotacion );
	    p->drawImage( 0, 0, *image );
	    if ( rotacion >= 360 )
	        rotacion = 0;
	}

	void Principal::slot_ciclos()  {
	    this->repaint();
	}

	bool Principal::eventFilter( QObject * obj, QEvent * e )  {
	    if ( obj == this )  {
	        if ( e->type() == QEvent::KeyPress )  {
	            int key = static_cast< QKeyEvent * >( e )->key();

	            switch( key )  {

	            case Qt::Key_Up:
	                transY -= 10;
	                break;
	            case Qt::Key_Down:
	                transY += 10;
	                break;
	            case Qt::Key_Left:
	                transX -= 10;
	                break;
	            case Qt::Key_Right:
	                transX += 10;
	                break;
	            case Qt::Key_Escape:
	                this->close();
	                break;
	            }

	            // Devolvemos true para indicar que este evento ya lo controlamos aquí
	            // y no es necesario que se propague para que alguien más lo controle.
	            return true;  
	        }
	    }

	    // Esto es para que la clase base decida si necesita controlar los eventos
	    // que no estamos capturando nosotros.
	    return QWidget::eventFilter( obj, e );
	}


Ejercicio 11:
============

- Modificar este ejemplo para que la imagen rote sobre su centro.
- Cada vez que el usuario desplace la ventana Principal, que al soltarlo se coloque centrado en pantalla.



Clase QCompleter
================

- Permite completar palabras que está escribiendo el usuario en un QLineEdit
- Se basa en un listado de palabras:

.. code-block::

	QCompleter( const QStringList & list, QObject * parente = nullptr );


.. code-block::

	QStringList lista;
	lista << "Jorge << "Miguel" << "Marcos" << "Luis";

	QCompleter * completer = new QCompleter( lista, this );
	completer->setCaseSensitivity( Qt::CaseInsensitive );
	ui->leUsuario->setCompleter( completer );



Ejercicio 12:
============

- Una universidad perdió su servidor de base de datos y sólo tiene backup en papel.
- Crear un formulario para que un DataEntry pueda cargar los datos.
- Los datos serán: nombre, apellido, legajo y carrera
- Permitir facilidad y velocidad de uso.
- Autompletar lo que sea necesario.
- Como base de datos utilice un QVector< Alumno >

Ejercicio 13:
============

- Hacer lo mismo pero que la base de datos sea un archivo SQLite.


Resolución de una parte del MiniExamen 1
============


.. code-block::

	#include <QVector>

	template< class T > class Inventario : private QVector< T >  {
	public:
	    Inventario()  {  }
	    void add( T elemento )  {  this->push_front( elemento );  }
	    T get( int posicion )  {  return this->at( posicion );  }
	    int cantidad()  {  return this->size();  }

	    void operator-( int nroRemover )  {
	        nroRemover = qMin( nroRemover, this->cantidad() );
	        this->remove( this->size() - nroRemover, nroRemover );
	    }
	};


.. code-block::
	
	#include <QApplication>
	#include <QDebug>
	#include "inventario.h"

	int main( int argc, char ** argv )  {
	    QApplication a( argc, argv );
	    Q_UNUSED( a );

	    Inventario< int > inventario;
	    inventario.add( 1 );
	    inventario.add( 2 );
	    inventario.add( 3 );
	    inventario.add( 4 );
	    inventario.add( 5 );

	    qDebug() << "Cantidad =" << inventario.cantidad();
	    qDebug() << "Elemento 3 =" << inventario.get( 3 );

	    qDebug() << "----------------";
	    for ( int i = 0 ; i < inventario.cantidad() ; i++ )
	        qDebug() << "i =" << i << ":: elemento =" << inventario.get( i );

	    inventario - 3;

	    qDebug() << "----------------";
	    for ( int i = 0 ; i < inventario.cantidad() ; i++ )
	        qDebug() << "i =" << i << ":: elemento =" << inventario.get( i );

	    inventario - 999;

	    qDebug() << "----------------";
	    for ( int i = 0 ; i < inventario.cantidad() ; i++ )
	        qDebug() << "i =" << i << ":: elemento =" << inventario.get( i );

	    return 0;
	}
