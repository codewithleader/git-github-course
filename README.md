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

Para salir del editor en Mac: `Esc + :wq!`

# ALIAS

- `git config --global alias.<name> "<command and labels>"`: Crea un alias.

Ejemplo:

- `git config --global alias.s "status --short"`: Ahora con solo escribir `git s` ejecuta el comando `git status --short`

## Alias importantes

- Alias para Log: `git config --global alias.lg "log --graph --abbrev-commit --decorate --format=format:'%C(bold blue)%h%C(reset) - %C(bold green)(%ar)%C(reset) %C(white)%s%C(reset) %C(dim white)- %an%C(reset)%C(bold yellow)%d%C(reset)' --all"`

- Alias para Status: `git config --global alias.s status --short`

- Alternativa útil de alias para status: `git config --global alias.s status -sb`

# Trabajando con repositorios:

- `git init`: Inicia repositorio local.

## Stage

- `git add .`: Agrega todos los files al stage.

- `git add *.html`: Agrega solo los archivos con extension (HTML o cualquier otra especificada) al stage. Solo los que esten en la raiz. Sinó especificar la ruta `git add js/*.js`

- `git add css/`: Agrega al stage todos los archivos y subdirectorios del directorio (css/ o cualquiera especificado).

- Unstage (opción 1): `git reset <file>` Remueve del stage el file especificado.

- Unstage (opción 2): `git restore --staged <file>` Remueve del stage el file especificado.

## Seguimiento de Directorios vacíos

- `.gitkeep`: Git no hace seguimiento de los directorios vacios. Si se desea hacer seguimiento al directorio se le debe agregar el archivo `.gitkeep`. Ver ejemplo en la carpeta uploads.

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

Si el file tiene el status en "M" se puede usar el comando:

- `git commit -am "any message"`: Agrega al stage y hace el snapshot en un solo paso pero solo con los archivos previamente añadidos al seguimiento. Los files "U" no son tomados en cuenta con este comando.

- `git log`: Ver todos los commits

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

# Eliminar archivos y directorios

`git rm <file>` or PATH. Ejemplo: `git rm README.md` or `git rm js/app.js`

`git rm -r <directory>` Ejemplo: `git rm -r css/` eliminaría el directorio "css/" y todo su contenido.

# Ignorar archivos o carpetas

- Crear el archivo `.gitignore` y agregar dentro los PATH y files que se desean ignorar. (Ver ejemplo en el archivo .gitignore)

# RAMAS (Parte 1)

- `git branch <name>`: Crea nueva rama.

- `git checkout <name>`: Cambia a una rama o tag especificada.

- PREFERIDO: `git checkout -b <name>`: Crea nueva rama y nos cambia a ella (Dos pasos en uno 🤓)

- `git checkout -- .`: Reconstruye todos los archivos al ultimo commit.

- `git checkout -- <file>`: Reconstruye solo el archivo especificado al ultimo commit.

- `git branch`: Muestra las ramas locales.

- `git branch -a`: Muestra todas las ramas locales + las ramas remotas.

- `git branch -m master main`: Cambia el nombre de la rama "master" a "main".

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

- `git show <name>`: Muestra toda la info de un tag.

- `git tag -d <name>`: Elimina la tag.

## Consideraciones para nombrar las etiquetas:

Se debe crear tags del tipo "versiones semanticas".

### Release tag:

- `git tag -a v1.0.0alpha -m "Version 1.0.0 ready"`: Crea etiqueta al commit actual.

- `git tag -a v0.1.0 <hash> -m "Version 0.1.0 ready"`: Crea etiqueta a un commit especifico por medio del hash.

`-a`: Annotated. `-m`: Message.

## Subir los tags al repositorio remoto

- `git push --tags`: Sube todos los tags al repositorio remoto.

# STASH

Es como una boveda donde se almacenan los cambios temporalmente.

- `git stash save "descriptive name"`: Permite agregar un stash con nombre personalizado (RECOMENDADO)

