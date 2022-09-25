# Curso de Git

`git --version`

`git help` OR `git --help`

`git help <commandName>` Ex: `git help commit` OR `git --help commit` It's the same help or --help

Note: to go out: `q`

# CONFIGURACIONES:

`git config --global user.name "Elis Antonio Perez"`

`git config --global user.email "elisperezmusic@gmail.com"`

`git config --global init.defaultBranch main`

`git config --global -e`: Muestra toda la configuración.

# Trabajando con repositorios:

`git init`: Inicia repositorio local.

`git add .`: Agrega todos los files al stage.

`git status`: Muestra los files en el stage.

`git reset <file>`: Remueve del stage el file especificado.

`git commit -m "any message"`: Crea el snapshot.

`git checkout -- .`: Reconstruye al ultimo commit.

`git branch`: Muestra las ramas.

`git branch -m master main`: Cambia el nombre de la rama master a main.

U: Untrack (sin seguimiento)
A: Added (Agregado al Stage)
M: Modified (Modificado)

Si el file está en "M" se puede usar el comando:

`git commit -am "any message"`: Agrega al stage y hace el snapshot en un solo paso pero solo con los archivos previamente añadidos al seguimiento. Los files "U" no son tomados en cuenta con este comando.

`git log`: Ver todos los commits