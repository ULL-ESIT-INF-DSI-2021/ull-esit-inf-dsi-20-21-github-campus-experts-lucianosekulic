# Informe Práctica 1: Configuración de máquina virtual en el Iaas.
## 1-. Introducción:

En esta primera práctica de la asignatura de ***Desarrollo de Sistemas Informáticos***, se nos ha propuesto por un lado la instalación y preparación de una máquina virtual en el ***Iaas*** para desarrollar la asignatura. Luego por otro lado realizaremos la configuración necesaria para que nuestra máquina local tenga una conexión más rápida entre ambas máquinas y aprenderemos a usar ***Markdown*** y ***Github Pages***.

## 2-.  Objetivos:

Esta práctica tiene como objetivo principal la preparación del entorno de trabajo que utilizaremos en la asignatura. Sin embargo dentro del mismo podemos destacar:

* Utilización de la ***VPN*** de la ULL para acceder al Iaas.
* La realización de conexiones remotas con ***SSH***.
* La configuración de un ***prompt*** de la máquina virtual.
* La familiarización con las ***Claves SSH***.
* La instalación de ***Git*** y ***Node***.
* Aprender a usar ***Markdown*** y ***Github Pages***.

## 3-. Desarrollo de la práctica:
### Tareas previas a la Práctica 1

Antes de empezar la realización de la práctica 1 es necesario tener previamente las siguientes tareas:

* Elegir un grupo de trabajo.
* Cumplimentar una encuesta sobre las expectativas y conocimientos previos de la asignatura.
* Darse de alta en ***Google Classroom***.
* Tener una cuenta de ***Github*** asociada al ***correo institucional*** para solicitar los beneficios que ofrece la plataforma para estudiantes.
* Darse de alta en ***Githuub Classrooom*** y aceptar la asgnación correpondiente.
* Leer la bibliográfia de ***Markdown***, ***Github Pages*** y ***Jekyll***.

Teniendo el repositorio para nuestra práctica, nos dispondremos a crear un Github Pages. Para ello vamos al apartado ***Settings*** en nuestro repositorio, buscamos el apartado ***Github Pages***, escogemos la rama deseada en el apartado ***Branch*** y escogemos el tema que más nos guste.

### 3.1-. Configuración de la máquina virtual Iaas
#### 3.1.1-. Conectarde a la VPN de la ULL

En primer lugar nos tendremos que conectar a la ***VPN*** de la ULL para poder entrar al entorno Iaas. En mi caso para esta práctica he utilizado Windows, para ello he descargado ***Global Protect***. Una vez descargado introduzco ***vpn.ull.es*** y le damos a conectar. Por último introducimos nuestro ***alu*** y ***contraseña institucional*** para estar conectados en la VPN.

Sin embargo también lo podemos hacer en un sistema basado en linux siguiendo los pasos del enlace posteriormente dado.

