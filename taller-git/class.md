* Qué es git
    * Snapshots
    * Metadata
* Por qué es útil
    * Ver versiones antiguas
    * Trabajar en paralelo y en diferentes funcionalidades de un app
    * Quien escribió este modelo
    * Quien hizo esta linea, por qué, por quién
    * Ejemplo de test que ya no pasa
	* Binary Search ultima vez que un test pasó
* Qué se espera al final del taller
    * Conozcan el data modelo de diseño de git
    * Conozcan como los diferentes comandos se relacionan y flujo de trabajo
* Se comienza de abajo hacia arriba (del modelo de git hacia los comandos)
* Data Model
    * Trabajo tradicional
	* Cada uno hace una parte del código del proyecto, se zipea y se manda por mensaje wsp, correo y luego manualmente se unen copiando lineas de código de un proyecto a otro.
    * Directory structure
	* Root
	    * Foo "tree" (folder)
		* bar.txt "blob" (file)
	    * baz.txt "blob" (file)
    * Esquema recursivo (tree pueden contener otros tree y archivos)
    * History of snapshots
	* Explain git Directed Acyclic Graph
	* Branching and Merging
	    * Merge Conflicts
	* Metadata para cada snapshot
	    * Autor
	    * mensaje (commit)
    * Pseudo-código
	* type blob = array<byte>
	* type tree = map<string, tree | blob>
	* type commit = struct{
	    parents array<commits>
	    autor string
	    mensaje string
	    snapshot: tree
	  }
	* type object = blob | tree | commit
	* objects = map<string, object>
	* def store(object):
	    id = shal(object)
	    objects[id] = object
	* def load(id):
	    return objects[id]
    * Referencias "references"
	* Un nodo tendrá como referencia un id (string muy largo)
	* Son nombres para cada "snapshot"
	* Git mantiene un array the objectos (objects) y un array de referencias.
	* references = map<string, string> (human readable - id-hash)
	    * 'fix-bug'
	* El grafo de git es inmutable. Se puede agregar nuevos nodos, pero no modificar ya existentes. Sin embargo, las referencias son mutables.
	* Las referencias pueden apuntar a un nuevo nodo

* Git commands demo
    * `git init` iniciar un nuevo repositorio
    * `git status` ver qué pasa con el repositorio en cualquier momento
    * `git add` `git commit`
    * Staging area -> qué archivos|directorios se van a incluir en el siguiente snapshot (git rm --cached <file>)
    * `git cat-file -p <hash>`
    * `git commit -a` -> hace commit a todos los archivos que estaban en el anterior snapshot y han sido modificados
    * Ejemplos sobre git add y commit (por qué hay esa flexibilidad)
    * `git log` >> `git log --all --graph --decorate`
    * Agregar otra linea para ver git log graph
    * Referencias (master) -> pointer al commit
    * Referencias (HEAD) -> en qué parte del grafo estás ahora
    * `git checkout` -> moverse en el grafo (commits)
    * `git diff hash hash <file>`
    * `git checkout <file>`

* Project
    * Agregar un archivo (python)
    * git add commit
    * `git branch --options`
	* Sin ninguna opción, lista todos los branches
	* con un nombre, se crea un branch con ese nombre
    * `git branch cat` and `git checkout cat`
    * `git branch dog` and `git checkout dog` (`git checkout -b dog`)
    * `git merge` fast-forward
    * `git merge` CONFLICT (vimdiff) `git mergetool`

* Remotes
    * `git remote`
    * Agregar un repositorio remoto local `git init --bare`
    * Agregar repositorio remoto `git remote add <name> <url|path>`
    * Comandos para interactuar con el repositorio remoto
	* `git push <remote> <local branch>:<remote:branch>`
	* `git push origin master:master`
	* `git clone <url|path> <folder>`
    * Changes in other local with same remote
	* `git fetch <remote:name>` fetch toda la data que hay en el repositorio remoto
	* `git merge` | `git pull origin`

* Git config
* Git add interactive
    * `git add -p <file>` se puede hacer split con `s`, aceptar con `y` y saltar con `n`
    * `git diff --cached` muestra lo que está en el Staging Area
    * `git commit` para hacer commit a lo que se guardo
    * `git diff <file>` se va a ver que la linea de debuging no ha sido agregada
    * `git checkout <file>` para resetear las lineas cambiadas que no fueron agregados en el commit anterior

* Git Ignore
