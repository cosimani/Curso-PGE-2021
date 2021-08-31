.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 07 - PGE 2021
===================
(Fecha: 31 de agosto)


Python, cuadernos de código y herramientas
==========================================

- Python
- Spyder
- Jupyter Notebook
- Anaconda
- Google Colab

.. figure:: images/clase06_pythom_spyder_jupyter_ana_colab.png

Módulos y paquetes
==================

**Módulo**: Es un archivo Python cuyas utilidades (funciones, clases, etc.) se pueden usar desde otro archivo.

- Supongamos el archivo ``matematicas.py``

.. code-block:: python 

	def sumar( a, b ) :
	    return a + b

	def restar( a, b ) :
	    return a - b

- Podremos utilizar estas funciones de la siguiente manera:

.. code-block:: python

	import matematicas  # Esta línea importa todos los recursos del archivo matematicas.py

	print( matematicas.sumar( 7, 5 ) )
	print( matematicas.restar( 17, 15 ) )

- Si sólo deseamos importar la función ``sumar`` hacemos:

.. code-block:: python

	from matematicas import sumar

	print( sumar( 7, 5 ) )

- Otras alternativas:

.. code-block:: python
	
	from matematicas import sumar, restar

	from matematicas import *


**Paquetes**: Es una carpeta que contiene varios módulos. 

.. code-block:: bash 

	operaciones/
	    |-- __init__.py    # Este archivo indica que la carpeta operaciones es un paquete y no una simple carpeta 
	    |-- matematicas.py
	    |-- matrices.py


- Alternativas para importar

.. code-block:: python
	
	import operaciones.matematicas

	from operaciones import matematicas

	from operaciones.matematicas import sumar


Biblioteca numpy
================

- Vectores, matrices, gran colección de funciones matemáticas.
- `Documentación de numpy <https://numpy.org/doc/stable/index.html>`_ 


**Algunos ejemplos de su uso**

.. code-block:: python

	import numpy as np

	lista = [ 25., 8., 20., 75. ] 
	print( type( lista ), lista )

	v = np.array( lista )  # Transformo la lista en vector
	print( '\nv =', v )  # El vector no lleva comas separando los elementos
	print( 'tipo de v:', type( v ) )  # el tipo es numpy.ndarray
	print( 'longitud de v:', len( v ) )

	# máximo y mínimo valor de v
	print( 'máximo de v:', v.max(), 'o', np.max( v ) )  # función de numpy.ndarray: np.max()
	print( 'mínimo de v:', v.min(), 'o', np.min( v ) )


.. code-block:: python

	import numpy as np

	u = np.array( [ 5, 9, 10, -1 ] )  # Transforma la lista en vector
	v = np.array( [ -2, 0, 5, 4 ] )

	print( "vector u =", u )
	print( "vector v =", v )

	z = u + v 
	print( "z = u + v  ->  z =", z )

	w = 2 * z
	print( "2 * z =", w )

	t = w - 3
	print( "Restamos 3 a cada elemento del vector anterior", t )

.. code-block:: python

	import numpy as np

	v = np.zeros( 4, dtype = np.float32 )
	u = np.ones( 4, dtype = np.int64 )
	w = np.full( 4, 128, dtype = np.int8 )
	print( "v =", v,"   u =", u, "   w =", w )

.. code-block:: python

	import numpy as np

	s = np.arange( 5, 26, 3 )
	print( s, type( s ), type( s[ 0 ] ) )

	t = s.astype( np.float32 )  # cambiamos el tipo de datos al vector s a float32
	print( t, type( t ), type( t[ 0 ] ) )

	r = t[ 0 : 3 ]
	print( '\nLos 3 primeros elementos de t son:', r )
	print( 'Muestra 1 =', t[ 3 : ] )
	print( 'Muestra 2 =', t[ : ] )
	print( 'Muestra 3 =', t[ : 5 ] )

	p = t[ [ 1, 3, 5 ] ]
	print( 'Vector con los lugares pares de t:', p )

	lineal = np.linspace( 0, 1, 5 )
	print( lineal )