- `git stash`: Guarda todos los cambios en una boveda y nos devuelve al ultimo commit (No Recomendado).

Es como el `git checkout -- .` pero con la ventaja de que los cambios realizados no se pierden sino que pueden ser recuperados posteriormente.

WIP: Work In Progress.

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

### Rebase - EDIT

Se usa para SEPARAR un commit.

`git rebase -i HEAD~2`

1. Identificar el commit que se desea hacer la separación y reemplazar "pick" por "edit" o simplemente "e"

```
pick 9ds9fo3 "my first commit"
e 3lkjd30 "file1 y file2"
```

2. No pasa nada mas pero es porque se ha entrado al modo de REBASE MANUAL "interactive rebase in progress" (Verificar con git status si se desea)

Para salir del modo REBASE MANUAL primero se deben hacer los commits que se desean hacer.

3. `git reset HEAD^`: Baja del stage el ultimo commit.

4. `git add <file1>` => `git commit -m "file1"`

5. `git add <file2>` => `git commit -m "file2"`

6. `git rebase --continue`: Termina el proceso y sale del REBASE.

7. `git lg` Veremos los nuevos cambios, el commit viejo ha sido separado en 2 nuevos commits:

```
* asdf0eh - (2 hours ago) file2 - Elis Antonio Perez (HEAD -> main)
* 63gj245 - (2 hours ago) file1 - Elis Antonio Perez
* 9ds9fo3 - (2 hours ago) my first commit - Elis Antonio Perez
```

# Repositorios REMOTOS

Subir repositorio local a repositorio remoto con REMOTE.

- `git remote add origin <url>`: Enlaza tu repositorio local con el remoto.

- `git branch -M main`: Forzar cambio de nombre de la rama a "main"

## PUSH

- `git push -u origin main`: Despliega todos los cambios de tu repositorio local al remoto.

La "-u" establece la rama main por defecto para los proximos `git push`

## Subir los tags

Los tags no se suben automaticamente despues de hacer el 'git push', hay que subirlos después con el comando:

- `git push --tags`: Sube todos los tags al repositorio remoto.

## PULL

- `git pull`: Si no hay conflicto jala todos los cambios del repositorio remoto al local.

Nota: En algunas versiones de GIT si es primera vez en hacer PULL aparecerá un mensaje de ADVERTENCIA indicando que debemos hacer una configuración adicional. PERO igualmente hace el pull si no hay conflictos. Ejecutar el siguiente comando para configurar el Fast Forward Only:

- `git config --global pull.ff only`: Fast Forward Only. (Cambiaremos esta config más adelante en la resolución de conflictos)

- `git pull --all`: trae tambien las ramas creadas por otros usuarios o ramas creadas desde GitHub.

## VER repositorios remotos

- `git remote -v`: Muestra una lista de todos los repositorios remotos enlazados a tu repositorio local.

## CLONAR

- `git clone <url>`: Clona el repositorio remoto a una copia local con toda la info del control de seguimiento git (todos los commits).

Si solo se descarga el ZIP no hará la descarga de la carpeta .git, entonces no tendrá la info de todos los commit.

# CONFLICTOS al hacer PULL

El problema:

```bash
elis@perezmusic ~/git-github-course (main) $ git pull
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 3 (delta 2), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), 706 bytes | 353.00 KiB/s, done.
From https://github.com/ElisPerez/git-github-course
   7326a42..3b867d3  main       -> origin/main
hint: You have divergent branches and need to specify how to reconcile them.
hint: You can do so by running one of the following commands sometime before
hint: your next pull:
hint:
hint:   git config pull.rebase false  # merge (the default strategy)
hint:   git config pull.rebase true   # rebase
hint:   git config pull.ff only       # fast-forward only
hint:
hint: You can replace "git config" with "git config --global" to set a default
hint: preference for all repositories. You can also pass --rebase, --no-rebase,
hint: or --ff-only on the command line to override the configured default per
hint: invocation.
fatal: Need to specify how to reconcile divergent branches.
```

