# Guía Completa de Comandos Git

Esta guía recopila los comandos fundamentales para manejar el control de versiones con Git, organizados según el flujo de trabajo habitual de cualquier proyecto de desarrollo de software.

---

## 1. Configuración Inicial
Comandos necesarios para configurar tu entorno por primera vez. Establecen la identidad del autor en los *commits*.

* `git config --global user.name "Tu Nombre"`: Asigna el nombre que se mostrará en todos tus commits.
* `git config --global user.email "tu_email@ejemplo.com"`: Asigna el correo vinculado a tus commits. Debe coincidir con el de tu cuenta de GitHub/GitLab.
* `git config --global init.defaultBranch main`: Configura `main` como el nombre predeterminado al inicializar nuevos repositorios.
* `git config --list`: Imprime todas las variables de configuración activas en tu sistema.

---

## 2. Inicialización y Obtención
Pasos para crear un repositorio desde cero o descargar uno existente.

* `git init`: Inicializa un repositorio Git vacío dentro de la carpeta actual (crea la carpeta oculta `.git`).
* `git clone <url>`: Descarga una copia exacta y completa de un repositorio remoto en tu computadora y lo vincula automáticamente al origen.

---

## 3. Ciclo Básico de Confirmación (Staging & Commits)
El flujo del día a día: revisar, preparar y confirmar cambios en el historial.

* `git status`: Muestra el estado actual del repositorio (archivos modificados, listos para confirmar o no rastreados).
* `git add <archivo>`: Prepara un archivo específico para el próximo commit (lo envía al *staging area*).
* `git add .`: Prepara **todos** los archivos nuevos y modificados del directorio actual.
* `git commit -m "Mensaje"`: Registra una instantánea de los cambios preparados de forma permanente, junto con un mensaje explicativo.
* `git commit --amend -m "Nuevo mensaje"`: Permite modificar el último commit realizado. Muy útil si te faltó incluir un archivo o quieres cambiar el mensaje.

---

## 4. Sincronización Remota (Remote)
Intercambio de código entre tu computadora y un servidor como GitHub.

* `git remote -v`: Muestra los servidores remotos configurados y sus URLs.
* `git remote add <nombre> <url>`: Añade una nueva conexión a un repositorio remoto (por lo general, el nombre asignado es `origin`).
* `git fetch <remoto>`: Descarga la información y los cambios del servidor a tu repositorio local, pero **no** modifica tus archivos de trabajo.
* `git pull <remoto> <rama>`: Descarga los cambios del servidor y los fusiona automáticamente con tu rama actual. (Equivale a `fetch` + `merge`).
* `git push <remoto> <rama>`: Sube tus commits locales a la rama correspondiente en el servidor remoto.
* `git push -u origin <rama>`: Sube los cambios y vincula tu rama local con la remota, permitiendo que en el futuro solo necesites escribir `git push`.

---

## 5. Gestión de Ramas (Branches)
Las ramas permiten desarrollar nuevas funcionalidades o arreglar errores sin afectar el código principal de producción.

* `git branch`: Lista todas tus ramas locales. Un asterisco `*` indica la rama actual.
* `git branch -a`: Muestra todas las ramas, tanto locales como remotas.
* `git branch <nombre>`: Crea una nueva rama, pero te mantiene en la actual.
* `git switch <nombre>` (o `git checkout <nombre>`): Cambia tu entorno de trabajo a la rama especificada.
* `git switch -c <nombre>` (o `git checkout -b <nombre>`): Crea una nueva rama y te cambia a ella en un solo paso.
* `git merge <rama>`: Fusiona el historial y los cambios de la `<rama>` indicada hacia la rama en la que te encuentras posicionado.
* `git branch -d <nombre>`: Elimina una rama local, siempre y cuando ya haya sido fusionada.
* `git branch -D <nombre>`: Fuerza la eliminación de una rama local, perdiendo los cambios que no se hayan fusionado.

---

## 6. Inspección de Historial y Cambios
Herramientas para auditar qué pasó, cuándo y quién lo hizo.

* `git log`: Muestra el historial cronológico de los commits realizados.
* `git log --oneline --graph --all`: Muestra un árbol visual y compacto de todo el historial del proyecto en todas sus ramas.
* `git diff`: Muestra las diferencias exactas, línea por línea, de los archivos que editaste pero aún no están preparados.
* `git diff --staged`: Muestra las diferencias de los archivos que ya están en el *staging area* en comparación con el último commit.
* `git show <commit-hash>`: Expone los detalles y cambios exactos de un commit específico.

---

## 7. Deshacer Cambios y Trabajo Temporal
Para salvarte de errores, guardar cosas por un rato o retroceder en el tiempo.

* `git stash`: Guarda todos tus cambios no confirmados en un espacio temporal, devolviendo los archivos al estado de tu último commit (útil para hacer un `pull` o cambiar de rama rápidamente).
* `git stash pop`: Recupera los últimos cambios guardados temporalmente y los aplica a tus archivos actuales.
* `git stash list`: Muestra la lista de todos tus guardados temporales.
* `git restore <archivo>`: Descarta los cambios locales no preparados de un archivo, devolviéndolo a como estaba en el último commit.
* `git restore --staged <archivo>`: Quita un archivo del *staging area* (cancela el `git add`), pero mantiene tus modificaciones intactas.
* `git revert <commit-hash>`: Crea un **nuevo** commit que deshace los cambios exactos de un commit anterior, manteniendo intacto el historial. (Forma segura de deshacer errores).
* `git reset --soft HEAD~1`: Borra el último commit, pero deja tus modificaciones listas en el *staging area* para reacomodarlas.
* `git reset --hard HEAD~1`: **Cuidado:** Borra el último commit y destruye irreversiblemente todas las modificaciones asociadas a él.