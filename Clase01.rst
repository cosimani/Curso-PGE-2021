.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 01 - PGE 2021
===================
(Fecha: 10 de agosto)

:Autor: César Osimani
:Correo: cesarosimani@gmail.com

:Temas principales:
	- Programación genérica y orientada a eventos
	- Sobrecarga de operadores
	- Uso de plantillas
	- Manejo de excepciones
	- Interface Humano Computadora
	- Resolver los problemas consultando documentación técnica de distintas fuentes
	- Funciones callback
	- Manejo de imágenes a nivel píxel
	- OpenGL
	- Procesamiento de imágenes
	- Creación de librerías	

Instalación de herramientas
===========================

:QtCreator: 
	- Desinstalar todas las versiones de Qt ejecutando C:\\Qt\\MaintenanceTool.exe y tildando Uninstall only 
	- Descargar el `Instalador Online <https://www.qt.io/download-thank-you?hsLang=en>`_
	- Ejecutar el archivo qt-unified-windows-x86-4.1.1-online.exe y utilizar una cuenta de Qt
	- Realizar la instalación Qt 6.1 for desktop development. Se requieren 16 GBytes de disco. En caso de querer reducir, se deberá hacer una Custom installation. Toma aproximadamente 45 minutos la descarga e instalación.
	- Asegurarse que en la configuración de QtCreator se utilice Qt 6.1.2 con MinGW 64 bits

:OpenSSL: 
	- Descargar instalador desde la `Página de descarga <https://slproweb.com/products/Win32OpenSSL.html>`_
	- Seleccionar el instalador .exe Win64 OpenSSL v1.1.1k 

:OBS Studio: 
	- Descargar instalador desde la `Página de descarga <https://obsproject.com/es>`_ e instalar

:Blender: 
	- Descargar instalador desde la `Página de descarga <https://www.blender.org/>`_ 
	- Es una herramienta para modelado 3D, animación, edición de video, ...


Metodología didáctica
=====================

- A continuación se resume el conjunto de estrategias, procedimientos y acciones para facilitar el aprendizaje y el logro de nuestros objetivos. 

:Teoría: 
	- Cada clase desde las 15:15 horas hasta las 16:45 horas se expondrán los contenidos teóricos.

:Práctica: 
	- Cada clase desde las 17:00 horas estará destinado a la resulición de ejercicios.
	- El docente propone un ejercicio que, si los contenidos de la materia se mantienen al día, se pueden realizar en 30 minutos.
	- La búsqueda de la resolución del ejercicio se realiza de manera grupal en las mesas de Discord y la entrega es individual.
	- Luego de resolver el ejercicio se debe explicar, en un video compartiendo pantalla, cómo se resuelve. Similar a los Desafíos Qt.
	- Para cada ejercicio se definirá un "Punto de partida" que indicará desde dónde debe comenzar la explicación en el video.
	- Los ejecicios entregados correctamente llevan la nota 10.
	- Ejercicios no entregados llevan nota 0.
	- Ejercicios que no respeten el punto de partida y/o la completitud del ejercicio llevan nota 5.
	- Debido a que puede suceder que alguien no pueda asistir a alguna de las clases, se da tiempo para la entrega del ejercicio hasta la medianoche del mismo día. En caso de necesitar más tiempo por razones particulares, escribir al docente por WhatsApp para fijar una nueva hora de entrega. Pasado este tiempo, el ejercicio queda como no entregado.
	- El video debe quedar disponible en Youtube como No listado y se comparte el link por mensaje privado en Teams.
	- Denominaremos a estos ejercicios como Entregables.
	- El tiempo de duración del video queda a criterio de quien lo entrega.
	- El video se puede editar con Blender si lo desea.

:Regularidad: 
	- Primer parcial: Trabajo práctico individual para resolver en 2 horas (será para la primer quincena de octubre).
	- Para este parcial se trabaja en equipo por Discord en distintas Mesas y se entrega individual ya que son exámenes distintos.
	- Segundo parcial: Promedio de todos los Entregables.
	- Recuperatorios: Se pueden recuperar ambos parciales durante la última semana de cursado. El recuperatorio del primer parcial es un práctico individual para resolver en 2 horas. El recuperatorio del segundo parcial tiene una parte práctica para resolver en 1 hora y una parte teórica para responder oralmente.

Introducción
============

**Programación Genérica**: Generalizar las funciones para que puedan ser utilizadas en varios casos.

Ventajas:
	- Reutilización de código.
	- Fácil mantenimiento de código.
	- Nos concentramos más en la lógica del sistema.

Desventajas:
	- Pérdida de interés para los amantes de la programación a bajo nivel.
	- En C++ requiere el uso de Templates y sobrecarga de operadores, que es dificultoso y poco legible.

**Programación Orientada a Eventos**: La ejecución está determinada por los sucesos que ocurran.
	- Generalmente el usuario es el que dirige la ejecución del programa.
	- Básicamente el programa queda sin actividad hasta producirse un evento.
	- Es la base de la interfaz de usuario.

*Ventajas*:
	- Mejoras en las interfaces de usuario.
	- Uso del mouse (o sea, hace tiempo estamos orientados a eventos)

*Desventajas*:
	- El hilo de ejecución se pierde de vista.
	- Es un tanto abstracto (abstracto se refiere al ¿qué hace? y no tanto en ¿cómo lo hace?, como si fuera una caja negra), se maneja generalmente a alto nivel. 
	- Complicado para manejar los eventos a bajo nivel.
	
Plantillas
==========
- Separa la estructura del contenido.
- Permite construir un diseño predefinido
- Facilita el trabajo de realizar copias idénticas de la estructura.

- Utilización de tipos como parámetros
- Teniendo la función ``ordena( v )``. Dependerá del tipo de v para generar la función.

.. code-block::

    template< class T > void ordena( T v[] )  {
    
    }

- Mecanismo que permite usar un tipo como parámetro en una clase o función.
- Clases genéricas: Es un “constructor” (o creador) de clases (no confundir con el constructor de una clase).
- Para el diseño de una clase genérica es aconsejable ir de lo particular a lo general.
- Primero diseñar y depurar una clase referido a un tipo concreto.
- Libro: El lenguaje de programación C++ de Stroustrup - 13.1 - 13.2 - 13.2.1 - 13.2.2

Entregable Clase 01
===================

- Punto de partida: Computadora sin Qt instalado
- Se pide grabar un video descargando el instalador online de Qt y mostrar hasta que comience la instalación.
- Este primer entregable tiene como objetivo poner a punto el mecanismo de entrega de los ejercicios.