## Resolver conflicto

La solucion será deshacer el commit en local y crear un nuevo commit con la fusion de ambos cambios usando el `REBASE INTERACTIVO`. El resultado al final será que ingresa al local el commit remoto sin cambios, se añade un nuevo commit con los cambios que se eligieron de los commits en conflicto y se sube al remoto el nuevo commit con los conflictos resueltos.

Pasos:

1. `git config --global pull.rebase true`: Configura el rebase al hacer el pull. Intenta hacer Fast Forward PERO si hay conflicto entra en REBASE INTERACTIVO.

2. `git pull`: Entrará en modo de resolución de conflictos y decidimos que cambios dejar.

Mostrará el siguiente mensaje indicando que debemos resolver todos los conflictos manualmente, hacer el commit y terminar el rebase.

```bash
elis@perezmusic ~/git-github-course (main) $ git pull
Auto-merging probando-rebase.md
CONFLICT (content): Merge conflict in probando-rebase.md
error: could not apply 89e3238... creando conflicto desde local
hint: Resolve all conflicts manually, mark them as resolved with
hint: "git add/rm <conflicted_files>", then run "git rebase --continue".
hint: You can instead skip this commit: run "git rebase --skip".
hint: To abort and get back to the state before "git rebase", run "git rebase --abort".
Could not apply 89e3238... creando conflicto desde local
```

3. Modificar cada archivo en conflicto y guardar cambios.

4. `git add .`: Agrega al stage los cambios elegidos.

5. `git commit -m "conflicto resuelto"`: Crea un nuevo commit.

6. `git rebase --continue`: para finalizar el REBASE.

7. Terminar con `git push`: para agregar el nuevo commit al remoto. (El commit del remoto que tuvo conflicto no es modificado sinó que queda debajo de este último)

```bash
elis@perezmusic ~/git-github-course (main) $ git lg
* 2a4d6d0 - (4 minutes ago) conflicto resuelto - Elis Antonio Perez (HEAD -> main, origin/main)
* 3b867d3 - (17 minutes ago) Conflicto desde GitHub creado - Elis Antonio Perez
```

Listo. 😱 🤯

# FETCH

- `git fetch`: Actualiza las referencias del remoto al local. PERO el HEAD del local sigue apuntuando al commit que tenía antes de hacer el FETCH.

Ejemplo: En local se tienen 3 commits y en remoto se añadieron 2 commits mas. Al hacer el FETCH entonces en local muestra los 5 commits pero el HEAD sigue apuntando al commit 3 ya que solo actualizó las referencias.

# FORK - Bifurcación.

Un Fork es una división del codigo a una nueva rama del tipo repositorio. Cada repositorio creado con Fork es una rama más del repositorio original.

# PULL REQUESTS (PR)

Consiste en hacer una solicitud de unión a la rama 'main'. Se usa cuando no tenemos total acceso a un repositorio.

1. FORK al repositorio que se desea contribuir para crear la rama (repositorio).

2. CLONE a local para realizar los cambios deseados desde VSCODE: `git clone <url>`

3. COMMITS and PUSH: Cuando terminemos de hacer los cambios guardamos, hacemos el commit y el PUSH para subir los cambios al FORK.

4. CONTRIBUTE: En el repositorio de GitHub (el FORK) buscamos el boton "contribute" y luego hacemos el OPEN PULL REQUEST.

5. COMPARING CHANGES: Verificar toda la info que se desea contribuir. En este punto si hay algo que nos falta, alguna linea de codigo que se nos olvidó, podemos ir a VSCODE, añadir los cambios faltantes, hacer el COMMIT, el PUSH y recargar la pagina del navegador de COMPARING CHANGES y veremos los nuevos commits añadidos.

6. CREATE PULL REQUEST: Se pedirá que añadas el mensaje para ese Pull Request.

