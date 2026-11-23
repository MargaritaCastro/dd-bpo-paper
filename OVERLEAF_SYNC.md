# Sincronizar las carpetas de Overleaf

Este repo tiene dos carpetas conectadas a proyectos de Overleaf con `git subtree`.
GitHub (`origin`) guarda el repo completo y cada proyecto de Overleaf guarda solo el contenido de su carpeta.
La rama de Overleaf es `main` (no `master`).

| Carpeta | Remoto | URL Git de Overleaf |
|---|---|---|
| `paper-revision1/` | `overleaf` | `https://git@git.overleaf.com/6af721e23449250387c8a07d` |
| `response-letter/` | `overleaf-response` | `https://git@git.overleaf.com/6b048f53e8844ae329cec56c` |

- Repo de GitHub: `git@github.com:MargaritaCastro/dd-bpo-paper.git`

> **Importante:** ambas carpetas se conectaron con `--squash`, así que **todos** los comandos `subtree pull` y `subtree push` deben llevar `--squash`. Mezclar comandos con y sin `--squash` produce errores o conflictos falsos.

## Flujo de trabajo diario

Ejecuta todo desde la raíz del repo (`dd-bpo-paper/`).

### 1. Al empezar: traer lo último de GitHub y de ambos proyectos de Overleaf

```bash
git pull origin main
git subtree pull --prefix=paper-revision1 overleaf main --squash -m "Sync paper from Overleaf"
git subtree pull --prefix=response-letter overleaf-response main --squash -m "Sync response letter from Overleaf"
```

### 2. Editar localmente y hacer commit

```bash
git add -A
git commit -m "Describe tus cambios"
```

### 3. Al terminar: enviar a Overleaf y luego a GitHub

Haz push solo de la carpeta que cambiaste (o de ambas):

```bash
# Si cambiaste el paper
git subtree push --prefix=paper-revision1 --rejoin --squash overleaf main

# Si cambiaste la carta de respuesta
git subtree push --prefix=response-letter --rejoin --squash overleaf-response main

# Siempre al final
git push origin main
```

Si solo editaste en Overleaf (desde el navegador), basta con el paso 1 y luego `git push origin main` para que GitHub quede al día.

Reglas:

- Siempre haz `pull` desde Overleaf antes de hacer `push` a Overleaf.
- Cada carpeta va con su propio remoto: `paper-revision1` ↔ `overleaf`, `response-letter` ↔ `overleaf-response`. No los cruces.
- Siempre usa `--rejoin` al hacer push a Overleaf. Sin eso, los pulls posteriores pueden generar conflictos falsos.
- Siempre usa `--squash` en `subtree pull` y `subtree push`.
- Nunca uses `--force`: Overleaf lo rechaza.
- `git subtree` necesita un árbol de trabajo limpio. Haz commit o `stash` de tus cambios antes.
- Los cambios fuera de las carpetas de Overleaf (por ejemplo, este archivo) solo van a GitHub; con `git push origin main` es suficiente.

## Configuración en un computador nuevo

```bash
git clone git@github.com:MargaritaCastro/dd-bpo-paper.git
cd dd-bpo-paper
git remote add overleaf https://git@git.overleaf.com/6af721e23449250387c8a07d
git remote add overleaf-response https://git@git.overleaf.com/6b048f53e8844ae329cec56c
git subtree -h          # verifica que git subtree esté instalado (si no: brew install git)
git subtree pull --prefix=paper-revision1 overleaf main --squash -m "Sync paper from Overleaf"
git subtree pull --prefix=response-letter overleaf-response main --squash -m "Sync response letter from Overleaf"
```

- Cuando Git pida contraseña, usa tu **token de Git de Overleaf**, no tu contraseña normal. El usuario es siempre `git`. El mismo token sirve para todos tus proyectos de Overleaf.
- Los tokens se gestionan en Overleaf → Account Settings → Git integration.
- En macOS, el Llavero (Keychain) guarda el token. En otros sistemas, ejecuta primero `git config --global credential.helper store` (ojo: guarda el token en un archivo de texto plano).
- Antes de cambiar de computador, ejecuta `git push origin main` en el anterior.

## Agregar un nuevo proyecto de Overleaf

Pasos para conectar otro proyecto de Overleaf como carpeta del repo. Así se agregaron `paper-revision1` y `response-letter`.

La URL Git del proyecto está en Overleaf → menú del proyecto → Integrations → Git.

```bash
# 0. Partir con el repo al día y el árbol limpio
git pull origin main
git status                       # no debe haber cambios sin commit

# 1. Agregar el proyecto de Overleaf como remoto (un nombre distinto por proyecto)
git remote add <nombre-remoto> https://git@git.overleaf.com/<ID-DEL-PROYECTO>

# 2. Verificar la conexión y el nombre de la rama (debe aparecer refs/heads/main)
git ls-remote <nombre-remoto>

# 3. Crear la carpeta con el contenido de Overleaf (solo la primera vez se usa "add")
git subtree add --prefix=<carpeta> <nombre-remoto> main --squash

# 4. Subir el resultado a GitHub
git push origin main
```

