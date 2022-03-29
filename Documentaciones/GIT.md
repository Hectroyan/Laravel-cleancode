# Libreto GIT

## 1. Configurar herramienta

 - `` $ git config --global user.name "[name]" ``                            
 Establece el nombre que desea esté anexado a sus transacciones  de commit 
 - `` $ git config --global user.email "[email address]" ``                  
 Establece el e-mail que desea esté anexado a sus transacciones de commit  
 - ``$ git config --global color.ui auto``                                   
 Habilita la útil colorización del producto de la línea de comando         

[🔝Volver al inicio](#libreto-git)
## 2. Crear repositorios

- ``$ git init [project-name] ``
Crear un nuevo repositorio local con el nombre x
- ``$ git clone [url]``
Descargas el proyecto y su historico de versiones

[🔝Volver al inicio](#libreto-git)
## 3. Ejecutar cambios
- ``$ git status``
Enumera todos los archivos nuevos o modificados que se deben confirmar
- ``$ git diff``
Muestra las diferencias de archivos que no se han enviado aún al
área de espera
- ``$ git add [file]``
Toma una instantánea del archivo para preparar la versión
- ``$ git diff --staged``
Muestra las diferencias del archivo entre el área de espera y la última
versión del archivo
- ``$ git reset [file]``
Mueve el archivo del área de espera, pero preserva su contenido
- ``$ git commit -m [mensaje descriptivo]``
Registra las instantáneas del archivo permanentemente en
el historial de versión, el mensaje debe seguir [estas reglas](#conventional-commits) a poder ser 
[🔝Volver al inicio](#libreto-git)

## 4. Cambios grupales

- ``$ git branch``
Enumera todas las ramas creadas
- ``$ git branch [branch-nombre]``
Crea una rama con este nombre a poder ser seguir [estas reglas](#ramas)
- ``$ git checkout [branch-nombre]``
Cambia a la rama especificada y actualiza el directorio activo
- ``$ git merge [branch-nombre]``
Combina el historial de la rama especificada con la actual
- ``$ git branch -d [branch-nombre]``
Borra la rama especificada

[🔝Volver al inicio](#libreto-git)

## 5. Nombres del archivo de refactorización

- ``$ git rm [file]``
Borra el archivo del directorio activo y lo pone en el area de espera
- ``$ git rm --cached [file]``
Retira el archivo del control de versiones pero lo preserva a nivel local
- ``$ git mv [file-original] [file-renamed]``
Cambia el nombre al archivo y lo prepara para el commit

[🔝Volver al inicio](#libreto-git)
## 6. Suprimir tracking

``` gitignore
*.log
build/
temp-*
```
Un archivo de texto llamado .gitignore suprime la creación accidental de versiones de archivos y rutas que concuerdan con los patrones
especificados
``$ git ls-files --other --ignored --exclude-standard``
Enumera todos los archivos ignorados en este proyecto

[🔝Volver al inicio](#libreto-git)
## 5. Gurardar fragmentos

- ``$ git stash``
Almacena temporalmente todos los archivos tracked modificados
- ``$ git stash pop``
Restaura los archivos guardados más recientemente
- ``$ git stash list``
Enumera todos los sets de cambios en guardado rápido
- ``$ git stash drop``
Elimina el set de cambios en guardado rápido más reciente

[🔝Volver al inicio](#libreto-git)
## 5. Repasar historial

- ``$ git log``
Enumera el historial de la versión para la rama actual
- ``$ git log --follow [file]``
Enumera el historial de versión para el archivo, incluidos los cambios de nombre
- ``$ git diff [first-branch]...[second-branch]``
Muestra las diferencias de contenido entre dos ramas
- ``$ git show [commit]``
Produce metadatos y cambios de contenido del commit especificado

[🔝Volver al inicio](#libreto-git)
## 6. Rehacer commits

- ``$ git reset [commit] ``
Deshace todos los commits después de [commit], preservando los cambios localmente 
- ``$ git merge [bookmark]/[branch]``
Desecha todo el historial y regresa al commit especificado

[🔝Volver al inicio](#libreto-git)
## 7. Sincronizar cambios

- ``$ git fetch [bookmark]``
Descarga todo el historial del marcador del repositorio
- ``$ git merge [bookmark]/[branch]``
Combina la rama del marcador con la rama local actual
- ``$ git push [alias] [branch]``
Carga todos los commits de la rama local al GitHub
- ``$ git pull``
Descarga el historial del marcador e incorpora cambios

[🔝Volver al inicio](#libreto-git)

## 8. Etiquetado
Seguir [estas convenciones](#etiquetas) dentro de lo posible
- `` $ git tag ``
Muestra todas las etiquetas
- `` $ git show [v1.4] ``
Muestra la info de la etiqueta especificada
- `` $ git tag -a v1.4 -m 'my version 1.4' ``
Crea una etiqueta con el nombre y el mensaje especificado

[🔝Volver al inicio](#libreto-git)

## Conventional commits

Por que usarlos:

- Generacion automatica de GITLOGs
- determinar las tareas semanticamente a partir del tipo
- Comunicar la naturaleza de los commits a tus compañeros etc.
- Hacer facil a otros miembros de el equipo o personas a contribuir en tu proyecto.
### Etiquetas

En cuanto al naming de las etiquetas seguira la siguiente convención:
- Seguiran un orden establecido por el equipo:
Generalmente será v0.0.1 dependiendo del versionado que se quiera usar
- Cuando se cree se creará con mensaje y debe ser descriptivo
ej. 
  - 1.0.0 version estable de la primera fase 
  - 1.0.1 Implementacion de logs de aplicación
  - 1.0.2 Creacion de la funcionalidad horarios
- Siempre se creará una etiqueta cuando se hayan pasado todos los test y sea 100% estable

### Ramas

Se evitara poner el nombre del usuario que este desarrollando, ya que el commit ya lleva esa informacion 
y es una duplicidad innecesaria, en caso de que no se quiera saturar el repositorio con naming de tarea
al menos se usará el nombre de la funcionalidad que se esta realizando

<b style="color:red">Mal</b>
```` git
    git branch desarrollos-horse-manuel
````
<b style="color:blue">Mejor</b>
```` git
    git branch authentication
````
<b style="color:green">Bien</b>

```` git
    git branch [nombre-tarea]
````

### Commits

Siempre seguir las siguientes convenciones:
- Nunca poner un mensaje sin ningun contexto
- A poder ser se escribirá en ingles
- El mensaje siempre empieza por el tipo de tarea ejecutado
  - fix:
  - feat:
  - docs:
  - test:
  - refactor:
  - style:
  - BREAKING CHANGE:
- Antes de los ":" puedes añadir a que parte de la aplicacion va entre ()
`` feat(api): ``
- Luego se añadirá el mensaje descriptivo necesario (empezando por un verbo)
`` feat(api): add friendly routes ``
- Puedes añadir un body y un footer en caso de querer especificar mas el commit.


<b style="color:red">Mal</b>
```` git
    He creado una cosa de autenticacion
````
<b style="color:blue">Mejor</b>
```` git
    feat: add authentication to api
````
<b style="color:green">Bien</b>
```` git
    feat(api): add authentication
````
<b style="color:grey">Opcionales</b>
```` git
    feat(api): add authentication
    
    Controllers invokables created for every action required for the authentication

    New routes added to the file
````