7. Confirmar en CREATE PULL REQUEST.

8. En la ultima pantalla puedes CANCELAR el PULL REQUEST o añadir otros mensajes adicionales para que el dueño del repositorio original los vea.

# SOLICITUDES DE CAMBIO

- `git checkout <hash> <file>`: Revierte los cambios de un file al commit especificado. (no borra el commit, solo revierte los cambios a ese unico archivo)

- `git commit -am "modificaciones de solicitud realizadas"`: Crea un nuevo commit con los cambios.

- `git push`

# OBTENER LOS ULTIMOS CAMBIOS DEL REPOSITORIO ORIGINAL

Podemos agregar tantos repositorios remotos queramos a nuestro local.

- `git remote add upstream <url> main`: Enlaza tu repositorio local con el remoto.

```bash
elis@perezmusic ~/Dev/git-github-course (main) $ git remote -v
origin  https://github.com/ElisPerez/git-github-course.git (fetch)
origin  https://github.com/ElisPerez/git-github-course.git (push)
upstream  https://github.com/ElisPerez/otro-repo-remote.git (fetch)
upstream  https://github.com/ElisPerez/otro-repo-remote.git (push)
```

- Como es para solo lectura solo podremos usar el git FETCH y git PULL. No podrémos hacer git PUSH ya que no somos dueños de ese otro repositorio

# Purgar las ramas basura (ramas eliminadas en remoto y que aun están en local)

- `git remote prune origin`: Limpia las referencias locales.

# RAMAS (Parte 2)

## Rama de Producción

1. `git checkout -b <name>`: Crea una nueva rama.

2. `git push --set-upstream origin <name>`: Sube al remoto la nueva rama creada. Truco: hacer el git PUSH para mostrar el mensaje de error y el comando sugerido.

3. `git tag -a v1.0.0 -m "NombreRama v1.0.0 - Stable`: Crea el tag para esa rama.

- "-a": Annotated.
- "v1.0.0": El numero de version. Puede lucir como v1.0.0-alpha también.
- "-m": Message.

4. `git push --tags`: Sube el tag al remoto.

5. En GitHub editar el tag para crear el RELEASE TAG.

# GITHUB - Cosas de GITHUB

# ISSUES: Problemas e inquietudes.

Un issue es un tema o problema importante para el debate o la discusión.

Los Issues integran el seguimiento de tareas ligeras en su repositorio. Mantenga los proyectos al día con etiquetas de Issues e Milestones, y haga referencia a ellos en los mensajes de commits.

## Uso de issues

- Los issues son usados mayormente para hacer preguntas o solicitar `features` a nuestra aplicación o a la de alguien más.

Ejemplo: Estamos por iniciar una nueva app en la ultima version de REACT JS y notamos un problema al hacer la instalación, nos dirijimos al repositorio oficial de React en GitHub, click en la pestaña ISSUES, procedemos a escribir el mensaje del issue comentando cual es el fallo con los detalles que creamos pertinentes, elegimos un `label` adecuado con nuestro problema y CREAMOS EL ISSUE. Los muchachos de React reciben un mensaje, reparan en dos minutos el problema, lanzan una nueva version, te dan las gracias por tu aporte y te contratan para una nueva version del software. 😎

- Etiqueta problemas y solicita cambios a tu equipo o nuevos colaboradores, GitHub ayudará a los posibles contribuyentes a descubrir problemas con un buen `label`.

## Labels de issues

Etiquetas comunes:

- `bug`: Algo no funciona.

- `documentation`: Mejoras o adiciones a la documentación.

- `duplicate`: Este problema o solicitud de extracción ya existe.

- `enhancement`: Nueva función o solicitud.

- `good first issue`: Bueno para los recién llegados.

- `help wanted`: Se necesita atención adicional.

- `invalid`: Esto no parece correcto.

- `question`: Se solicita más información.

