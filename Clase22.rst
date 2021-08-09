.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 22 - PGE 2019
===================
(Fecha: 12 de noviembre)




**Otro ejemplo de callback**

.. code-block:: c++

	#ifndef BOTONES_H
	#define BOTONES_H

	class Boton  {
	public:
	    virtual void click()  {  }
	};

	template < class T > class BotonCallBack : public Boton  {
	private:
	    T * destinatario;
	    void ( T::*callback )( void );
	public:
	    BotonCallBack( T *otro, void ( T::*puntero_funcion )( void ) )
	        : destinatario(otro), callback(puntero_funcion)  {  }
	
	    void click()  {
	        ( destinatario->*callback )();
	    }
	};

	#endif // BOTONES_H

.. code-block:: c++

	#ifndef REPRODUCTOR_H
	#define REPRODUCTOR_H

	#include <QDebug>

	class MP3Player{
	public:
	    void play()  {
	        qDebug() << "Escuchando...";
	    }
	};

	#endif // REPRODUCTOR_H

.. code-block:: c++

	#include <QApplication>
	#include "botones.h"
	#include "reproductor.h"

	int main( int argc, char** argv )  {
	    QApplication a( argc, argv );

	    MP3Player mp3;
	    BotonCallBack< MP3Player > * boton;

	    //Conecta un MP3Player a un botón
	    boton = new BotonCallBack< MP3Player >( &mp3, &MP3Player::play );

	    boton->click();

	    return 0;
	}






Mini Examen 2: LineaDeTexto
============

- `Descargar resolución del Mini Examen 2 <https://github.com/cosimani/Curso-PGE-2019/blob/master/sources/clase19/MiniExamenLineaDeTexto.zip?raw=true>`_





MiniExamen de preguntas múltiples
=================================

:Tarea para Clase 19:
	Completar y estudiar exhaustivamente operador de asignación y constructor copia

	Estudiar eventFilter y LineaDeTexto mejorado

	Ver `Tutorial Qt Creator - qDebug Operador de inserción <https://www.youtube.com/watch?v=IBMU3FyisKY>`_ de `Videos tutoriales de Qt <https://www.youtube.com/playlist?list=PL54fdmMKYUJvn4dAvziRopztp47tBRNum>`_

	Ver `Tutorial Qt Creator - Timer <https://www.youtube.com/watch?v=_Ps7aHDoAr4>`_ de `Videos tutoriales de Qt <https://www.youtube.com/playlist?list=PL54fdmMKYUJvn4dAvziRopztp47tBRNum>`_

	Ver `Tutorial Qt Creator - QTimer <https://www.youtube.com/watch?v=c6JZECBL54Q>`_ de `Videos tutoriales de Qt <https://www.youtube.com/playlist?list=PL54fdmMKYUJvn4dAvziRopztp47tBRNum>`_

	Ver `Tutorial Qt Creator - Archivo de recursos <https://www.youtube.com/watch?v=u8xKE0zHLsE>`_ de `Videos tutoriales de Qt <https://www.youtube.com/playlist?list=PL54fdmMKYUJvn4dAvziRopztp47tBRNum>`_

	Se tomarán preguntas en Opcionables y un MiniExamen para desarrollar en computadora




Ejercicio 24:
============

- Agregar en Login el siguiente método para poder utilizar una base SQLite.

.. code-block:: c++

	bool validarCon( QString archvioSqlite, QString tabla, QString campoUsuario, QString campoClave );

- Dar a este método la funcionalidad que desee (incluir la documentación en el código para que otro programador sepa cómo usar este método).




**Ejemplo de Manager como singleton**

.. code-block:: c++

	#include <QApplication>
	#include "manager.h"

	int main( int argc, char ** argv )
	{
	    QApplication a( argc, argv );

	    Manager::getInstancia()->iniciar();

	    return a.exec();
	}

.. code-block:: c++

	#ifndef MANAGER_H
	#define MANAGER_H

	#include <QObject>
	#include <QVector>

	#include "login.h"
	#include "formulario.h"

	class Manager : public QObject
	{
	    Q_OBJECT

	private:
	    static Manager *instancia;
	    explicit Manager( QObject * parent = 0 );

	public:
	    static Manager *getInstancia();
	    ~Manager();

	    void iniciar();

	private:
	    Login * login;
	    Formulario * formulario;

	private slots:
	    void slot_ingreso( bool valido, QStringList usuario );

	};

	#endif // MANAGER_H

