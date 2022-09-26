# Curso de Git

- `git --version`

- `git help` OR `git --help`

- `git help <commandName>` Ex: `git help commit` OR `git --help commit` It's the same help or --help

Note: to go out: `q`

# CONFIGURACIONES:

- `git config --global user.name "Elis Antonio Perez"`

- `git config --global user.email "elisperezmusic@gmail.com"`

- `git config --global init.defaultBranch main`

- `git config --global -e`: Muestra toda la configuración.

# Trabajando con repositorios:

- `git init`: Inicia repositorio local.

## Stage

- `git add .`: Agrega todos los files al stage.

- `git add *.html`: Agrega solo los archivos con extension (HTML o cualquier otra especificada) al stage. Solo los que esten en la raiz. Sinó especificar la ruta `git add js/*.js`

- `git add css/`: Agrega al stage todos los archivos y subdirectorios del directorio (css/ o cualquiera especificado).

- Unstage (opción 1): `git reset <file>` Remueve del stage el file especificado.

- Unstage (opción 2): `git restore --staged <file>` Remueve del stage el file especificado.

## Status

- `git status`: Muestra los files en el stage.

- ??: Sin seguimiento.
- U: Untrack (sin seguimiento)
- A: Added (Agregado al Stage)
- M: Modified (Modificado)
- R: Rename (Se le cambió el nombre al archivo)
- D: Deleted

## Commits (Snapshots)

- `git commit -m "any message"`: Crea el snapshot.

- `git checkout -- .`: Reconstruye al ultimo commit. (Otra alternativa sería `git reset --hard`)

- `git branch`: Muestra las ramas.

- `git branch -m master main`: Cambia el nombre de la rama master a main.

Si el file tiene el status en "M" se puede usar el comando:

- `git commit -am "any message"`: Agrega al stage y hace el snapshot en un solo paso pero solo con los archivos previamente añadidos al seguimiento. Los files "U" no son tomados en cuenta con este comando.

- `git log`: Ver todos los commits

# Directorios

- `.gitkeep`: Git no hace seguimiento de los directorios vacios. Si se desea hacer seguimiento al directorio se le debe agregar el archivo `.gitkeep`. Ver ejemplo en la carpeta uploads.

# ALIAS

- `git config --global alias.<name> "<command and labels>"`: Crea un alias.

Ejemplo:

- `git config --global alias.s "status --short"`: Ahora con solo escribir `git s` ejecuta el comando `git status --short`

## Alias importantes

- Alias para Log: `git config --global alias.lg "log --graph --abbrev-commit --decorate --format=format:'%C(bold blue)%h%C(reset) - %C(bold green)(%ar)%C(reset) %C(white)%s%C(reset) %C(dim white)- %an%C(reset)%C(bold yellow)%d%C(reset)' --all"`

- Alias para Status: `git config --global alias.s status --short`

- Alternativa útil de alias para status: `git config --global alias.s status -sb`

# Trabajando con los Commits

- `git diff`: Muestra los cambios en los archivos. (No muestra nada si están en el stage)

- `git diff --staged`: Muestra los cambios en los archivos que ya están en el stage.

## Modificar mensaje del ultimo commit:

- `git commit --amend`: Abre editor (Nano) para modificar el mensaje.

OR:

- `git commit --amend -m "any message"`: Enmendar mensaje del commit directamente (Amend).

## Resetear cambios

- `git reset --soft HEAD^`: Nos mueve al commit antes de HEAD para que podamos agregar los nuevos cambios al ultimo commit. (Practicamente borra el ultimo commit para que creemos uno nuevo)

- `HEAD^2`: El número despues del ACENTO CIRCUNFLEJO (^) indica el número de commits antes del HEAD.

- Moverme a un commit especifico con el numero del hash del commit:

- `git reset --soft <hash>` Ejemplo `git reset --soft b9cd3b5` Nota: `git lg` para conseguir el número hash del commit.

- `git reset --mixed <hash>`: Similar al soft. Saca todo del stage. Conserva los nuevos cambios.

- `git reset --hard <hash>`: Destructivo. Deja todo como estaba en ese punto. Elimina todos los cambios.

# RECUPERAR

`git reflog`: Lista todos los cambios realizados con los comandos reset y commit. Muy útil para recuperar algun commit borrado con git reset --hard.