Estos datos se pueden ver en el siguiente enlace proporcionado por la ULL.
[Configuracion VPN de la ULL]
(https://www.ull.es/servicios/stic/2020/12/01/servicio-de-vpn-de-la-ull/)

#### 3.1.2-. Acceso al Iaas de la ULL

Una vez conectados a la ***VPN*** de la ULL, accedemos a la web del [Iaas](https://iaas.ull.es). Ahí entramos con nuestras credenciales institucionales, elegimos la máquina vitual correspondiente a ***DSI*** y la iniciamos. Luego de iniciarla, se asignará un nombre, en mi caso es ***DSI-54***. En el apartado de ***Interfaces de red*** podremos ver la ***IP*** de nuestra máquina virtual para conectarnos por ***SSH***.

```bash
ssh usuario@10.6.XXX.XX
```

Una vez conectados se mostrará el siguiente mensaje:

```bash
The authenticity of host '10.6.XXX.XXX (10.6.XXX.XXX)' can't be established.
...
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

Introduciremos ***YES***. Luego, nos va a pedir la contraseña actual (usuario) y posteriormente vamos a poner dos veces una contraseña nueva de nuestra preferencia.

Después debemos cambiar el nombre de la máquina virtual abriendo el archivo ***etc/hostname***.

```bash
usuario@10.6.129.221:~$ cat /etc/hostname 
usuario
usuario@10.6.129.221:~$ sudo vi /etc/hostname
[sudo] contraseña para usuario: 
usuario@10.6.129.221:~$ cat /etc/hostname 
iaas-dsi54
```

Debemos cambiar el nombre antiguo de la máquina en el fichero ***/etc/hosts*** por el nuevo.

```bash
cat /etc/hosts
127.0.0.1	localhost
127.0.1.1	usuario
...

usuario@10.6.129.221:~$ sudo vi /etc/hosts
usuario@10.6.129.221:~$ cat /etc/hosts
127.0.0.1	localhost
127.0.1.1	iaas-dsi54
...
```

Vamos a proceder a reiniciar la máquina virtual pero antes vamos a instalar y actualizar el software con los siguientes comandos:

```bash
usuario@10.6.129.221:~$ sudo apt update
...
usuario@10.6.129.221:~$ sudo apt upgrade
...
```

Una vez hecho esto, procedemos a reiniciar el sistema

```bash
usuario@10.6.129.221:~$ sudo reboot
```
Ahora procedemos a configurar el archivo ***/etc/hosts*** de la máquina local mientras la máquina virtual se reinicia.

```bash
luciano@pc:~$ cat /etc/hosts
127.0.0.1	localhost
127.0.1.1	pc

luciano@pc:~$ sudo vi /etc/hosts
[sudo] contraseña para luciano: 
luciano@pc:~$ cat /etc/hosts
127.0.0.1	localhost
127.0.1.1	pc

10.6.129.221  iaas-dsi54
```

#### 3.1.4-. Claves públicas-privadas

Las ***claves SSH*** nos sirven para acceder a nuestra máquina virtual sin necesidad de poner la contraseña correspondiente cada vez que entres. La clave se crea con el siguiente comando

```bash
luciano@pc:~$ ssh-keygen
```
Para generar la clave se pueden poner las opciones por defecto, es importante no introducir ninguna **passphrase** asociada al par de claves públicas-privada.

Una vez creada la clave se puede mostrar con el siguiente comando:

```bash
luciano@pc:~$ cat .ssh/id_rsa.pub 
```

Luego, usamos el siguiente comando

```bash
luciano@pc:~$ ssh-copy-id usuario@iaas-dsi54
```

Esto nos permite copiar la clave de la máquina local en la virtual para poder conectarnos mediante ***SSH*** a la máquina virtual sin necesidad de acordarnos de la ***IP*** y la ***contraseña***.

```bash
luciano@pc:~$ ssh usuario@iaas-dsi54

luciano@pc:~$ ssh usuario@iaas-dsi54
Welcome to Ubuntu 18.04.5 LTS (GNU/Linux 4.15.0-135-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Sat Feb 21 16:30:36 WET 2021
  ...
```

Para no tener que utilizar el nombre de usuario cada vez que querramos entrar a nuestra máquina virtual, editaremos el fichero ***/.ssh/config***.


```bash
luciano@pc:~$ vi ~/.ssh/config
luciano@pc:~$ cat ~/.ssh/config

Host iaas-dsi54
  HostName iaas-dsi54
  User usuario
```

Una vez hecho esto podemos realizar la conexión ***SSH*** indicando solamente el nombre de la máquina.

```bash
luciano@pc:~$ ssh iaas-dsi54
```

Por último nos quedaría generar las ***claves pública-privada*** en la ***máquina virtual***. Este paso es idéntico al realizado para la ***máquina local***

``bash
usuario@iaas-dsi54:~$ ssh-keygen
Generating public/private rsa key pair.
...
```
```bash
usuario@iaas-dsi54:~$ cat .ssh/id_rsa.pub 
```

### 3.2-. Instalación de git y Node.js en la máquina virtual del IaaS
#### 3.2.1-. Instalación y configuración de git

Para instalar ***Git*** en nuestra ***máquina virtual*** usaremos el siguiente comando: 

```bash
usuario@iaas-dsi54:~$ sudo apt install git
```

Una vez instalado, seguimos con unas configuraciones iniciales:

```bash
usuario@iaas-dsi54:~$ git config --global user.name "Jacobo Labrador"
usuario@iaas-dsi54:~$ git config --global user.email alu0101119663@ull.edu.es
usuario@iaas-dsi54:~$ git config --list
user.name=Luciano Sekulic
user.email=alu0101022239@ull.edu.es
```

#### 3.2.2-. Configuración del prompt de la terminal de la máquina virtual

Vamos a realizar la configuración de un prompt de la terminal de la máquina virtual, nos ayudará a saber en que rama actual nos encontramos cuando estemos en un determinado directorio asociado a un ***repositorio git***. Para ello debemos descargar el [script del prompt](https://github.com/git/git/blob/master/contrib/completion/git-prompt.sh).

Otra opción es crear un fichero el nombre ***git-prompt.sh*** mediante el comando ***touch*** y abrir con ***vi*** para copiar el contenido dentro del fichero. Por último realizamos las siguientes secuencias de comandos para modificar el fichero ***.bashrc*** y reiniciar la terminal.

```bash
usuario@iaas-dsi54:~$ mv git-prompt.sh .git-prompt.sh
usuario@iaas-dsi54:~$ vi .bashrc
usuario@iaas-dsi54:~$ tail .bashrc
...
source ~/.git-prompt.sh
PS1='\[\033]0;\u@\h:\w\007\]\[\033[0;34m\][\[\033[0;31m\]\w\[\033[0;32m\]($(git branch 2>/dev/null | sed -n "s/\* \(.*\)/\1/p"))\[\033[0;34m\]]$'

usuario@iaas-dsi54:~$ exec bash -l
[~()]$
```

#### 3.2.3 Conexión de la máquina virtual con Github.

Como se aprecia en la última línea del ejemplo anterior, el prompt ha cambiado. Pero aún debemos comprobar que funcione de forma correcta. Vamos a añadir la clave pública de nuestra máquina virtual en la configuración de las claves de nuestra cuenta de Github. Esto nos facilitará el trabajo con repositorios remotos, y podremos clonar un repositorio para hacer la prueba de que el ***prompt*** funcione correctamente.

```bash
[~()]$cat ~/.ssh/id_rsa.pub
```

Con la clave pública copiada realizaremos lo anteriormente dicho. vamos a realizar los siguientes pasos:
* Vamos a nuestra cuenta de Github -> *Account Settings*
* En la sección de ***SSH and GPG keys***, pulsamos ***New SSH key***
* Añadimos un título y pegamos la ***clave pública*** de nuestra máquina virtual
* Pulsamos sobre ***Add SSH key*** para confirmar.

```bash
[~()]$git clone git@github.com:ULL-ESIT-INF-DSI-2021/prct01-iaas-vscode.git
Clonando en 'prct01-iaas-vscode'...
remote: Enumerating objects: 156, done.
remote: Counting objects: 100% (156/156), done.
remote: Compressing objects: 100% (117/117), done.
remote: Total 156 (delta 67), reused 85 (delta 34), pack-reused 0
Recibiendo objetos: 100% (156/156), 351.19 KiB | 1.02 MiB/s, listo.
Resolviendo deltas: 100% (67/67), listo.
```

```bash
[~()]$ls
prct01-iaas-vscode
[~()]$cd prct01-iaas-vscode/
[~/prct01-iaas-vscode(main)]$
```
Como se puede apreciar, hemos hecho todos los pasos correctamente ya que en el ***prompt*** se puede ver que estamos en la rama *main*.

#### 3.2.4-. Instalación de Node

Vamos a instalar Node Version Manager (nvm). Es un gestor de versiones de ***Node.js*** el cual es un entorno que permite la ejecución de código desarrollado en ***JavaScript*** y otros como ***TypeScript***.
Para instalarlo haremos:

```bash
[~()]$wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.37.2/install.sh | bash
[~()]$exec bash -l
$exec bash -l
```
Ahora instalaremos la version más reciente de ***Node.js***.

```bash
[~()]$nvm install node
...
```
Podemos comprobar las versiones de ***Node*** y de ***NPM***.

```bash
[~()]$node --version
v15.9.0
[~()]$npm --version
7.5.3
```
Para instalar una ***version concreta de NVM*** lo haremos de la siguiente forma:

```bash
[~()]$nvm install 12.0.0
...
```
Volvemos a comprobar las versiones de ***Node*** y de ***NPM***.

```bash
[~()]$node --version
v12.0.0
[~()]$npm --version
6.9.0
```

Podemos comprobar que versión se está utilizando en estos momentos.

```bash
[~()]$nvm list
->      v12.0.0
        v15.9.0
default -> node (-> v15.9.0)
iojs -> N/A (default)
unstable -> N/A (default)
node -> stable (-> v15.9.0) (default)
stable -> 15.9 (-> v15.9.0) (default)
lts/* -> lts/fermium (-> N/A)
lts/argon -> v4.9.1 (-> N/A)
lts/boron -> v6.17.1 (-> N/A)
lts/carbon -> v8.17.0 (-> N/A)
lts/dubnium -> v10.23.3 (-> N/A)
lts/erbium -> v12.20.2 (-> N/A)
lts/fermium -> v14.15.5 (-> N/A)
```

Para finalizar se puede cambiar de versiones de la siguiente forma.

```bash
[~()]$nvm use v15.9.0
Now using node v15.9.0 (npm v7.5.3)
```

Y comprobamos que el cambio se ha hecho de forma satisfactoria.

```bash
[~()]$node --version
v15.9.0
[~()]$npm --version
7.5.3
```

## 4-. Conclusiones

En conclusión, la realización de esta práctica me refresco conocimientos adquiridos en otras asignaturas tales como la utilización de la VPN y el entorno del Iaas. En cuanto a Git y Github, no habia tenido ninguna toma de contacto con la plataforma como lo he hecho en esta práctica, simplemte lo he utilizado para subir códigos de otras asignatuas.

Sin embargo he aprendido a configurar Node.js, nunca antes lo haba hecho y al leer información al respecto me pareció muy interesante. Por último he de destacar que tanto el lenguaje de marcas, Markdown y las herramientas de Github Pages nunca había leído sobre ellas ni tenía conocimiento de su existencia, por lo que me ha parecido muy interesante sus funciones y adquirir la habilidad de poder usarlo en esta práctica.

Finalmente he de destacar que la dificultad de esta primera práctica no ha sido muy alta. Sin embargo he tenido ciertos problemas en afrontar la parte de Github debido a mi escaso conocimiento sobre el tema pero fue satisfactorio poder aprender sobre un tema interesante y nuevo.

## 5-. Bibliografía

Guión Práctica 1 - https://ull-esit-inf-dsi-2021.github.io/prct01-iaas/
Configurar Git por primera vez - https://git-scm.com/book/es/v2/Inicio---Sobre-el-Control-de-Versiones-Configurando-Git-por-primera-vez   
Libro Pro Git de Scott Chacon y Ben Straub - https://git-scm.com/book/es/v2   
Información Github Pages - https://docs.github.com/es/github/working-with-github-pages/about-github-pages   
Web Jekyll - https://jekyllrb.com/   
Información Node.js - https://nodejs.org/es/about/   
Node Version Manager - https://github.com/nvm-sh/nvm   
Node Package Manager - https://www.npmjs.com/  
Introducción a Markdown - https://guides.github.com/features/mastering-markdown/





