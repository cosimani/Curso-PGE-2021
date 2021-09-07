.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 09 - PGE 2021
===================
(Fecha: 7 de septiembre)

Instalación de Python
=====================

:Python: 
	- Descargamos e instalamos `Python 3.8.10 <https://www.python.org/downloads/release/python-3810/>`_ 
	- Elegir instalador para Windows 64 bits (archivo python-3.8.10-amd64.exe)
	- Realizamos una instalación customizada (Install for all users - Create shortcuts for installed applications - Add Python to environment variables - Precompile standard library - pip - C:\\Program Files\\Python38) 
	- Verificamos la instalación de Python ejecutando desde consola ``python --version``
	- Verificamos la instalación de PIP ejecutando desde consola ``python -m pip --version``
	- En linux ``sudo apt-get install python3.8 python3-pip``

Creación de entorno virtual
===========================

.. code-block:: bash 

	# Lo siguiente se ejecuta desde consola, por ejemplo en C:\Users\Usuario>

	pip install virtualenv==20.4.6  # Instala la herramienta para generar entornos virtuales

	pip freeze  # Muestra el listado de paquetes instalados

	# Para crear un entorno virtual, creamos una carpeta donde se estarán todos los entornos virtuales.
	# Creamos por ejemplo la carpeta -> C:\Cosas\2021\PGE2021\EntornosVirtuales

	cd C:\Cosas\2021\PGE2021\EntornosVirtuales  # Accedemos a la carpeta

	virtualenv QtCamera  # Creamos un entorno virtual llamado QtCamera

	.\QtCamera\Scripts\activate  # Activamos el entorno virtual

	# El comando anterior nos lleva al entorno virtual -> (QtCamera) C:\Cosas\2021\PGE2021\EntornosVirtuales>
	# También podemos ver que se creó un directorio nuevo -> C:\Cosas\2021\PGE2021\EntornosVirtuales\QtCamera 

	pip freeze  # Ejecutamos esto dentro del entorno virtual para ver los paquetes instalados

	pip install numpy  # Instalamos numpy
	pip install matplotlib  # Instalamos matplotlib
	pip install numpy==1.19.5  # Instalamos numpy en su versión 1.19.5

	deactivate  # Desactivamos el entorno virtual 
	
	# Para borrar el entorno virtual hay que borrar la carpeta donde se creó
	# -> C:\Cosas\2021\PGE2021\EntornosVirtuales\QtCamera 

	# Si desea actualizar la versión de la herramienta pip, ejecutar lo siguiente:
	python.exe -m pip install --upgrade pip

	# Las bibliotecas que utilizaremos serán las siguientes:

	pip install numpy
	pip install matplotlib
	pip install opencv-python
	pip install pyside6


Aplicación para visualizar las imágenes de la cámara
====================================================


ventana.py
^^^^^^^^^^

.. code-block:: python

	import cv2 

	from PySide6.QtCore import *
	from PySide6.QtGui import QImage, QPixmap
	from PySide6.QtWidgets import QLabel

	class Visor( QLabel ) :

	    def __init__( self ) :
	        super( Visor, self ).__init__()

	        self.videoCapture = cv2.VideoCapture( 0 )

	        self.timer = QTimer()

	        QObject.connect( self.timer, SIGNAL( "timeout()" ), self.slot_procesar )
	        self.timer.start( 10 )
	        
	    @Slot()
	    def slot_procesar( self ) :

	        if self.videoCapture.isOpened() :

	            success, frame = self.videoCapture.read()

	            if success != True :
	                return

	            h, w, ch = frame.shape
	            bytesPerLine = ch * w
	            
	            frame = cv2.cvtColor( cv2.flip( frame, 1 ), cv2.COLOR_BGR2RGB )

	            convertToQtFormat = QImage( frame.data, w, h, bytesPerLine, QImage.Format_RGB888 )
	            im = convertToQtFormat.scaled( self.width(), self.height() )

	            im = im.convertToFormat( QImage.Format_ARGB32 )

	            alpha = QImage( im.width(), im.height(), QImage.Format_Alpha8 )
	            alpha.fill( 210 )
	            im.setAlphaChannel( alpha );

	            pixmap = QPixmap.fromImage( im )
	            self.setPixmap( pixmap );

	    def detener( self ) : 
	        self.timer.stop()
	        self.videoCapture.release()

visor.py
^^^^^^^^

