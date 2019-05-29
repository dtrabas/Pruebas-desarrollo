
RESUMEN DEL eBOOK PRO GIT
=========================

[https://git-scm.com/book/en/v2]


Parámetros que puedes utilizar ANTES del comando
------------------------------------------------

	git --git-dir=e-Tica_web/.git --no-pager <...comando...>
	
		--git-dir	--> Ruta a la carpeta .git del proyecto sobre el que deseamos trabajar
		
		--no-pager	-->	No paginar la salida (comando 'less' en GitBash)


Introducción
-------------

* `git init`

	Situados en la carpeta que queremos que controle git, este comando creará la carpeta oculta `.git` que contendrá el esqueleto del repositorio git para esa carpeta. En este punto ningún fichero de la carpeta está trackeado todavía.

	
* `git clone <url>`

	Clona un repositorio existente en la carpeta en la que estamos situados. La primera vez que clonas un repositorio, todos los ficheros están en estado *tracked* y *sin modificar*.
	
	
* `git help <nombreComandoGit>`

	Abre en el navegador por defecto la `man page` del comando indicado
	
	
* `git <nombreComandoGit> -h`

	(ó `--help`)
	
	Muestra por consola los parámetros disponibles para el comando indicado.
	

* `git status`

	Muestra el estado de nuestros ficheros
	
	
* `git status -s`

	(ó `--short)

	Muestra el estado de nuestros ficheros en formato reducido (el nombre del fichero precedido de dos letras):
	
		. ?? Fichero sin trackear
		. A  Fichero nuevo añadido al stage area
		. M  Fichero modificado añadido al stage area
		.  M Fichero (trackeado) modificado
		. MM Fichero (trackeado) modificado, stageado y modificado de nuevo
		. AM Fichero nuevo añadido al stage area  y modificado de nuevo 

	Los ficheros en Git pueden estar en dos estados: tracked o untracked

	*Tracked*: son los ficheros que estaban en tu último snapshot. A su vez, pueden estar en tres estados:
	
		- Sin modificar (commited): el fichero está guardado en tu database local 
	
		- Modificado: has cambiado el fichero pero aún no lo has añadido al stage area.
	
		- Staged: has marcado un fichero modificado para ir en tu próximo commit snapshot
			
	*Untracked*: el resto de ficheros. Es decir, ficheros que no estaban en el último snapshot ni están en el stage area.
	
	*Stage area*
		
		Area que contiene los ficheros que serán commiteados en el próximo `git commit`
	
		
* Fichero .gitignore

	Contiene los ficheros de los que no queremos que se haga tracking. Para un proyecto java típico serían:
	
		$ cat .gitignore
		.classpath
		.project
		.settings
		target/
		/bin/
		
	Si queremos omitir un tipo de archivo en todas las subcarpetas: 
	
		**.tipo
	
	Por ejemplo, para omitir todos los archivos con extensión tmp: **.tmp
	
			
* `git add <file>`

	Pasa el fichero indicado al area staged. Por tanto, si el fichero es nuevo, pasa a estar trackeado.
	

* `git diff <parametros> <file>`

	Sin parámetros: Muestra los cambios del fichero *Modified* (no está aún *staged*) respecto del fichero en el area staged ó de su último *commit* si no está en dicho area

	`--staged`: (ó `--cached`) Muestra los cambios del fichero en el area *staged* respecto de su último *commit*. Si también hay cambios en el fichero *Modified*, estos no se mostrarán.
	
	>Ojo: si el fichero está en el área *staged*, `git diff` sin parámetros no mostrará ningún cambio!!!
	
	`git diff <ID_DE_COMMIT_A_COMPARAR> <file>` Muestra los cambios de los ficheros *Modified/Staged* respecto de ese *commit*. 
	
	>Ojo: para las diferencias del *commit* SI se tiene en cuenta el area *staged*!!!
	
	`git diff HEAD <file>` nos mostrará todos los cambios del fichero ya esté modificado o en el *stage area*.
	
		
* `git commit <file>`

	Hace commit del <file> del staged area. Se abre el editor por defecto para escribir la descripción del commit. Ejecutado sin indicar el nombre del fichero que queremos añadir al commit, añadirá todos los ficheros del staged area.
	
	`git commit -a` (ó `--all`) Pasa todos los ficheros en seguimiento (tracked) al staged area (e.d. ejecuta git add ./) y después realiza el commit.
	
	>Ojo: sólo tiene en cuenta los ficheros tracked. Si acabas de crear un fichero nuevo, este estará untracked y no será tenido en cuenta en el commit. Así que primero tendrás que ejecutar 'git add .' para para pasar a tracked los ficheros nuevos.
	
	`git commit -m 'descripción del commit' <file>` (ó `--message`) Añade el fichero al stage area y ha commit del mismo. También permite indicar la descripción del commit en la misma línea de comando. 
	
	`git commit --amend` Nos permite modificar el comentario del último commit y/o añadir el contenido del stage area a dicho commit. Este proceso reemplaza completamente el commit original de tal forma que es como si nunca se hubiera realizado.
	

* git rm <file>

	Borra el <file> (rm <file>) y mueve el cambio al stage area (git add <file>)
	
* git mv <file1> <file2>

	Renombra file1 a file2. 
	
	Es equivalente a ejecutar estos comandos:
	
		mv file1 file2
		git rm file1
		git add file2
	
* git log

	Muestra los commits realizados sobre el repositorio en orden cronológico inverso, es decir, el commit mas reciente se muestra el primero.
	
	Las características avanzadas de este comando pueden dividirse en dos categorías: 
	
		1) Formatear como se muestra cada commit
		
		2) Filtrar que commits se incluyen en la salida
		
	Parámetros utilizados para el formateo:
	
		--oneline	-->	Condensa cada commit en una única línea
		
		--decorate	--> Muestra todas las referencias (ramas, tags, etc) que apuntan a cada commit
		
		--stat		--> Muestra el número de inserciones y borrados de cada commit 
		                La cantidad de signos + y - a lado de cada nombre de archivo indica el número relativo de cambios a cada fichero alterados por el commit. Esto te da una idea de donde están los cambios del commit.
						
		                (ojo: modificar una línea se representa como 1 insertion y 1 deletion)
						
		-p 			--> Muestra los cambios realizados a cada fichero del commit
		--patch
		
		--graph		--> Dibuja un gráfico ASCII representando la estructura de ramas de la historia del commit. 
						Suele utilizarse junto a --oneline y --decorate para facilitar determinar visualmente que commit pertenece a que rama.
						
							git log --graph --oneline --decorate
							
						El asterisco muestra sobre que rama se hizo el commit.
						
		--pretty=format:"<string>"	-->	Permite mostrar cada commit como nos de la gana usando los mismos placeholders que usamos en el comando 'printf' de C
		
										Por ejemplo, en el siguiente comando:
										
											git log --pretty=format:"%cn committed %h on %cd"
											
										Los placeholders se sustituyen de la siguiente forma:
										
											%cn --> Autor del commit
											%h	--> Hash del commit
											%cd --> Fecha del commit
											
										La lista completa de placeholders se puede buscar en Internet.
		
										Este comando es muy útil cuando necesitas usar la salida del comando git log como entrada para otro programa (pipe)
										
		--pretty=oneline --> muestra un commit por línea
										
	
	Parámetros utilizados para filtrar que commits se incluyen en la salida:
	
		-NumCommits	--> Limita la salida al nº de comits indicado
		
		--all		--> Muestra todos los commits independientemente de la rama en la que estamos
		
		--after		--> Limita la salida a los commits anteriores o posteriores a la fecha indicada entre comillas dobles.
		--before		OJO: la fecha debe ir en formato yyyy-mm-dd
		
						p.ej.: git log --after="2019-12-01"
							   git log --before="yesterday"
							   
						Para buscar commits que creados en un periodo:
						
							   git log --after="2018-12-01" --before="2018-12-31"
							   
						Para buscar los commits de los últimos 3 días
						
								git log --after=3.days
							   
		--since		--> Son sinónimos de --after y --before respectivamente
		--until
		
		--author	--> Muestra únicamente los commits creados por el autor indicado entre comillas dobles
		
							git log --author="Daniel"
							
						El nombre del autor no tiene por qué coincidir excatamente. Vale con que contenga el texto indicado:
						
							git log --author="Trabas"
							
						Puedes utilizar expresiones regulares para crear búsquedas mas complejas:
						
							git log --author="Trabas\|Belmonte"		--> Muestra los commits creados por Daniel Trabas o David Belmonte
							
						Ten en cuenta que le email se guarda también con el nombre por lo que este comando te permite la búsqueda por email
						
							git log --author="dtrabas@calculo-sa.es"
							
						Se puede combinar con el parámetro -i para que git ignore mayúsculas/minúsculas
							
		
		--commiter	--> Sinónimo de --author
		
		--grep		--> Muestra únicamente los commits cuyo Mensaje coincida con el texto indicado entre comillas dobles. Funciona de manera similar
						a --author, es decir, basta con que el mensaje contenga eol texto indicado, se pueden utilizar expresiones regulares, etc...
						
							git log --grep "MEA-254"
							
		--			--> Muestra los commits en los que hubo cambios en el fichero o ficheros cuyo nombre se indica a continuación. Se pueden utilizar comodines
						para evitar tener que indicar el path completo del fichero.
		
		-S			--> Filtra por contenido del commit. 
		
						Por ejemplo, para buscar los commits que modificaron algún fichero *Query.xml añadiendo o quitando el texto "TRUNC" sin distinguir masyúsculas/minúsculas:
		
							 git log -S"trunc" -i -- *Query.xml
							 
		-G			--> Igual que -S pero indicando una expresión regular en lugar de una cadena de texto
		
		..			--> Permite filtrar commits en un rango de referencias (p.ej. commits, branchs, etc). 
		
						Por ejemplo, puedes filtrar los commits que hay entre una rama y otra así:
						
							git log develop..RELASE_3.1 --> NO CONSIGO QUE FUNCIONE...
							
		--no-merges	--> No tener en cuenta commits de mergeo
		
		--merges	--> Tener en cuenta únicamente commits de mergeo
		
	Ejemplos:
	
		. Buscar los commits del usuario cuyo nombre contiene la palabra 'trabas' (Daniel Trabas, dtrabas, etc...) en los que se incluyó el texto 'observaciones' en el mensaje del commit. No se distinguen mayúsculas/minúsculas (parámetro -i):
		
			git log -i --author "trabas" --grep "observaciones" --all
		
		. Mostrar todos los commits en los que el usuario indicado haya modificado el fichero tallerSiniestrosQuery.xml: 
		
			git log --author "Belmonte" -- *tallerSiniestrosQuery.xml
			
		. Útil para las Auditorías: mostrar todos los commits de e-Tica_web que generaron versión durante el año 2018:
		
			git --git-dir=e-Tica_web/.git --no-pager log --no-merges --grep "1.4-RC-" --after="2018-01-01" --before="2018-12-31"


* git shortlog

	Es una versión especial de 'git log'. Agrupa cada commit por Autor y muestra la primera línea del mensaje del commit. Es una forma fácil de ver quien ha estado trabajando en que.
	
	Por defecto ordena la salida por nombre de Autor. Con el parámetro -n la salida se ordenará por Número de Commits.

* git show --pretty="" --name-only  <ID_DEL_COMMIT>

	Muestra la lista de los ficheros que fueron modificados en el commit indicado
	

=============
   Remotes
=============	
	
* git remote add origin <url_repositorio_remoto>

	Añade el repositorio remoto indicado y le asigna el nombre origin
	
* git remote -v

	Muestra información de los respositorios remotos configurados para el proyecto actual
	
* git remote show <origin>

	Muestra información sobre el repositorio remoto <origin>
	
* git remote remove <nombre_repo>

	Elimina el repositorio 
	
* git remote rename <nombre_repo> <nombre_nuevo>

	Cambia el nombre del repositorio remoto
	
* git fetch <remote>

	Obtiene información de las ramas del repositorio <remote>
	
	El repositorio por defecto siempre es 'origin'
	
	Descarga la información nueva de estado que no tenemos en nuestro repositorio local (sólo estado, no descarga ficheros). Debemos ejecutarlo siempre antes de git status para estar seguros que estamos comparando el estado de nuestro repositorio local con el del remoto.
	
* git pull

	Hace un 'git fetch' seguido de un 'git merge' de la rama actual con la del respositorio remoto
	
* git push --set-upstream <remote> <branch>

	Sube la rama indicada al repositorio remoto. Por ejemplo:
	
		git push --set-upstream origin bug#IE-294
		
		ó
		
		git push -u origin bug#IE-294
	
=============
    Tags
=============	
	
* git tag [nombreNuevoTag]

	Muestra en orden alfabético los tags definidos ó crea un nuevo tag con el nombre indicado.
	
	Existen dos tipos de tags:
	
		. Lightweight: similar a una rama que no cambia. Básicamente es un puntero a un commit específico.
		
			git tag <nombre_tag>
		
		. Annotated: se guardan como full objects en la bbdd de Git. Tienen checksum, mensaje de tagging, email y fecha.
		
			git tag -a <nombre_tag> -m "mensaje de tagging"
			
* git show <nombre_tag>

	Muestra la información asociada al tag.
	
* git tag -l "patrón"

	Muestra los tags que cumplen el patrón indicado
	
* git tag -d <nombreTag>

	Elimina el tag especificado. No se elimina del remoto. 
	
	Para eliminarlo del repositorio remoto:
	
		git push origin :refs/tags/<nombreTag>
		
		ó
		
		git push origin --delete <nombreTag>

* git push origin <nombre_tag>

	Transfiere el tag al repositorio remoto
	
	OJO: 'git push' no transfiere por defecto tags al remoto. Hay que hacerlo explicitamente
	
* git push origin --tags

	Transfiere al repositorio todas las tags que no tenga ya
	
* git checkout <nombre_tag>

	Permite ver las versiones de los ficheros a los que apunta este tag. Pone el repositorio en estado 'detached HEAD'. En este estado hay que tener cuidado con los commits y el branching ya que funciona diferente. Mejor revisar la documentación...

=============	
  Branching
=============

	. Un commit consiste en una estructura de objetos formada por:
	
		(Usamos como ejemplo un directorio conteniendo tres archivos sobre los que hacemos stage y commit)
	
		1. Un Objeto principal del commit: contiene el nombre y email del autor, el mensaje empleado y un puntero a un objeto tree
		
		2. Un objeto tree: contiene el contenido del directorio y por cada archivo un puntero al objeto blob correspondiente
		
		3. Tres objetos blob (uno por archivo del directorio) con el contenido del archivo.
		
	. Al conjunto anterior lo llamamos Snapshot: cada vez que hacemos un commit se guarda un puntero al commit del que partió
	
	. Una rama es simplemente un puntero a un commit determinado
		
	. La rama por defecto en git es 'master'
	
* git branch <nombreRama>

	Crea una nueva rama apuntando al commit en el que estás situado (pero no cambia la rama sobre la que estás)
	
	¿ Como sabe Git la rama en la que estás ?: git mantiene un puntero especial llamado HEAD con el nombre de la rama actual (fichero ./.git/HEAD)
	
	Con este comando tienes una representación muy acertada de a donde apunta cada commit:
	
		git log --oneline --decorate
		
	Este otro muestra una referencia visual del branching:
	
		git log --oneline --decorate --graph --all
		
	Si no se indica un nombreRama, el comando muestra todas las ramas disponibles y marca con un * la rama actual.
	
	Otros parámetros útiles para el comando git branch son:
	
		-v	--> Muestra el último commit de cada rama
		
		--merged	-->	Muestra únicamente las ramas que han sido mergeadas.
		
						Si a continuación pones un nombreRama, el comando mostrará solo las ramas mergeadas en nombreRama.
		
		--no-merged	--> Sólo muestra las ramas pendientes de mergear. Una rama pendiente de mergear no puede ser eliminada mediante 'git branch -d', es 
						necesario hacerlo mediante el comando 'git branch -D'.
						
						Si a continuación pones un nombreRama, el comando mostrará solo las ramas pendientes de mergear en nombreRama.
	
	
	
* git checkout <nombreRama>

	Cambia la rama actual a la rama indicada
	
	Para crear automáticamente a la nueva rama, añade el parámetro -b
	
		git checkout -b <nombreRama>
		
* git merge <nombreRama>

	Mergea los cambios de la rama indicada sobre la rama actual.
	
	Se dice que se hace Fast-Forward cuando mergeas un commit con otro commit que puede ser alcanzado siguiendo la historia del primer commit. En otras palabras, el commit de la rama que vas a mergear viene directamente del commit de la rama en la que estás.
	
	El merge será Recursive cuando el commit que mergeamos no es un descendiente directo del commit sobre el que hacemos el merge. Git Crea un nuevo snapshot a partir de los snapshot de cada commit. En este caso, se permite indicar un mensaje con el parámetro -m 'Mensaje del merge...':
	
		git merge -m 'Mensaje del merge' <nombreRama>
		
		
* git branch

	Sin parámetros, lista todas las ramas LOCALES
	
	Con parámetros:
	
		-r 	-->	Lista sólo ramas REMOTAS
		
		-a	--> Lista TODAS las ramas
	
* git branch -d <nombreRama>

	Elimina la rama indicada
	
	Ojo: tienes que estar situado en una rama en la que se haya mergeado la rama que quieres borrar. En caso contrario, te sale el error 'error: The branch 'hotfix2' is not fully merged' y necesitas ejecutar el comando así 'git branch -D hotfix2' --> (-D = Force delete)
	
	Ojo2: cuando borras la rama, ésta deja de aparecer en el SourceTree...

	
=====================
  Deshaciendo cosas	
=====================

* Sacar el <file> del stage area

	git reset HEAD <file>

* Hace un discard de los cambios realizados en el <file> que todavía no están en el stage area 

	git checkout -- <file>
	
	Por ejemplo, cuando hacemos el merge de la rama RELEASE_X.Y sobre develop y nos da conflicto el pom.xml, para no tenerlo en cuenta tenemos que ejecutar los siguientes comandos:
	
		git reset HEAD pom.xml	
	    git checkout -- pom.xml
		
* Deshacer merge:

	git reset --hard HEAD
	
* Deshacer el último commit (deja los ficheros commiteados como unstaged)

	git reset HEAD~1