.. code-block:: c++

	#include "manager.h"

	Manager * Manager::instancia = NULL;

	Manager::Manager( QObject * parent ) : QObject( parent ),
	                                       login( new Login ),
	                                       formulario( new Formulario )
	{

	    connect( login, SIGNAL( signal_usuarioValidado( bool, QStringList ) ), 
	             this, SLOT( slot_ingreso( bool, QStringList ) ) );

	    QVector< QStringList > nuevosUsuarios;

	    QStringList usuario1;  usuario1 << "carlos" << "123";
	    QStringList usuario2;  usuario2 << "miguel" << "1234";
	    QStringList usuario3;  usuario3 << "julio" << "12345";

	    nuevosUsuarios << usuario1 << usuario2 << usuario3;

	    login->setBaseUsuarios( nuevosUsuarios );
	}


	Manager * Manager::getInstancia()
	{
	    if( instancia == NULL )
	    {
	        instancia = new Manager();
	    }
	    return instancia;
	}

	Manager::~Manager()
	{
	    if( instancia != NULL )
	    {
	        delete instancia;
	    }
	}

	void Manager::iniciar()
	{
	    login->show();
	}

	void Manager::slot_ingreso( bool valido, QStringList usuario )
	{
	    if ( valido )  {
	        login->hide();
	        formulario->show();
	        formulario->setWindowTitle( "Bienvenido: " + usuario.at( 0 ) );
	    }
	    else  {
	        login->close();
	    }  
	}


Ejercicio 25:
============

- Hacer funcionar este ejemplo con Formulario, Login y Manager como singleton.
- Agregar la característica a Formulario para que se puedan agregar nuevos usuarios a login.
- Al cerrar Formulario, no se cierra el programa sino que vuelve a Login para que pueda usar el usuario nuevo.
- Cuando un usuario se loguea, Login se debe ocultar.



**Ejemplos programados en clase**

- `ManagerSingleton-v1.zip <https://github.com/cosimani/Curso-PGE-2019/blob/master/sources/clase15/ManagerSingleton-v1.zip?raw=true>`_

- `ManagerSingleton-v2.zip <https://github.com/cosimani/Curso-PGE-2019/blob/master/sources/clase15/ManagerSingleton-v2.zip?raw=true>`_

- `ManagerSingleton-v3.zip <https://github.com/cosimani/Curso-PGE-2019/blob/master/sources/clase15/ManagerSingleton-v3.zip?raw=true>`_

- `ManagerSingleton-v4.zip <https://github.com/cosimani/Curso-PGE-2019/blob/master/sources/clase15/ManagerSingleton-v4.zip?raw=true>`_







Utilización de cámaras de video con Qt
======================================

- Clase QCamera: Controlador de las cámaras
- Clase QCameraViewfinder: Es un QWidget visualizador de imágenes de la cámara
- Clase QCameraInfo: Listado de las cámaras disponibles y la info de cada una
- Requiere en el .pro: QT += multimedia multimediawidgets #Qt5.3 mínimo

**Publicar la descripción de las cámaras disponibles**

.. code-block::

	QList< QCameraInfo > cameras = QCameraInfo::availableCameras();
	for ( int i = 0 ; i < cameras.size() ; i++ )  
	    qDebug() << cameras.at( i ).description();

**Instanciar QCamera y mostrar los frames sobre el QCameraViewfinder**

.. code-block:: c++

    QCameraInfo cameraInfo = cameras.at( 0 );
    QCamera * camera = new QCamera( cameraInfo );

    QCameraViewfinder * visor = new QCameraViewfinder;

    camera->setViewfinder( visor );
    camera->start();

    visor->show();

**Creación de un visor promovido a QWidget para QtDesigner**

.. code-block:: c++

	// Puede estar sólo en el .h (en visor.h)
	#ifndef VISOR_H
	#define VISOR_H

	#include <QCameraViewfinder>

	class Visor : public QCameraViewfinder  {
	    Q_OBJECT
	public:
	    explicit Visor( QWidget * parent = 0 ) : QCameraViewfinder( parent )  {   }
	};

	#endif // VISOR_H

Ejercicio 23:
=============

- Crear una aplicación con un QCameraViewfinder promovido a QWidget en QtDesigner
- Un botón "Mostrar imagen" para que encienda la cámara y muestre la imagen
- Que complete un QComboBox con las cámaras disponibles
- Un QPushButton para iniciar la cámara seleccionada

**Resolución**

- `Código fuente <https://github.com/cosimani/Curso-PGE-2019/blob/master/resources/clase13/camera.zip?raw=true>`_

