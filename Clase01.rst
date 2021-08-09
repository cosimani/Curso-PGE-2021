.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 01 - PGE 2021
===================
(Fecha: 10 de agosto)

:Autor: César Osimani
:Correo: cesarosimani@gmail.com
:Regularidad: 
	- Primer parcial: Trabajo práctico individual para resolver en 3 horas.
	- Segundo parcial: Trabajo práctico individual para resolver en 3 horas.
	- Para los parciales se trabaja en equipo por Discord en distintas Mesas y se entrega individual ya que son exámenes distintos.
	- Tercer parcial: Cada día de clases hay un práctico para entregar individualmente. En caso de no asistir en el horario de clases se podrá entregar un video (grabado con OBS) con el desarrollo completo del práctico.
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
	- Desinstalar todas las versiones de Qt ejecutando C:\Qt\MaintenanceTool.exe y tildando Uninstall only 
	- Descargar el `Instalador Online <https://www.qt.io/download-thank-you?hsLang=en>`_
	- Ejecutar el archivo qt-unified-windows-x86-4.1.1-online.exe y utilizar una cuenta de Qt
	- Realizar la instalación Qt 6.1 for desktop development. Se requieren 16 GBytes de disco. En caso de querer reducir, se deberá hacer una Custom installation. 16:21

:OpenSSL: 
	- Descargar instalador desde la `Página de descarga <https://slproweb.com/products/Win32OpenSSL.html>`_
	- Seleccionar el instalador .exe Win64 OpenSSL v1.1.1k Light


:OBS Studio: 
	- Descargar instalador desde la `Página de descarga <https://obsproject.com/es>`_ e instalar


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

