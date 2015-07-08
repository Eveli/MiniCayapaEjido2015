Pasos para canaimizar debian 8 nombre código Jessie

1. Instalar debian 8

2. Editar repositorios

	# [EDITOR] /etc/apt/sources.list
		deb http://[REPO]/debian jessie main contrib non-free
		deb http://[REPO]/canaima kukenan main 

	NOTA: Si los repositorios a los que se va a conectar estan almacenados en un dispositivo local, la ruta sera algo como:
		deb file:///[REPO]/debian jessie main contrib non-free 

3. Actualizar los repositorios

	# apt update

4. Instalar el software necesario para las pruebas:
	git -> para versionar paquetes fuentes.
	git-buildpackage -> crear los paquetes.
	gdebi -> instalar paquetes.
	build-essential -> dependencias de compilación.
	canaima-llaves -> para que el repositorio valide el paquete.
	ssh -> para conexión con equipos remotos.

	# apt install git git-buildpackage gdebi build-essential canaima-llaves ssh

5. Seleccionar un paquete. Por ejemplo: canaima-estilo-visual-gnome

6. Intentar instalar el paquete

	# apt install canaima-estilo-visual-gnome 

7. Si el paquete tiene problemas de dependencias, seleccionar una de las dependencias e intentar instalarla. Por ejemplo: canaima-estilo-visual-gnome depende de gnome-shell-extensions-gnamon. 

	NOTA: Se debe repetir este paso hasta llegar a una dependencia que no sea parte de Canaima. Por ejemplo: gnome-shell-extensions-gnamon depende de gnome-shell. gnome-shell-extensions-gnamon es un paquete de canaima, gnome-shell ya no es un paquete de canaima.

8. Crear un usuario en github para guardar las distintas versiones de los cambios que se realicen a los paquetes.

	http://github.com

9. Ir a la ruta paquetes.canaima.softwarelibre.. , buscar el paquete a descargar, darle clic en el enlace y copiar la dirección del repositorio git.

10. Crear un repositorio en git con el nombre del paquete seleccionado e importar el codigo pegando la dirección copiada en el paso anterior. Copiar la ruta del repositorio ahora creado, para clonarlo en nuestro equipo.

11. Crear un carpeta donde se descagarán los paquetes a probar

	$ mkdir paquetes

12. Entrar a la carpeta

	$ cd paquetes

13.  CONFIGURAR GIT 

	.1. Configurar el nombre del usuario en git

		$ git config --global user.name "Francisco Guerrero"

	.2. Configurar el correo del usuario en git
	
		$ git config --global user.email "jsfrncscg@gmail.com"

	NOTA: Ninguna de estas configuraciones debe hacer como ROOT

14. Clonar el repositorio del paquete
	
	$ git clone RUTA

15. A partir de aca empiezan a realizar los cambios respectivos para arreglar los problemas que impiden la instalacion del paquete. Durante este proceso lo mas comun es hacer lo siguiente: 

	.1. Modificar el paquete en la carpeta de codificación debian->control

	.2. Agregar los cambios al repositorio local
		
		$ git add .

	.3. Descripción de los cambios realizados

		$ git commit -a

16. Una vez guardados los cambios y realizado el commit es necesario actualizar la versión del paquete. Para eso colocamos lo siguiente: 

	.1. ACTUALIZAR CHANGELOG DEL PAQUETE
		
		$ gbp dch --new-version="X.Y" --release --auto --id-length=7 --full --commit --git-author

17. Crear carpeta comprimida del codigo fuente (orig.tar.gz)

Previamente hay que exportar ciertas variables de entorno

	.1. EXPORTAR NOMBRE DEL PAQUETE Y NUMERO DE VERSION

		$ export PKGNAME=$(dpkg-parsechangelog | grep-dctrl -esSource . | awk -F' ' '{print $2}')
		export VERSION=$(dpkg-parsechangelog | grep-dctrl -esVersion . | awk -F' ' '{print $2}')
		export ORIGVER=$( echo $VERSION | sed 's/-.*//g' )

	.2. CREAR ORIG

		$ tar --anchored --exclude-vcs --exclude "./debian" --exclude "./.sublime" -cvzf ../$( echo $PKGNAME"_"$ORIGVER ).orig.tar.gz --directory=$(pwd) ./

18. Construir el paquete

	$ gbp buildpackage -tc --git-tag --git-retag -uc -us

19. Probar el paquete para ver si funciona correctamente

Abrir con gdebi el archivo .deb que se creo y tratar de instalarlo. Si no da problemas procedemos a subirlo al repositorio para que este disponible al público.

20. Subir paquete al repositorio en Github

	$ git push origin master

21. Subir paquete binario al repositorio

	.1. Copiar el paquete en la maquina del repositorio

		$ scp <<paquete.deb> root@ipmaquina:/RUTA

	.2. Conectar por ssh a la maquina del repositorio

		$ ssh usuario@ipmaquina

	.3. Entrar a la ruta del repositorio

		$ cd RUTA

	.4. Subir paquete

		$ reprepo includedeb repositorio /RUTA del paquete.deb