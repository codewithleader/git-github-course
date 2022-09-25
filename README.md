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

`git add *.html`: Agrega solo los archivos con extension (HTML o cualquier otra especificada) al stage. Solo los que esten en la raiz. Sinó especificar la ruta `git add js/*.js`

`git add css/`: Agrega al stage todos los archivos y subdirectorios del directorio (css/ o cualquiera especificado).

`git status`: Muestra los files en el stage.

Unstage (opción 1): `git reset <file>` Remueve del stage el file especificado.

Unstage (opción 2): `git restore --staged <file>` Remueve del stage el file especificado.

`git commit -m "any message"`: Crea el snapshot.

`git checkout -- .`: Reconstruye al ultimo commit. (Otra alternativa sería `git reset --hard`)

`git branch`: Muestra las ramas.

`git branch -m master main`: Cambia el nombre de la rama master a main.

U: Untrack (sin seguimiento)
A: Added (Agregado al Stage)
M: Modified (Modificado)
R: Rename (Se le cambió el nombre al archivo)
D: Deleted

Si el file está en "M" se puede usar el comando:

`git commit -am "any message"`: Agrega al stage y hace el snapshot en un solo paso pero solo con los archivos previamente añadidos al seguimiento. Los files "U" no son tomados en cuenta con este comando.

`git log`: Ver todos los commits

- Git no hace seguimiento de los directorios vacios. Si se desea hacer seguimiento al directorio se le debe agregar el archivo `.gitkeep`. Ver ejemplo en la carpeta uploads.

# Crear Alias

`git config --global alias.<name> "<command and labels>"`.

Ejemplo:

- `git config --global alias.s "status --short"`: Ahora con solo escribir `git s` ejecuta el comando `git status --short`

## Alias importantes
### Log
- `git config --global alias.lg "log --graph --abbrev-commit --decorate --format=format:'%C(bold blue)%h%C(reset) - %C(bold green)(%ar)%C(reset) %C(white)%s%C(reset) %C(dim white)- %an%C(reset)%C(bold yellow)%d%C(reset)' --all"`

### Status
`git config --global alias.s status --short`

### Alternativa útil de status
`git config --global alias.s status -sb`

# Commits

`git diff`: Muestra los cambios en los archivos. (No muestra nada si están en el stage)

`git diff --staged`: Muestra los cambios en los archivos que ya están en el stage.

- Modificar mensaje del ultimo commit:

`git commit --amend`: Abre editor (Nano) para modificar el mensaje.

OR:

`git commit --amend -m "any message"`: Enmendar mensaje del commit directamente (Amend).

- `git reset --soft HEAD^`: Nos mueve al commit antes de HEAD para que podamos agregar los nuevos cambios al ultimo commit. (Practicamente borra el ultimo commit para que creemos uno nuevo)

- `HEAD^2`: El número despues del acento circunflejo indica el número de commits antes del HEAD.

- Moverme a un commit especifico con el numero del hash del commit:

`git reset --soft <hash>` Ejemplo `git reset --soft b9cd3b5` Nota: `git lg` para conseguir el número hash del commit.

- `git reset --mixed <hash>`: Similar al soft. Saca todo del stage. Conserva los nuevos cambios.

- `git reset --hard <hash>`: Destructivo. Deja todo como estaba en ese punto. Elimina todos los cambios.

# Recuperar

`git reflog`: Lista todos los cambios realizados con los comandos reset y commit. Muy útil para recuperar algun commit borrado con git reset --hard.

Simplemente copiar el hash del punto en que deseamos movernos y hacer el `git reset --hard <hash>`

# Mover archivos

- `git mv <file> <newFileName>`: En caso de querer cambiar el nombre al archivo.

Ejemplo: `git mv ressme.md README.md`: Cambia el nombre del archivo "ressme.md" a "README.md"

# Eliminar

`git rm <file>`