Simplemente copiar el hash del punto en que deseamos movernos y hacer el `git reset --hard <hash>`

# Mover archivos

- `git mv <file> <newFileName>`: Or PATH. En caso de querer cambiar el nombre al archivo.

Ejemplo: `git mv ressme.md README.md`: Cambia el nombre del archivo "ressme.md" a "README.md"

# Eliminar

`git rm <file>` or PATH.

# Ignorar archivos o carpetas

- Crear el archivo `.gitignore` y agregar dentro los PATH y files que se desean ignorar. (Ver ejemplo en el archivo .gitignore)

# RAMAS

- `git branch <name>`: Crea nueva rama.

- `git checkout <name>`: Cambia a la rama especificada.

- PREFERIDO: `git checkout -b <name>`: Crea nueva rama y nos cambia a ella (Dos pasos en uno 🤓)

## Eliminar rama

- `git branch -d <branch-name>`: Elimina la rama especificada.

- `git branch -d <branch-name> -f`: Si existen cambios sin fusionar (merge) te pide confirmación. Con la bandera "-f" forzamos la eliminacion de la rama.

# Merge

## Merge: Fast-Forward strategy (Fusionar: Estrategia de Avance Rápido):

- `git merge <branch-name>`: Fusiona la rama actual con la rama especificada.

Ejemplo: Fusionar los cambios de la rama "chat" en la rama "main"

```
elis@perezmusic ~/git-course (main) $ git branch chat // Crea la rama "chat"
elis@perezmusic ~/git-course (main) $ git merge chat // Fusiona la rama "chat" a la rama "main"
```

## Merge: Recursive strategy (Estrategia Recursiva) - Fusión Automática

- Al hacer el Merge nos pide el mensaje para el nuevo commit donde se hará la fusion de las dos ramas.
- Esto sucede cuando creas una nueva rama y haces cambios en ella, a su vez haces cambios en la rama main. Al fusionar si no hay ningun conflicto usa la estrategia recursiva para crear un nuevo commit con ambos cambios.

## Merge: Fusiones con conflictos.

- Al hacer el marge si aparece uno o varios conflictos hay que resolverlos todos y luego hacer un nuevo commit. (Los archivos que no presentaron conflictos hacen merge, solo los del conflicto no pasan).

Ejemplo:

1. `git merge chat` // Conflicto en el archivo README.md

2. Resolver el conflicto revisando el archivo README.md decidiendo que lineas dejas y qué quitar.

3. `git commit -am "conflicto resuelto"`

# Tags

Los tags son etiquetas para los commits.

- `git tag <name>`: Crea una etiqueta al commit actual.

- `git tag`: Listar los tags.

- `git tag -d <name>`: Elimina la etiqueta.

## Consideraciones para nombrar las etiquetas:

Se debe crear tags del tipo "versiones semanticas".

### Release tag:

- `git tag -a v1.0.0 -m "Version 1.0.0 ready"`: Crea etiqueta al commit actual.

- `git tag -a v0.1.0 <hash> -m "Version 0.1.0 ready"`: Crea etiqueta a un commit especifico por medio del hash.

`-a`: Annotated. `-m`: Message.

- `git show <tagName>`: Muestra toda la info de un tag (Listar los tags con `git tag` para encontrar el nombre)

# STASH

Es como una boveda donde se almacenan los cambios temporalmente.

- `git stash`: Guarda todos los cambios en una boveda y nos devuelve al ultimo commit.

Es como el `git checkout -- .` pero con la ventaja de que los cambios realizados no se pierden sino que pueden ser recuperados posteriormente.

WIP: Work In Progress.

- `git stash save "descriptive name"`: Permite agregar un stash con nombre personalizado (RECOMENDADO)

- `git stash list`: Muestra todos los stash.

- `git stash list --stat`: Muestra estadisticas de los stash.

- `git stash show stash@{<number>}`: Muestra el contenido de un stash especifico.

- `git stash pop`: Recupera el ultimo stash y hace marge con la rama actual. También borra ese stash.

- `git stash apply stash@{<number>}`: Aplicar un stash especifico (Ejemplo: `stash@{2}`)

APPLY no elimina el stash como el POP.

- `git stash drop`: Elimina el último stash.