Biblioteca matplotlib
=====================

- Generador de gráficos.
- `Documentación de matplotlib <https://matplotlib.org/>`_ 


**Algunos ejemplos de su uso**

.. code-block:: python

	import matplotlib.pyplot as plt
	import numpy as np

	n = 21
	x = np.linspace( 0, 2, n )  # del 0 al 2 (inclusive), en n=21 números equiespaciados
	x2 = x * x
	x3 = x ** 3
	plt.plot( x, x, 'b.', x, x2, 'rd', x, x3, 'g^' )

	plt.xlim( -1, 2.5 )  # límites para el eje x
	plt.gca().legend( ( 'Lineal', 'Cuadrática', 'Cúbica' ) )

	plt.show()


.. code-block:: python

	import matplotlib.pyplot as plt
	import numpy as np

	n = np.arange( 0, 5, 1 )
	y = np.exp( np.sin( n ) )

	plt.stem( n, y )
	plt.show()


.. code-block:: python

	import numpy as np
	import matplotlib.pyplot as plt

	# Variables independientes
	x1 = np.linspace( 1, 12, 12 )
	x2 = np.linspace( 1, 12, 12 ) + 2
	x3 = np.linspace( 1, 12, 12 )
	x4 = x2 + x3

	# Para varios gráficos es útil usar la función subplots y luego axs
	# Documentación de subplots en: https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.subplots.html
	fig, axs = plt.subplots( nrows = 2, ncols = 2 )  
	fig.set_figwidth( 10 )
	fig.set_figheight( 6 )

	axs[ 0, 0 ].plot( x1, x2 )
	axs[ 0, 0 ].set_title( 'Gráfico 1' )
	axs[ 0, 0 ].set_xlabel( 'x' )
	axs[ 0, 0 ].set_ylabel( 'y' )

	axs[ 0, 1 ].plot( x1, x3 )
	axs[ 0, 1 ].set_title( 'Gráfico 2' )
	axs[ 0, 1 ].set_xlabel( 'x' )
	axs[ 0, 1 ].set_ylabel( 'y' )


	axs[ 1, 0 ].plot( x1, x4 )
	axs[ 1, 0 ].set_title( 'Gráfico 3' )
	axs[ 1, 0 ].set_xlabel( 'x' )
	axs[ 1, 0 ].set_ylabel( 'y' )

	axs[ 1, 1 ].scatter( x1, x4 )  # scatter plot = Diagrama de dispersión
	axs[ 1, 1 ].set_title( 'Gráfico 4' )
	axs[ 1, 1 ].set_xlabel( 'x' )
	axs[ 1, 1 ].set_ylabel( 'y' )

	plt.show()


.. code-block:: python

	import numpy as np
	import matplotlib.pyplot as plt

	x1 = np.linspace( 1, 12, 12 )
	x2 = np.linspace( 1, 12, 12 ) + 2

	fig, axs = plt.subplots( nrows = 2, ncols = 2 )  

	axs[ 0, 0 ].plot( x1, x2 )
	axs[ 0, 1 ].plot( x1, x2, 'g--d' )  
	axs[ 1, 0 ].scatter( x1, x2 )  
	axs[ 1, 1 ].stem( x1, x2 )
	plt.show()



`Iteraciones y decisiones (ipynb) <https://colab.research.google.com/drive/1AKFdK7Cr7kovUoiSwt9hqz6Zra3THmQb?usp=sharing>`_ 
================================


**Sentencia if**

.. code-block:: python

	edad = int( input( 'Ingrese edad: ' ) )

	if edad < 0 :
	    print( 'Error' )
	elif edad < 18 :
	    print( 'Menor de edad' )
	else :
	    print( 'Mayor de edad' )

	# Operadores para comparar: ==  !=  <  >  <=  >=

**Sentencia for**

