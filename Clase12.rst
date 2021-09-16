.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 12 - PGE 2021
===================
(Fecha: 16 de septiembre)


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


Constructor copia
=================

.. code-block:: c

	Persona( const Persona & persona );	

- "Copiar" un objeto supone la creación de otro objetos
- Puede ser una copia física o lógica
- La copia completa o física de objetos grandes es costosa
- Siempre que se necesite crear un nuevo objeto se llama (o invoca) al constructor copia. 

:Se invoca al constructor copia en los siguientes momentos:
	- Cuando se pasa un objeto como argumento
	- Cuando algún método o función devuelve un objeto
	- Cuando se crea un objeto mediante asignación
	- Cuando se lanza una excepción

- El compilador crea un constructor copia oficial cuando no se define explícitamente
- Este constructor realiza un clon exacto miembro a miembro
- A veces es necesario definir un constructor copia explícito


Operador de asignación
======================

- El operador de asignación oficial es análogo al constructor copia oficial
- El compilador lo define si no hay una versión explícita
- La diferencia con el operador de asignación es que ambos objetos deben ser creados previamente

.. code-block:: c

	Persona & operator=( const Persona & persona );

- Hay inconvenientes con el constructor copia oficial cuando los miembros son punteros
- Igual sucede con el operador de asignación oficial	


Entregable Clase 12
===================

- Punto de partida: Utilizar el código desarrollado en clase para agregar lo que pide este entregable.
- Crear un QVector< QWidget > y agregar 3 objetos (puede hacerlo directamente en la función main).
- Tener en cuenta que no son punteros, sino objetos concretos de QWidget.
- Usar un for para extraerlos y mostrarlos como ventanas independientes.
- Entrar al siguiente `link para ver el registro de los entregables <https://docs.google.com/spreadsheets/d/1xbj6brqzdn3R9sfjDEP0LEjg6CwMNMOb8dBEYGmxhTw/edit?usp=sharing>`_ 
- El link de Youtube se comparte con el docente por mensaje privado de Teams.
- En caso de requerir más tiempo para la entrega, escribir por WhatsApp al docente antes de medianoche.