.. code-block:: python

	import sys

	from PySide6.QtCore import QObject
	from PySide6.QtGui import *
	from PySide6.QtWidgets import QWidget, QApplication, QSizePolicy, QGridLayout

	import tkinter as tk

	import visor  

	class Ventana( QWidget ) :
	    def __init__( self ) :
	        super( Ventana, self ).__init__()

	        self.visor = visor.Visor()

	        # Para que se mantenga en top y para ventana sin bordes.
	        self.setWindowFlags( Qt.WindowStaysOnTopHint | Qt.FramelessWindowHint )  
	        self.setAttribute( Qt.WA_TranslucentBackground, True )  # Hace transparente el color gris de los widgets

	        grid = QGridLayout()
	        grid.setContentsMargins( 0, 0, 0, 0 )
	        grid.addWidget( self.visor )
	        self.setLayout( grid )
	     
	    def keyPressEvent( self, e ) :
	       if e.key() == Qt.Key_Escape :
	            self.close()

	    def closeEvent( self, e ) :
	        self.visor.detener()        

	if __name__ == '__main__':
	    app = QApplication( sys.argv )

	    root = tk.Tk()
	    screen_w = root.winfo_screenwidth()
	    screen_h = root.winfo_screenheight()  

	    ventana = Ventana()

	    ancho_imagen_camara = 320
	    alto_imagen_camara = 240
	    ventana.resize( ancho_imagen_camara, alto_imagen_camara )

	    ventana.move( screen_w - ancho_imagen_camara, screen_h - alto_imagen_camara )
	    ventana.show()

	    sys.exit( app.exec() )	        



¿Cómo ejecutamos este código?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- Creamos una carpeta para almacenar este código, por ejemplo: C:\\Cosas\\2021\\PGE2021\\Codigos\\QtCamera
- Podemos usar Sublime Text para editar lo que necesitemos de estos archivos ``ventana.py`` y ``visor.py``
- Abrimos consola con CMD
- Entrar al entorno virtual, ejecutando lo siguiente (acomodar las rutas de ser necesario):

.. code-block:: bash 

	cd C:\Cosas\2021\PGE2021\EntornosVirtuales  # Accedemos a la carpeta

	.\QtCamera\Scripts\activate  # Activamos el entorno virtual

	# Si aparece un mensaje Warning diciendo que hay una versión nueva de pip, 
	# podemos ejecutar el comando que nos recomienda

	pip freeze  # Revisamos el listado de bibliotecas instaladas en el entorno virtual

	python C:\Cosas\2021\PGE2021\Codigos\QtCamera\ventana.py

	# Recordar que para salir debemos desactivar el entorno virtual
	deactivate

	exit  # Para cerrar la consola



**Alternativa para la creación de entornos virtuales**

- Módulo *venv*
- `Documentación de venv <https://docs.python.org/3/library/venv.html>`_ 
- Adaptar los siguientes comandos a lo visto en la `Clase 01 <https://github.com/cosimani/Curso-PIII-2021/blob/main/Clase01.rst>`_ 
- Este módulo ya viene instalado con Python (quizás debemos asegurarnos de esto durante la instalación)

.. code-block:: bash 

	cd C:\Cosas\2021\PGE2021\EntornosVirtuales   # Accedemos a la carpeta en donde creamos los entornos virtuales
	python -m venv QtCamera                      # Creamos el entorno virtual
	.\QtCamera\scripts\activate.bat             # Activamos el entorno virtual

	deactivate                                   # Desactivamos el entorno virtual

	# Para borrar el entorno virtual hay que borrar la carpeta donde se creó
	# -> C:\Cosas\2021\PGE2021\EntornosVirtuales\QtCamera


**Para linux**

.. code-block:: bash 

	python3 --version  # Python 3.8.10

	sudo apt install python3.8-venv  

	python3 -m venv QtCamera

	source ./QtCamera/bin/activate

	pip freeze

	deactivate


Entregable Clase 09
===================

- Punto de partida: La aplicación anterior funcionando
- Utilizar OpenCV para colocar tu nombre centrado en la parte inferior de la imagen.
- Entrar al siguiente `link para ver el registro de los entregables <https://docs.google.com/spreadsheets/d/1xbj6brqzdn3R9sfjDEP0LEjg6CwMNMOb8dBEYGmxhTw/edit?usp=sharing>`_ 
- El link de Youtube se comparte con el docente por mensaje privado de Teams.
- En caso de requerir más tiempo para la entrega, escribir por WhatsApp al docente antes de medianoche.