Ejercicio 24:
============

- Siguiendo el ejercicio anterior, crear una carpeta donde se irán guardando las imágenes de la cámara
- Colocar un QSlider con rango entre 500 y 5000, paso de 500, que indica una cantidad en mili segundos
- Descargar en el disco las imágenes en archivos jpg cada un tiempo según el QSlider anterior
- El nombre del archivo tendrá la fecha y hora en que fue capturada

Ejercicio 25:
============

- Siguiendo el ejercicio anterior, usar todas las imágenes de esa carpeta mostrándolas en un QWidget cada 100 mseg.


Análisis píxel a píxel
======================

- **Ejemplo:** Dejar sólo el componente rojo

.. code-block::

    QPixmap pixMap = ui->visor->grab();  // Para llevar el QWidget a QImage
    QImage image = pixMap.toImage();

    for ( int x = 0 ; x < image.width() ; x++ )  {
        for ( int y = 0 ; y < image.height() ; y++ )  {
            QRgb rgb = image.pixel( x, y );  // typedef unsigned int QRgb;
            QRgb nuevoValorRgb = qRgb( qRed( rgb ), 0, 0 );
            image.setPixel( x, y, nuevoValorRgb );
        }
    }



Base de datos con SQLite (repaso)
========================

.. figure:: images/clase09/sqlite1.png

.. figure:: images/clase09/sqlite2.png

.. figure:: images/clase09/sqlite3.png

**Ejercicio**

.. figure:: images/clase09/ejercicio4.png

.. figure:: images/clase09/ejercicio4a.png

.. figure:: images/clase09/ejercicio4b.png

**Para independizar del SO**

.. figure:: images/clase09/independizar.png

**Consulta a la base de datos**

.. figure:: images/clase09/consultar1.png

.. figure:: images/clase09/consultar2.png


Ejercicio 26:
============

- Agregar un QPushButton "Capturar imagen" para procesarla
- Dibujar con paintEvent esa imagen procesada 
- Procesar la imagen con lo siguiente:
	- Invertir los colores rgb a bgr
	- Transformar a escala de grises
		Y = 0.3 R + 0.3 G + 0.3 B // Y en cada componente
	- Convertir al negativo: Cada componente, si lo leemos en binario, debemos invertir cada bit.
		- Ejemplo: R=144=10010000 -> R=01101111


Levantar frame por frame: Clase QAbstractVideoSurface
=====================================================

- QAbstractVideoSurface es una clase abstracta
- Proporciona streaming de video a través de la función virtual pura present()

.. code-block:: c++

	bool QAbstractVideoSurface::present ( const QVideoFrame & frame ) [pure virtual]


**Clase Capturador para obtener los frames de la cámara**

.. code-block:: c++

	class Capturador : public QAbstractVideoSurface  {
	    Q_OBJECT

	public:
	    Capturador( QObject * parent = 0 );

	    QList< QVideoFrame::PixelFormat > supportedPixelFormats(
	          QAbstractVideoBuffer::HandleType handleType = QAbstractVideoBuffer::NoHandle ) const;

	    bool present( const QVideoFrame & frame );

	    QVideoFrame getFrameActual()  {  return frameActual;  }

	private:
	    QVideoFrame frameActual;
	};


- QVideoFrame encapsula los datos de video (bits, ancho, alto, etc.)
- Para acceder a los bits es necesario mapearlo con el método map()
- El mapeo deja en memoria los datos para se accedidos.

.. code-block:: c++

	bool Capturador::present( const QVideoFrame & frame )  {
	    frameActual = frame;
		
	    frameActual.map( QAbstractVideoBuffer::ReadOnly );
		
	    return true;  // Con la idea de devolver true si este frame fue usado
	}


- La función virtual pura supportedPixelFormats() devuelve un listado de formatos soportados.

.. code-block:: c++

	QList< QVideoFrame::PixelFormat > Capturador::supportedPixelFormats(
	                         QAbstractVideoBuffer::HandleType handleType ) const  {
	
	    if ( handleType == QAbstractVideoBuffer::NoHandle ) {
	        return QList< QVideoFrame::PixelFormat >()
	                                                  << QVideoFrame::Format_RGB32
	                                                  << QVideoFrame::Format_ARGB32;
	    }
	    else {
	        return QList< QVideoFrame::PixelFormat >();
	    }
	}

**El constructor**

.. code-block:: c++

	Capturador::Capturador( QObject * parent ) : QAbstractVideoSurface( parent )  {

	}

