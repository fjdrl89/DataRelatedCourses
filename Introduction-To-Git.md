# 1. Introduction to GIT

## Useful terminal comands

- `pwd` -> nos permite ver la ubicación del directorio que estamos utilizando.
- `ls` -> para ver que hay en el directorio actual
- `cd archive` -> si necesitamos cambiar al directorio "archive"
- `git --version` -> nos permite ver qué versión de GIT tenemos instalada
- `git init` -> nos permite convertir el directorio existente en un repositorio Git.
- `git init workspace-name` -> para crear un nuevo repoistorio GIT
- `cd wrokspace-name` -> para entrar al directorio que creaamos bajo dicho nombre 
- `git status` -> para comprobar que el repositorio se ha inicializado correctamente
- `git add` -> para rastrear los archivos creados usando Git

## Prepare and send files

- `git add` + `'file's-name.extension` -> Podemos agregar un solo archivo al área de preparación
- `git add .` -> agregar todos los archivos modificados en el directorio actual
- `git commit -m "Comment."` -> nos permite confirmar nuestro archivo borrador e incluir un mensaje

## Viewing the version history

- Git commits tiene 3 partes:
  * Commit: contiene la metadata - author, log message, commmit time.
  * Tree: rastrea los nombres y las ubicaciones de los archivos y directorios cuando se produjo esa confirmación; es como un diccionario, con claves representadas como identificadores únicos, asignados a archivos o directorios.
  * BLOB: para cada archivo en el arbol, hay un blob (Binary Large OBject). Puede contener datos de cualquier tipo. Contienen una instantanea comprimida del contenido de archivo cuando se produjo la confirmación (commit).
 
- Git hash -> es una cadena de 40 aracteres de numeros y letras
- `git log` ->  nos permite ver la información de confirmación, de la más reciente a la más antigua. Si aparecen ":" es porque hay más confirmaciones que no estamos visualizando por el límite de extensión, para verlas usamos la barra espaciadora. Para salir del log, basta con presionar la letra "q".

## Version history tips and tricks

Es una buena idea personalizar la salida del registro git, especialmente cuando estamos en un proyecto grande. Para ello, podemos por ejemplo:
- restringir la cantidad de confirmaciones mostradas, incluyendo un guión y el numero deseado:
```git
git log -3
```
- restringir la salida a un archivo específico. Para ello, incluimos el nombre del archivo:
```git
git log file-name.md
```
- aplicar ambas restricciones de forma simultanea:
```git
git log -2 file-name.md
```
- restringir la salida del registro por fecha añadiendo el indicador `--since = 'Month Day Year'`. Esta opción tiene múltiples variantes:
  * `git log --since= 'Month Day Year'` por ejemplo `git log --since= Apr 2 2024`.
  * `git log --since= 'Month Day Year' --until= 'Month Day Year'`, por ejemplo: `git log --since= 'Apr 2 2024' --until= 'Apr 11 2024`.
  * Esta opción acepta diversos formatos: `"2 weeks ago"`, `"3 months ago"`, `"yesterday"` en lenguaje natural o bien en los date formats más comunes como: `"07-15-2024"`, o en el ISO format 6801 `"YYYY-MM-DD"` to avoid sistem compatibility problems.
- también es posible ubicar un *commit* particular, para ello incluimos los primeros 8 a 10 caracteres del hash, al hacer esto obtenemos el registro en la parte superior, seguido de un *diff* que muestra los cambios entre el archivo en esa versión y la versión actual. Para ello hacemos:
```git
git log c27fa856
```

## Comparing Versions

Un *diff*, ejecutado usando `git diff` es la forma en que GIT nos muestra las diferencias existentes entre dos versiones. 

Entonces, si queremos por ejemplo comparar dos versiones de un cierto archivo `file.md`, es decir, comparar la última versión confirmada en nuestro informe con una versión modificada que no está en el área de preparacion (*staging area*) usamos: 
```git
git diff file.md
```

En el resultado, la versión "a" es la versión previa y la versión "b" es la más reciente. 

La línea que comienza con dos símbolos arroba @ nos indica que cambió entre las dos versiones. 

Por ejemplo, si consideramos el siguiente output como referencia:
<img width="885" alt="Screenshot 2025-06-13 at 10 16 33" src="https://github.com/user-attachments/assets/a830a00c-7578-45f5-9776-0fa87b369667" />

- El `-1` y el `5` indican que la versión A comienza en la línea 1 y tiene 5 líneas; los `+1` y `5` indican que de forma equivalente, la versión B comienza en la línea 1 y también contiene 5 líneas.
- El texto en rojo con el simbolo `-` al inicio, indica una línea en la versión A que no está en la versión B (más reciente).
- El texto en verde con el símbolo `+` al inicio, indica que agregamos una tarea en la versión B del informe, respecto a lo que teníamos en la versión A.

Si deseamos comparar la última *commited version* del file `file.md` con una versión existente en la *staging area* usamos:
```git
git diff --staged file.md
```
El reporte resultante será similar en estructura al que obtuvimos previamente. 