Para la carta de respuesta, se usó:

```bash
git remote add overleaf-response https://git@git.overleaf.com/6b048f53e8844ae329cec56c
git ls-remote overleaf-response
git subtree add --prefix=response-letter overleaf-response main --squash
git push origin main
```

Notas:

- La primera vez se usa `git subtree add`; `git subtree pull` solo funciona cuando la carpeta ya existe.
- Usa nombres de carpeta sin espacios ni paréntesis.
- Después de agregar un proyecto, súmalo a la tabla del inicio y al flujo diario de este archivo.
- Para que `.DS_Store` no aparezca como archivo sin seguimiento:

```bash
echo ".DS_Store" >> .gitignore
git add .gitignore
git commit -m "Ignore .DS_Store"
git push origin main
```

## Solución de problemas

| Error | Solución |
|---|---|
| `'<carpeta>' does not exist; use 'git subtree add'` | La carpeta aún no existe en el repo. La primera vez usa `git subtree add` en vez de `pull`. |
| `couldn't find remote ref master` | La rama de Overleaf es `main`. Revisa con `git ls-remote <nombre-remoto>`. |
| `remote <nombre> already exists` | Ya existe un remoto con ese nombre. Revisa con `git remote -v` y elige otro nombre, o corrige la URL con `git remote set-url <nombre> <url>`. |
| `prefix '<carpeta>' already exists` | La carpeta ya existe. Usa `git subtree pull` en vez de `add`. |
| `working tree has modifications. Cannot add.` | Si `git status` muestra cambios: haz commit (`git add -A && git commit -m "..."`) o stash (`git stash`, luego el pull, luego `git stash pop`). Si `git status` dice que está limpio, son fechas de archivos desactualizadas: ejecuta `git update-index --refresh` y vuelve a intentar. |
| `403` / "only supports Git authentication tokens" | El usuario debe ser `git` y la contraseña el token. Para borrar credenciales viejas en macOS: `printf "protocol=https\nhost=git.overleaf.com\n\n" \| git credential-osxkeychain erase` |
| `rejected ... (fetch first)` | Overleaf tiene cambios más nuevos. Haz `git subtree pull ...` primero y luego vuelve a hacer push. |
| `rejected ... (non-fast-forward)` justo después de `Added dir '<carpeta>'` | No son cambios nuevos en Overleaf: el split reconstruyó la historia desde cero. Ver "El split reconstruyó la historia" más abajo. |
| `forced push prohibited` | No uses `--force`. Haz pull primero y luego push normal. |
| `zsh: missing end of string` | El nombre de la carpeta tiene espacios o paréntesis. Usa comillas o, mejor, un nombre simple. |
| Archivos de una carpeta aparecen en el otro proyecto de Overleaf | Se cruzó carpeta y remoto en un `subtree pull` o `push`. Revisa que cada `--prefix` vaya con su remoto (ver tabla del inicio). |
| Conflicto de merge después de un pull | Arregla los archivos en conflicto, luego `git add <archivo>` y `git commit`. |
| Archivos aparecen modificados sin haberlos editado | Si solo cambiaron permisos: `git config core.fileMode false`. Si solo cambiaron finales de línea: `git config core.autocrlf input` y luego `git restore .`. |

## El split reconstruyó la historia (`Added dir '<carpeta>'`)

Si `git subtree push` imprime `Added dir '<carpeta>'` y Overleaf rechaza el push como non-fast-forward aunque `git subtree pull` no traiga nada nuevo, el split no reconoció los commits de Overleaf como parte de la historia de la carpeta. Suele pasar cuando se hicieron pushes sin `--rejoin`, así que no hay marca `git-subtree-split` desde donde continuar.

Arreglo (una vez), desde la raíz del repo. Reemplaza `<carpeta>` y `<remoto>` por el par que falló (`paper-revision1` + `overleaf`, o `response-letter` + `overleaf-response`):

```bash
# 1. Si el push fallido creó un merge "Split '<carpeta>/' into commit ...", deshazlo.
#    No cambia archivos; solo mueve el puntero de la rama.
git reset --soft <commit-anterior-al-merge-del-split>

# 2. Reconstruir el split sobre la historia de Overleaf y registrar la marca.
git fetch <remoto> main
git subtree split --prefix=<carpeta> --onto=<remoto>/main --rejoin --squash -m "Rejoin <carpeta>/ with Overleaf"

# 3. Enviar el commit del split directamente (subtree push no acepta --onto) y luego a GitHub.
git push <remoto> "$(git subtree split --prefix=<carpeta> --onto=<remoto>/main)":main
git push origin main
```

`--onto=<remoto>/main` le dice al split que trate los commits de Overleaf como ya separados, así el resultado es un fast-forward de la rama de Overleaf. Después de esto el flujo diario vuelve a funcionar.

## Nota

Guarda este archivo en la raíz del repo, no dentro de las carpetas de Overleaf. Todo lo que esté en `paper-revision1/` o `response-letter/` se sincroniza con su proyecto de Overleaf.