Ejercicio 27:
============

- Usar Capturador para levantar las imágenes de la cámara.
- Convertir a escala de grises y visualizarlo en pantalla.

Ejercicio 28:
============

.. figure:: images/clase11/ejer2.png

Ejercicio 29:
============

.. figure:: images/clase11/ejer3.png

- En esa grilla de 6 celdas, todas esas celdas son objetos Frame.
- 5 de ellos serán imágenes fijas cargadas desde el disco duro.
- La restante serán las imágenes obtenidas en tiempo real desde la cámara.


Ejercicio 30:
============

.. figure:: images/clase11/ejer4.png




Primer parcial - Entrega de avances del proyecto
================================================

**Características recomendadas para los proyectos**

- Base de datos en SQLite 
- Singleton
- Métodos estáticos
- Programación genérica
- Sobrecarga de operadores
- Método para dimensionar widgets y adaptar resolución de pantalla
- Dedicar mucho tiempo al diseño de la interfaz gráfica




Uso de atributos estáticos
^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: c++

	// Archivo archivador.h
	#ifndef ARCHIVADOR_H
	#define ARCHIVADOR_H

	#include <QFile>
	#include <QTextStream>

	class Archivador  {
	private:
	    static QFile *file;

	public:
	    static bool abrir( QString ruta );
	    static bool almacenar( QString texto );
	};
	
	#endif // ARCHIVADOR_H

	
.. code-block:: c++

	// Archivo archivador.cpp
	#include "archivador.h"

	QFile * Archivador::file = new QFile("./defecto.txt");

	bool Archivador::abrir( QString ruta )  {
	    file->setFileName( ruta );

	    if ( ! file->exists() )  {
	        return false;
	    }

	    return file->open( QIODevice::Append | QIODevice::Text );
	}

	bool Archivador::almacenar( QString texto )  {
	    if ( ! file->isOpen() )
	        return false;

	    QTextStream salida( file );
	    salida << texto;

	    return true;
	}


Ejercicio 24:
============

.. figure:: images/clase13/logger.png





Texturas
========

- Con la texturización agregamos una imagen a un polígono.
- En lugar de ver un color plano o color en degradé, veremos una imagen proyectada.

**Cargando texturas en memoria**

- Las dimensiones de las texturas tienen que ser potencias de 2 (64x64, 128x64, etc.).
- Tendremos un puntero a un segmento de memoria que contiene la imagen:

.. code-block:: c++

	unsigned char *textura;

	QImage im;
	if (!im.load(":/recursos/pared.bmp"))
	    QMessageBox::critical(this, "Recurso no disponible", "No pudo ser cargada.");

	im = QGLWidget::convertToGLFormat( im );
	textura = im.bits();

**Pasando las texturas a OpenGL**

- Cuando le pasamos la textura a OpenGL, este nos devolverá un identificador.
- Cada textura tendrá un identificador propio.
- ¿Cómo obtenemos ese identificador? Creamos una variable para almacenarlo:

.. code-block:: c++

	GLuint idTextura;

- Llamamos a ``glGenTextures(...)`` pasando la cantidad de texturas que queremos generar y un array donde queremos almacenar los identificadores. 
- En este caso, sólo queremos una textura, y por lo tanto no hace falta pasarle un array, sino un puntero a una variable de tipo GLuint.

.. code-block:: c++

	glGenTextures( 1, &idTextura );  // este ultimo parámetro es GLuint *textures

- OpenGL pondrá en ``idTextura`` el valor del identificador. 
- Con ``glBindTexture(...)`` asignamos el valor de idTextura, a una textura de destino. 
- Es decir, activamos la textura asignada a idTextura, y todas las propiedades que modifiquemos, serán modificaciones de esa textura.

.. code-block:: c++

	glBindTexture( GL_TEXTURE_2D, idTextura );

- Ahora lo más importante, pasarle la textura a OpenGL. Con ``glTexImage2D(...)``.

.. code-block:: c++

	glTexImage2D( GL_TEXTURE_2D, 0, 3, anchoTextura, altoTextura, 0, GL_RGB, GL_UNSIGNED_BYTE, textura );

donde:

.. code-block:: c++

	void glTexImage2D(
	    GLenum tipoTextura,  // Ahora es GL_TEXTURE_2D
	    GLint nivelMipMap,  // Nivel de MipMapping. Por ahora ponemos 0
	    GLint formatoInterno,  // Nro de componentes de colorde textura. Si es RGB entonces es 3
	    GLsizei ancho,  // Ancho de la textura. Recordar que es potencia de 2
	    GLsizei alto,  // Alto de la textura. Recordar que es potencia de 2
	    GLint borde,  // Anchura del borde. Debe ser 0
	    GLenum formato,  // Formato de la textura en memoria. Usamos GL_RGBA
	    GLenum tipo,  // Tipo de variable con que almacenamos la textura. Si la 
	                  // almacenamos en unsigned char entonces GL_UNSIGNED_BYTE
	    const GLvoid *pixels  // El puntero a la región de memoria donde se almacena
	);

**Visualización de una textura**

- Texel es la unidad mínima de textura
- Si la textura es de 64 x 64 píxeles y la mostramos completa en una 1024x768.
- OpenGL escalará estos píxeles, de manera que cada píxel de la textura (de ahora en adelante téxel) ocupará 16x12 píxeles en la pantalla.

.. code-block:: c++

	1024 píxeles de ancho / 64 téxeles de ancho = 16;
	768 píxeles de alto / 64 téxeles de alto = 12;

- Lo que veremos serán "cuadrados" de 16x12.
- Poco realista ver una textura *pixelizada*.
- Podemos aplicar filtros.
- El más común es el *filtro lineal*, que hace una interpolación.

.. code-block:: c++

	glTexParameteri( GL_TEXTURE_2D, GL_TEXTURE_MAG_FILTER, GL_LINEAR );
	glTexParameteri( GL_TEXTURE_2D, GL_TEXTURE_MIN_FILTER, GL_LINEAR );

- Con esto estamos parametrizando dos filtros. 
- Uno para cuando la textura se representa más grande de lo que es en realidad.
- Y otro para cuando la textura es más pequeña.
- Le decimos que haga un filtro lineal. 
- También podríamos decirle que no aplique ningún filtro ``(GL_NEAREST)``.

**Renderizando con texturas**

- Ya teniendo las texturas cargadas y ajustadas, dibujamos polígonos con texturas.
- Supongamos dibujar un simple cuadrado con la textura cargada.
- Si lo dibujamos sin textura seria:

.. code-block:: c++

	glBegin ( GL_QUADS );
	    glVertex3i ( -100, -100, -5 );
	    glVertex3i ( -100,  100, -5 );
	    glVertex3i (  100,  100, -5 );
	    glVertex3i (  100, -100, -5 );
	glEnd ();

- Aplicando textura sería:


.. code-block:: c++
	glEnable( GL_TEXTURE_2D );	// Activamos la texturización
	glBindTexture( GL_TEXTURE_2D, idTextura );	// Activamos la textura con idTextura

	glBegin ( GL_QUADS );
	    glTexCoord2f( 0.0f, 0.0f );    glVertex3i ( -100, -100, -5 );
	    glTexCoord2f( 1.0f, 0.0f );    glVertex3i ( -100,  100, -5 );
	    glTexCoord2f( 1.0f, 1.0f );    glVertex3i (  100,  100, -5 );
	    glTexCoord2f( 0.0f, 1.0f );    glVertex3i (  100, -100, -5 );
	glEnd ();

	glDisable( GL_TEXTURE_2D );  // Desactivamos la textura. Para que no intente
	                           // texturizar algo que dibujemos después.


- Con la función ``glTexCoord2f( 0.0f, 0.0f )`` tenemos: Las coordenadas de textura. 

**Ejemplo:**

.. code-block:: c++
	
	// archivo ogl.h
	#ifndef OGL_H
	#define OGL_H

	#include <QGLWidget>

	class QTimer;

	class Ogl : public QGLWidget  {
	    Q_OBJECT

	public:
	    Ogl();
	    void setTimer( int timerIntervalo );

	protected:
	    virtual void initializeGL() = 0;
	    virtual void resizeGL( int width, int height ) = 0;
	    virtual void paintGL() = 0;

	    virtual void keyPressEvent( QKeyEvent * e );

	    virtual void timeout();

	protected slots:
	    virtual void slot_timeout();

	private:
	    QTimer *timer;

	};

	#endif // OGL_H