- `git stash drop stash@{<number}`: Elimina un stash especifico.

- `git stash clear`: Borra todos los stash.

# REBASE

### Rebase normal

- elis@perezmusic ~/git-github-course (branch2) $ `git rebase <branch1>`: Estando en la rama "branch2" se hace el rebase a la "branch1" ¿Qué hace esto?

1. Coloca todos los commits de la "branch2" en un espacio temporal.
2. Hace merge de la rama "branch1" hacia la rama "branch2". (Se agregan todos los commits de la rama "branch1" a la rama "branch2")
3. Agrega los commit del espacio temporal a la rama "branch2" pero quedando estos commit por encima de todos los commit de la rama "branch1" que se fusionaron en el paso anterior. 🧐

### Rebase interactivo - SQUASH

Squash se usa para fusionar 2 commit en un nuevo commit (Reemplazando los 2 commits por el nuevo commit)

- `git rebase -i HEAD~<number>`: Entra en modo edición, el numero muestra una cantidad de commits antes del HEAD (incluyendo al HEAD). Seguir los siguientes pasos:

1. Identificar los 2 commits que se desean fusionar.

Ejemplo:

Se desean unir el commit "b8cd0e2" con el commit "0d052c9" porque fue tan minimo el codigo modificado que no es necesario tener 2 commits.

elis@perezmusic ~/Dev/git-github-course (main) $ `git rebase -i HEAD~4`

```
pick 49fc325 "my first commit"
pick 9f6aa51 "login agregado"
pick b8cd0e2 "landing page agregada"
pick 0d052c9 "detallito adicional a la landing page"
```

2. Reemplazar en el ultimo commit la palabra pick por "squash" o simplemente la "s" para usar la abreviación. Y guardar.

```
pick 49fc325 "my first commit"
pick 9f6aa51 "login agregado"
pick b8cd0e2 "landing page agregada"
s 0d052c9 "detallito adicional a la landing page"
```

3. Se abrirá otra pantalla de edición para agregar el mensaje al nuevo commit resultante de la fusion.

Agregamos el mensaje "landing page agregada + el detallito adicional" en la primera linea del documento y listo:

```
landing page agregada + el detallito adicional
# This is a combination of 2 commits.
# This is the 1st commit message:

landing page agregada

# This is the commit message #2:

detallito adicional a la landing page

# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# Date: Mon Sep 26 09:34:46 2022 -0500
#
# interactive rebase in progress;
# Last commands done (4 commands done):
#   pick b8cd0e2 landing page agregada
#   squash 0d052c9 detallito adicional a la landing page
# No commands remaining.
# You are currently rebasing branch 'main' on 'c9sj0e3'
#
# Changes to be committed:
```

### Rebase - REWORD

Reword se usa para editar el mensaje de un commit.

Ejemplo:

- `git rebase -i HEAD~4`: Entra en modo edición, el numero muestra una cantidad de commits antes del HEAD (incluyendo al HEAD). Seguir los siguientes pasos:

1. Identificar el commit que se desea modificar el mensaje.

```
pick 25s9dow "my first commit"
pick 03dj283 "segundo commit"
pick y8hc0e5 "tercer commit"
pick a8s9dk2 "quinto commit"
```

2. Reemplazar la palabra "pick" por "reword" o simplemente "r" y guardar.

```
pick 25s9dow "my first commit"
pick 03dj283 "segundo commit"
pick y8hc0e5 "tercer commit"
r a8s9dk2 "quinto commit"
```

3. Se abre el editor. Pulsar la tecla A para empezar a editar el mensaje.

Reemplazar "quinto commit"

```
quinto commit
# Please enter the commit message for your changes...
# ...
```

Por "cuarto commit" y guardar.

```
cuarto commit
# Please enter the commit message for your changes...
# ...
```

4. Verificar con `git lg` el nuevo mensaje del commit y Listo.

```
* a8s9dk2 - (2 hours ago) cuarto commit - Elis Antonio Perez (HEAD -> main)
* y8hc0e5 - (2 hours ago) tercer commit - Elis Antonio Perez
* 03dj283 - (2 hours ago) segundo commit - Elis Antonio Perez
* 25s9dow - (3 hours ago) my first commit - Elis Antonio Perez
```