**Importante:** si por alguna razón olvidamos poner el nombre del archivo a comparar, obtendremos una comparación de todos los archivos preparados frente a sus últimas versiones confirmadas.

También es posible comparar versiones entre *commits*. Para ello, primero encontramos los *hashes* de confirmación de interés usando `git log`, y posteriormente ejecutamos:
```git
git diff 35f4b4d 186398f
```
Esto nos mostrará qué cambió desde la primera confirmación hasta la segunda. Considera colocar el *hash* de confirmación más reciente en la segunda posición del código.

Alternativamente, hay un atajo que podemos utilizar, la palabra `HEAD` se puede utilizar para referirse al *commit* más reciente. Para ver confirmaciones más antiguas, podemos hacer referencia a `HEAD` usando una tilde y luego un numero. Por ejemplo, si queremos comparar la segunda confirmación más reciente con la última confirmación usamos:
```git
git diff HEAD~1 HEAD
```

En resumen:

| **Command** | **Function** |
| --- | --- |
| `git diff` | Show changes between all unstaged files and the latest commit |
| `git diff file.md` | Show changes between an unstaged file and the latest commit |
| `git diff --staged` | Show changes between all staged files and the latest commit |
| `git diff --staged file.md` | Show changes between a staged file and the latest commit |
| `git diff 35f4b4d 186398f` | Show changes between two commits using hashes  |
| `git diff HEAD~1 HEAD` | Show changes between two commits using `HEAD` instead of commit *hashes* |

## Restoring and Reverting Files

Supongamos, por ejemplo, que hemos modificado un archivo, lo hemos agregado al área de preparación y hemos realizado una confirmación. Sin embargo, nos dimos cuenta que en la última versión tiene un error y necesitamos deshacer nuestros cambios más recientes restaurando este archivo al estado anterior a la última confirmación. 

`git revert ` -> Nos permite reestablecer los cambios anteriores y crear una nueva confirmación con estas versiones anteriores. Esto funciona con *commits*, no con archivos individuales. 

**Nota:** Esto restaurará todos los archivos actualizados en esa confirmación. Por lo que es necesario proporcionar una referencia a través de un *hash* de confirmación o del uso de `HEAD`. Por ejemplo:
```git
git revert HEAD
```

Esto abre un editor de texto donde se nos solicita que agreguemos un mensaje de confirmación. Podemos guardar haciendo `Ctrl + 0`seguido de `Enter` o salir usando `Ctrl + X`. Si queremos que no se abra el editor de texto, usamos:
```git
git revert --no-edit HEAD
```

Una vez cerrado el editor de texto o si éste no se abre, obtenemos un *terminal output* que confirma la reversión. 

También es posible revertir sin hacer inmediatamente un *commit* en ese punto, usamos:
```git
git revert -n HEAD
```

Como se mencionó previamente, el método hasta ahora descrito únicamente permite trabajar con *commits* y no con archivos individuales, por lo que, al ejecutar dicho código, si hubieramos actualizado más archivos, entonces estos también se habrían revertido. Si queremos evitar eso, y revertir un solo archivo, entonces usamos un comando diferente:
```git
git checkout
```

Este comando nos permitirá verificar uno o más archivos de una confirmación específica (usando nuevamente æa sintaxis `HEAD`), por ejemplo:
```git
git checkout HEAD~1 -- file.md
```

PAra restaurar un archivo en el área de prueba, antes de que se haya confirmado la actualización. Es decir, necesitamos eliminar este archivo del área de preparación para que vuelva al repositorio. Entonces, para *unstage* un *file* individual usamos:
```git
git restore --staged file.csv
```
Esto moverá el archivo nuevamente al directorio de trabajo, lo que nos permitirá editarlo antes de volver a prepararlo y confirmarlo. 

Alternativamente, para mover **todos** los archivos en el área de preparación al repositorio, ejecutamos:
```git
git restore --staged
```

Entonces, en resumen: 
| **Command** | **Result** |
| --- | --- |
| `git revert HEAD` | Revert all files from a given commit |
| `git revert HEAD --no-edit` | Revert without opening a text editor |
| `git revert HEAD -n` | Revert without making a new commit |
| `git checkout HEAD~1 -- file.extension` | Revert a single file from the previous commit |
| `git restore --staged file.extension` | Revert a single file from the staging area |
| `git restore --staged` | Remove all files from the staging area |

# 2. Introduction to GitHub Components

## 2.1 Introduction to GitHub

- **Git** es un software de control de versiones, por lo que se puede utilizar de forma independiente a GitHub u otras plataformas.
- **GitHub** por su parte, es una plataforma que mejora *Git* para facilitar la gestión de proyectos y la colaboración.

Recordemos que el **control de versiones** es el concepto de rastrear un archivo a través de sus diferentes estados. De este modo, podemos acceder al historial completo de cada etapa del proyecto. 










