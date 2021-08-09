.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 12 - PGE 2020
===================
(Fecha: 6 de octubre)


Creación y uso de librerías dinámicas
=====================================

.. figure:: images/clase08/librerias.png

.. figure:: images/clase08/librerias01.png

.. figure:: images/clase08/librerias02.png

.. figure:: images/clase08/librerias03.png


Ejercicio 19
============

- Crear una clase Login que tenga una funcionalidad independiente
- Crear un librería dinámica que la contenga
- Usar esta librería en otro proyecto

**clase Login que puede servir para usar en este ejemplo**

- Sólo se muestra el código de los archivo login.h y login.cpp
- Tener en cuenta que la GUI de este login está creado con el QtDesigner
- Sus widgets en el layout son: ``ui->leUsuario`` ``ui->leClave`` ``ui->pb``

.. code-block:: c++

	// Archivo login.h ////////////////////////////

	#ifndef LOGIN_H
	#define LOGIN_H

	#include <QWidget>
	#include <QVector>
	#include <QStringList>

	namespace Ui  {
	    class Login;
	}

	class Login : public QWidget  {
	    Q_OBJECT

	public:
	    explicit Login( QWidget * parent = 0 );
	    ~Login();
	    void agregarUsuario( QString usuario, QString clave );

	private:
	    Ui::Login *ui;
	    QVector< QStringList > baseUsuarios;

	signals:
	    void signal_usuarioLogueado();

	private slots:
	    void slot_validarUsuario();
	};

	#endif // LOGIN_H

.. code-block:: c++

	// Archivo login.cpp ////////////////////////////

	#include "login.h"
	#include "ui_login.h"

	Login::Login( QWidget * parent ) : QWidget( parent ), ui( new Ui::Login )  {
	    ui->setupUi( this );

	    ui->leClave->setEchoMode( QLineEdit::Password );

	    connect( ui->pb, SIGNAL( pressed() ), this, SLOT( slot_validarUsuario() ) );
	}

	Login::~Login()  {
	    delete ui;
	}

	void Login::agregarUsuario( QString usuario, QString clave )  {
	    QStringList usuarioNuevo;
	    usuarioNuevo << usuario << clave;
	    baseUsuarios.append( usuarioNuevo );
	}

	void Login::slot_validarUsuario()  {
	    QStringList usuarioIngresado;
	    usuarioIngresado << ui->leUsuario->text() << ui->leClave->text();

	    if ( baseUsuarios.contains( usuarioIngresado) )  {
	        emit signal_usuarioLogueado();
	    }
	}


Ejercicio 20
============

- Utilizar la clase LineaDeTexto.
- LineaDeTexto debe tener las carecterísticas que hemos ido agregando (las sugerencias, los operadores, etc.)
- Crear una librería dinámica con estas clases.
- Crear una carpeta con subcarpetas con los archivos importantes (.h .dll .a)
- Usar esta librería en otro proyecto para probar su funcionamiento.


**Esquema para creación y uso de librerías**

.. figure:: images/clase11/cuadro_librerias.jpg
