# Mini Cayapa Ejido Junio de 2015

## *Objetivo*

Iniciar el desarrollo de Canaima 5 partiendo de la modificación de los paquetes de Debian 8.

## *Actividades Realizadas*

1. Instalación de debian 8 en varios equipos

2. Agregar repositorios de Canaima 4.1 al sources.list de Debian 

3. Instalación de software necesario para las pruebas a realizar

> git -> para versionar paquetes fuentes.
> 
> git-buildpackage -> crear los paquetes.
> 
> gdebi -> instalar paquetes.
> 
> build-essential -> dependencias de compilación.
> 
> canaima-llaves -> para que el repositorio valide el paquete.
> 
> ssh -> para conexión con equipos remotos.
> 
> Un buen editor de texto.

4. Intentar instalar paquetes de Canaima en Debian y conocer si sus dependencias se cumplen

5. Descargar los paquetes fuentes seleccionados de Canaima 4.1 desde el repositorio de paquetes de [Canaima](http:git.canaima.softwarelibre.gob.ve)

6. Revisión de la estructura de paquetes y dependencias que requiera ser actualizadas o darle mantenimiento a la misma

7. Reempaquetamiento de los paquetes actualizados a un repositorio ssh creado para ese fin.

8. Prueba de temas de Canaima en el plymouth de Debian

## *Conocimientos adquiridos*

* Instalación de Debian 8
* Manejo básico de git

	```sh
	# Configura el nombre de del usuario GIT
	$ git config --global user.name "Nombre del usuario GIT"
	# Configura el correo del usuario GIT
	$ git config --global user.email "usuario@email.com"
	```

* Canaimizar paquetes de debian
	* Cambiar la versión y/o dependencias de los paquetes
	* Manipulación de los archivos control y changelog
	* Empaquetar
	* Comandos

		```sh 
		# Actualizar changelog
        gbp dch --new-version="X.Y" --release --auto --id-length=7 --full --commit --git-author
      
        # Exportar nombre del paquete y número de versión
        export PKGNAME=$(dpkg-parsechangelog | grep-dctrl -esSource . | awk -F' ' '{print $2}')
		export VERSION=$(dpkg-parsechangelog | grep-dctrl -esVersion . | awk -F' ' '{print $2}')
		export ORIGVER=$( echo $VERSION | sed 's/-.*//g' )
        
        # Copia del paquete original
		tar --anchored --exclude-vcs --exclude "./debian" --exclude "./.sublime" -cvzf ../$( echo $PKGNAME"_"$ORIGVER ).orig.tar.gz --directory=$(pwd) ./
        
        # Construir el paquete
		gbp buildpackage -tc --git-tag --git-retag -uc -us
        ```