.. code-block:: c++

	// Archivo ogl.cpp
	#include "ogl.h"

	#include <QTimer>
	#include <QKeyEvent>
	#include <QDebug>

	Ogl::Ogl()  {
	}

	void Ogl::setTimer( int timerIntervalo )  {
	    if( timerIntervalo > 0 )  {
	        timerIntervalo = qMin( 15, timerIntervalo );
	        timer = new QTimer( this );
	        connect( timer, SIGNAL( timeout() ), this, SLOT( slot_timeout() ) );
	        timer->start( timerIntervalo );
	    }
	}

	void Ogl::keyPressEvent( QKeyEvent * e )  {
	    switch( e->key() )  {
	    case Qt::Key_Escape:
	        close();
	    }
	}

	void Ogl::slot_timeout()  {
	    this->timeout();
	}

	void Ogl::timeout()  {
	}

.. code-block:: c++

	// Archivo visual.h
	#include "ogl.h"
	
	class Visual : public Ogl  {
	    Q_OBJECT
	public:
	    Visual();
	protected:
	    void initializeGL();
	    void resizeGL( int ancho, int alto );
	    void paintGL();
	    void timeout();
	private:
	    void cargarTexturas();
	    unsigned char *textura;
	    GLuint idTextura;
	};
 
.. code-block:: c++

	// Archivo visual.cpp
	#include <GL/glu.h>

	Visual::Visual() : Ogl()  {
	}

	void Visual::initializeGL()  {
	    this->cargarTexturas();
	    glEnable( GL_TEXTURE_2D );
	    glShadeModel( GL_SMOOTH );
	    glClearColor( 0.0f, 0.0f, 0.0f, 0.0f );
	    glClearDepth( 1.0f );
	    glEnable( GL_DEPTH_TEST );
	}

	void Visual::resizeGL( int ancho, int alto )  {
	    glViewport( 0, 0, ( GLint )ancho, ( GLint )alto );
	    glMatrixMode( GL_PROJECTION );
	    glLoadIdentity();
	    gluPerspective( 45.0f, ( GLfloat )ancho / ( GLfloat )alto, 1, 100.0f );
	    glMatrixMode( GL_MODELVIEW );
	    glLoadIdentity();
	}

	void Visual::paintGL()  {
	    glClear( GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT );
	    glLoadIdentity();
	    glEnable( GL_TEXTURE_2D );  // Activamos la texturización
	    glBindTexture( GL_TEXTURE_2D, idTextura );  // Activamos la textura con idTextura

	    glBegin( GL_QUADS );
	        glTexCoord2f( 0.0f, 0.0f );  glVertex3f( -2.0f, -2.0f, -8 );
	        glTexCoord2f( 2.0f, 0.0f );  glVertex3f(  2.0f, -2.0f, -8 );
	        glTexCoord2f( 2.0f, 1.0f );  glVertex3f(  2.0f,  2.0f, -8 );
	        glTexCoord2f( 0.0f, 1.0f );  glVertex3f( -2.0f,  2.0f, -8 );
	    glEnd();
	    glDisable( GL_TEXTURE_2D );
	    glFlush();
	}

	void Visual::timeout()  {
	    this->updateGL();
	}

	void Visual::cargarTexturas()   {
	    QImage im;
	    if ( ! im.load( ":/recursos/pared.bmp" ) )
	        QMessageBox::critical( this, "Recurso no disponible", "La imagen no pudo ser cargada." );
	    im = QGLWidget::convertToGLFormat( im );
	    textura = im.bits();

	    glGenTextures( 1, &idTextura );  // Generamos 1 textura. Guardamos su id en idTextura.
	    glBindTexture( GL_TEXTURE_2D, idTextura );  // Activamos idTextura.    
	    glTexParameteri( GL_TEXTURE_2D, GL_TEXTURE_MAG_FILTER, GL_LINEAR ); // GL_LINEAR - Interpolacion
	    glTexParameteri( GL_TEXTURE_2D, GL_TEXTURE_MIN_FILTER, GL_LINEAR ); // GL_NEAREST - Sin 

	    glTexParameteri( GL_TEXTURE_2D, GL_TEXTURE_WRAP_S, GL_REPEAT );  // GL_CLAMP -    
	    glTexParameteri( GL_TEXTURE_2D, GL_TEXTURE_WRAP_T, GL_REPEAT );  // GL_REPEAT - Permite repetir
	    glTexImage2D( GL_TEXTURE_2D, 
	                  0, 3, im.width(), im.height(), 0, 
	                  GL_RGBA, GL_UNSIGNED_BYTE, textura );
	}


- `Descargar el código fuente <https://github.com/cosimani/Curso-PGE-2017/blob/master/sources/clase14/EjemploTexturas.zip?raw=true>`_


Ejercicio 27:
============