- `wontfix`: Esto no se trabajará.

- Sientase libre de crear más etiquetas...

## Referencia de los ISSUES

- GitHub lleva el control de los issues por nosotros asignandole un `#` más un numero único a cada issue. El número asignado nunca cambiará. 

Ejemplo: Si creamos 3 issues (serian #1, #2 y #3) y eliminamos el segundo issue (#2), al crear un nuevo issue no tomará el numero #2 ya que este hace referencia al issue que fue borrado, entonces el nuevo issue tomaría el #4 y quedarian asi: #1, #3, #4. Dejando en claro que el #2 eliminado, no se le asignará a ningun otro issue en toda la historia de nuestro repositorio.

- Estas referencias podemos usarlas al hacer comentarios para que se cree un enlace hacia ese issue en particular.

Ejemplo de comentario: "@ElisPerez por favor revisa los issues #3 y #7, y realiza los cambios respectivos"

## CERRAR ISSUE MEDIANTE UN COMMIT 🤯

- `git commit -am "Fixes #1: Reparado el problema de este issue"`: En el mensaje del commit podemos agregar la PALABRA CLAVE `Fixes` seguido del número de referencia al issue para hacer el cierre del issue automáticamente.

### PALABRAS CLAVES <!-- TODO: Falta el comentario a cada una -->

- Close: <!-- TODO: -->
- Closes: <!-- TODO: -->
- Closed: <!-- TODO: -->
- Fix: <!-- TODO: -->
- Fixes: <!-- TODO: -->
- Fixed: <!-- TODO: -->
- Resolve: <!-- TODO: -->
- Resolves: <!-- TODO: -->
- Resolved: <!-- TODO: -->

Más info en la doc oficial: [Link](https://docs.github.com/es/issues/tracking-your-work-with-issues/linking-a-pull-request-to-an-issue#linking-a-pull-request-to-an-issue-using-a-keyword)

## ISSUES TEMPLATE

GitHub nos permite crear una plantilla personalizada para que cuando alguien desee hacer un issue tenga la opción de elegir la plantilla que le cargará todo un formulario que debe llenar con la info que nosotros deseamos que nos aporte para poder darle solución a ese issue.

- Ir a configuraciones -> Caracteristicas -> Issues -> Set Up Template.
- Elegir una plantilla: Bug Report, Feature Request ó Custom Template.
- Editar plantilla -> Propose Changes -> Commit changes.

Nota: En la edición de la plantilla podemos agregar una `label` por defecto (RECOMENDADO)

# MILESTONES: Hitos.

Un mileston es una acción o evento que marca un cambio significativo o una etapa importante en el desarrollo de nuestra aplicación.

## Uso de milestones

Use `Milestones` para `CREAR COLECCIONES DE ISSUES Y PULL REQUESTS` para un lanzamiento o proyecto en particular.

Ejemplo:

- Se espera tener lista la nueva version de año nuevo para 1 enero del 2023.
- Se crea el Milestone "Lanzamiento enero 2023". Cada issue o PR que tenga que ver con ese acontecimiento se asigna a ese Milestone.
- Se crea el issue "implementar dark theme" y en el panel izquierdo buscamos la ficha Milestones y seleccionamos "Lanzamiento enero 2023".
- ¡Ya tenemos nuestro primer issue para trabajar y solucionar antes de hacer el lanzamiento!.

# GitHub Wiki

Permite crear paginas para la documentación del repositorio.

# GitHub Pages

Permite desplegar tu proyecto en un servidor gratuito.

- Eliges que rama usar y desplegará tu repositorio. Genera un link que puedes compartir a tus amigos.
- Solo funciona con archivos HTML, css, Javascript. NodeJS, Java y otros no son soportados.

# GitHub Project

Los proyectos en GitHub lo ayudan a organizar y priorizar su trabajo. Puede crear proyectos para trabajos de características específicas, hojas de ruta integrales o incluso listas de verificación de lanzamiento