.. code-block:: python

	suma = 0
	mis_numeros = [ 4, 8, 12, 18 ] 

	# la variable numero designará a los elementos de la lista
	for numero in mis_numeros :
	    suma = suma + numero
	    print( 'Esto se imprime en cada ciclo. Suma parcial =', suma )

	print( 'Esto se imprime fuera del ciclo' )
	print( 'La lista es:', mis_numeros )
	print( 'La suma de los números de la lista es:', suma )


	print( '\nUn for para range de 0 a 3' )
	for i in range( 4 ) :
    	print( i )

    imagenes = [ 'impulso.jpg', 'gráficos.png', 'esquema.jpg' ]
	for imagen in imagenes :
	    print ( '\nEl nombre de la imagen es: {0} y el largo ' \
	            'del nombre es: {1}'.format( imagen, len( imagen ) ) )    

	mi_lista = [ 'manzana', 'bananas', 'uvas', 'peras' ]
	# La primera variable (c en nuestro caso) es el contador
	# La segunda variable (valor en nuestro caso) es precisamente el valor almacenado en cada posición
	# El segundo parámetro de eumerate es desde dónde comienza la enumeración 
	for c, valor in enumerate( mi_lista, 0 ) :
	    print( '\n', c, valor )

**Sentencia while**

.. code-block:: python

	n = 5
	print( 'Loop 1 started.' )
	while n > 0 :
	    n -= 1
	    if n == 2 :
	        break
	    print( n )
	print( 'Loop 1 ended.' )

	n = 5
	print( 'Loop 2 started.' )
	while n > 0 :
	    n -= 1
	    if n == 2 :
	        continue
	    print( n )
	print( 'Loop 2 ended.' )


`Sonidos humanamente audibles (ipynb) <https://colab.research.google.com/drive/1HqBx7K9EH9fG0DXtqDiiMD8lv4pGLdpb?usp=sharing>`_ 
=====================================

- Aproximadamente entre 20 Hz y 20 kHz

.. code-block:: python

	import numpy as np

	# Para reproducir audio en la notebook.
	from IPython.display import Audio, display

	sample_rate = 44100

	segundos_de_audio = 2
	n = np.linspace( 0, segundos_de_audio, sample_rate * segundos_de_audio )
	tono = 440
	data = np.sin( 2 * np.pi * tono * n )

	data

	def generador_de_tono( frecuencia, duracion, sample_rate, A = 1 ) :
	    n = np.linspace( 0, duracion, sample_rate * duracion )
	    return np.sin( 2 * np.pi * frecuencia * n )

	la440 = generador_de_tono( 440, 1, 44100 )

	Audio( data = la440, rate = 44100 )

	# Todas las notas comenzando desde el 'La'
	#   La La# Si Do Do# Re Re# Mi Fa Fa# Sol Sol# La
	n_0 = 440
	notas = [ 440 * 2**( n / 12 ) for n in range( 0, 13 ) ]

	# Tono de cada nota
	muestras_de_todas_las_notas = [] 
	for frecuencia_de_nota in notas : 
	     muestras_de_todas_las_notas.append( generador_de_tono( frecuencia_de_nota, 1, 44100 ) )

	index_notas = [ 0, 2, 4, 5, 7, 9, 11, 12 ]
	escala_La_mayor = [ muestras_de_todas_las_notas[ i ] for i in index_notas ]

	muestras_escala_La_mayor = np.concatenate( escala_La_mayor )

	Audio( muestras_escala_La_mayor, rate = 44100 )



Entregable Clase 07
===================

- Punto de partida: Cuaderno Colab llamado entregable05.ipynb
- Reproducir la escala pentatónica menor de La
- Buscar información donde desee.
- Deben sonar las notas que se pueden escuchar `Aquí <https://es.wikipedia.org/wiki/Archivo:PentMinor.mid>`_ 
- Entrar al siguiente `link para ver el registro de los entregables <https://docs.google.com/spreadsheets/d/1xbj6brqzdn3R9sfjDEP0LEjg6CwMNMOb8dBEYGmxhTw/edit?usp=sharing>`_ 
- El link de Youtube se comparte con el docente por mensaje privado de Teams.