- Caminando en la habitación.
- Buscar una imagen de piso de cerámica para texturizar un plano horizontal.
- Con las teclas UP y DOWN realizar el efecto como si estuviéramos desplazándonos sobre la habitación hacia delante y atrás.
- Colocar una pared al fondo de la habitación con textura de ladrillos.

Ejercicio 28:
============

- En un nuevo proyecto promocionar en QtDesigner dos Escenas.
- Como si estuviéramos haciendo un App para la CardBoard.
- Intentar hacer ese pequeño desplazamiento de la imagen para cada ojo.

 



Recordatorio sobre la entrega de ejercicios
===========================================

- Se deberán entregar los ejercicios que están enumerados
- Cada alumno entregará sólo algunos ejercicios asignados según este `Documento en Drive <https://docs.google.com/spreadsheets/d/1zVNfyvZA01IA9ErGCsfEZLRegmMzTmTWH6xRbXhRpP0/edit?usp=sharing>`_
- Después del segundo Mini Examen ya se define el ranking y los ejercicios que cada uno debe entregar.








MiniExamen de preguntas múltiples (que no se tomará este año 2019)
==================================================================

:Tarea para Clase 22 (12 de noviembre):
	Ver `Tutorial Qt Creator - Librería DLL <https://www.youtube.com/watch?v=WSk8ojnCrrI>`_ de `Videos tutoriales de Qt <https://www.youtube.com/playlist?list=PL54fdmMKYUJvn4dAvziRopztp47tBRNum>`_

	Ver `Tutorial Qt Creator - Librería estática <https://www.youtube.com/watch?v=nqS5WNZZnzU>`_ de `Videos tutoriales de Qt <https://www.youtube.com/playlist?list=PL54fdmMKYUJvn4dAvziRopztp47tBRNum>`_

	Ver `Tutorial Qt Creator - QWidget <https://www.youtube.com/watch?v=NpwRtpndqA4>`_ de `Videos tutoriales de Qt <https://www.youtube.com/playlist?list=PL54fdmMKYUJvn4dAvziRopztp47tBRNum>`_

	Ver `Tutorial Qt Creator - Sintaxis alternativa de signals & slots <https://www.youtube.com/watch?v=ARPUSKsU3-U>`_ de `Videos tutoriales de Qt <https://www.youtube.com/playlist?list=PL54fdmMKYUJvn4dAvziRopztp47tBRNum>`_

	Ver `Tutorial Qt Creator - Caso especial de signal & slot <https://www.youtube.com/watch?v=cBcbmRGAktU>`_ de `Videos tutoriales de Qt <https://www.youtube.com/playlist?list=PL54fdmMKYUJvn4dAvziRopztp47tBRNum>`_

	Ver `Tutorial Qt Creator - Slot lambda <https://www.youtube.com/watch?v=XL6OTXEh6P8>`_ de `Videos tutoriales de Qt <https://www.youtube.com/playlist?list=PL54fdmMKYUJvn4dAvziRopztp47tBRNum>`_







MiniExamen de preguntas múltiples (que no se tomará este año 2019)
==================================================================

:Tarea para Clase 23 (14 de noviembre):
	Ver `Tutorial Qt Designer <https://www.youtube.com/watch?v=na0dOHmLFYI>`_ de `Videos tutoriales de Qt <https://www.youtube.com/playlist?list=PL54fdmMKYUJvn4dAvziRopztp47tBRNum>`_
	
	Ver `Tutorial Qt Creator - Icono de la aplicación <https://www.youtube.com/watch?v=eM9ItsibSjc>`_ de `Videos tutoriales de Qt <https://www.youtube.com/playlist?list=PL54fdmMKYUJvn4dAvziRopztp47tBRNum>`_
	
	Ver `Tutorial Qt Creator - QMessageBox <https://www.youtube.com/watch?v=pEjzODGZCxk>`_ de `Videos tutoriales de Qt <https://www.youtube.com/playlist?list=PL54fdmMKYUJvn4dAvziRopztp47tBRNum>`_
	
	Ver `Tutorial Qt Creator - QCompleter <https://www.youtube.com/watch?v=VmDVprlLupo>`_ de `Videos tutoriales de Qt <https://www.youtube.com/playlist?list=PL54fdmMKYUJvn4dAvziRopztp47tBRNum>`_
	
	Ver `Tutorial Qt Creator - QSystemTrayIcon <https://www.youtube.com/watch?v=Fe1L6u064ao>`_ de `Videos tutoriales de Qt <https://www.youtube.com/playlist?list=PL54fdmMKYUJvn4dAvziRopztp47tBRNum>`_
	
	Ver `Tutorial Qt Creator - QMouseEvent <https://www.youtube.com/watch?v=5dI0u84VGoY>`_ de `Videos tutoriales de Qt <https://www.youtube.com/playlist?list=PL54fdmMKYUJvn4dAvziRopztp47tBRNum>`_
	
	Ver `Tutorial Qt Creator - QResizeEvent <https://www.youtube.com/watch?v=2mFuXsgJBoI>`_ de `Videos tutoriales de Qt <https://www.youtube.com/playlist?list=PL54fdmMKYUJvn4dAvziRopztp47tBRNum>`_

	Ver `Tutorial Qt Creator - QKeyEvent <https://www.youtube.com/watch?v=44fCm1KlQGY>`_ de `Videos tutoriales de Qt <https://www.youtube.com/playlist?list=PL54fdmMKYUJvn4dAvziRopztp47tBRNum>`_



Ejercicio 39:
============

- Descargar la escena `Habitación <https://github.com/cosimani/Curso-PGE-2018/blob/master/sources/clase19/Habitacion.zip?raw=true>`_

- Replicar lo que se visualiza en el siguiente video: https://www.youtube.com/watch?v=Jr_luYdSfRE



**Podemos ahora llevar las imágenes de la cámara como textura a OpenGL**

.. code-block:: c++

	class Visual : public Ogl  {
		Q_OBJECT
	public:
		Visual();
		void iniciarCamara();

	protected:
		void initializeGL();
		void resizeGL( int ancho, int alto );
		void paintGL();

	private:
		Capturador * capturador;
		QCamera * camera;

		void cargarTexturas();
		void cargarTexturaCamara();

		unsigned char * texturaCielo;
		unsigned char * texturaMuro;
		GLuint idTextura[ 2 ];

		unsigned char * texturaCamara;
		GLuint idTexturaCamara[ 1 ];
	};

	void Visual::iniciarCamara()  {
		capturador = new Capturador;

		QList<QCameraInfo> cameras = QCameraInfo::availableCameras();

		for ( int i = 0 ; i < cameras.size() ; i++ )  {
			qDebug() << cameras.at( i ).description();

			if ( cameras.at( i ).description().contains( "Truevision", Qt::CaseInsensitive ) )  {
				camera = new QCamera( cameras.at( i ) );
				camera->setViewfinder( capturador );
				camera->start(); // to start the viewfinder
			}
		}

		glGenTextures( 1, idTexturaCamara );
	}

	void Visual::cargarTexturaCamara()  {

		QVideoFrame frameActual = capturador->getFrameActual();
		texturaCamara = frameActual.bits();

		glBindTexture( GL_TEXTURE_2D, idTexturaCamara[ 0 ] );  // Activamos idTextura.
		glTexParameteri( GL_TEXTURE_2D, GL_TEXTURE_MAG_FILTER, GL_LINEAR ); 
		glTexParameteri( GL_TEXTURE_2D, GL_TEXTURE_MIN_FILTER, GL_LINEAR ); 

		glTexImage2D( GL_TEXTURE_2D, 
		              0, 
		              3, 
		              frameActual.width(), 
		              frameActual.height(), 
		              0, 
		              GL_BGRA, 
		              GL_UNSIGNED_BYTE, 
		              texturaCamara );
	}

**Ejercicio:**

- Crear una escena con OpenGL con glOrtho para mostrar como textura las imágenes de la cámara en un QUADS.
- Luego probar con gluPerspective

**Resolución**

- Descargar `Código fuente <https://github.com/cosimani/Curso-PGE-2018/blob/master/sources/clase19/camaraOgl.zip?raw=true>`_

Ejercicio 40:
============

- Dentro de la habitación elegir una pared para colocar un monitor LCD con las imágenes de la cámara.

Ejercicio 41:
============

- En el ejercicio de la Habitación, mejorar los movimientos que se realizan con el mouse.

Ejercicio 42:
============

- Con la barra espaciadora se deberá saltar dentro de la escena.


Cálculo de la tercer nota
^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: c++

	Nota final = Promedio de (
	                           Cuestionario 1
	                           Cuestionario 2
	                           Mini Examen 1
	                           Mini Examen 2
	                           Promedio de ( Ejercicios 1 2 y 3 )
	                           Promedio de ( Ejercicios 4 5 y 6 )
	                           Promedio de ( Ejercicios 7 8 y 9 )
	                           Promedio de ( Ejercicios 10 11 y 12 )
	                         )




