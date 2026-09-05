# Guía para sincronizar un Fork

Esta guía explica cómo mantener actualizada una copia (fork) de un repositorio Git con los cambios del repositorio principal u originario.

## Conceptos de Remotos en Git

Al trabajar con un fork, normalmente existen dos repositorios remotos configurados:

- **origin**: Tu fork personal (donde tienes permisos de escritura para hacer push).
- **upstream**: El repositorio original desde el cual se creó el fork (o el repositorio padre si es un fork de un fork).

## Configuración Inicial del Remoto Upstream

Antes de sincronizar, verifica si ya tienes configurado el remoto `upstream`.

```bash
git remote -v
```

Si no aparece `upstream` en la lista, agrégalo indicando la URL del repositorio principal.

```bash
git remote add upstream https://github.com/deepseek-ai/deepseek-harness.git
```

Si realizaste un fork de un fork y deseas sincronizar con el repositorio padre intermedio o con el repositorio raíz original, asigna la URL correspondiente al remoto `upstream`.

## Sincronización Paso a Paso

### 1. Guardar cambios locales pendientes

Si tienes cambios locales no confirmados (uncommitted), guárdalos temporalmente en el stash.

```bash
git stash
```

### 2. Cambiar a la rama principal

Asegúrate de estar ubicado en la rama que deseas actualizar (normalmente `master` o `main`).

```bash
git checkout master
```

### 3. Descargar los cambios remotos

Obtén los últimos commits y ramas del repositorio principal sin alterar tu trabajo local todavía.

```bash
git fetch upstream
```

### 4. Fusionar los cambios

Integra los cambios de la rama principal remota en tu rama local.

```bash
git merge upstream/master
```

Si prefieres mantener un historial lineal en lugar de crear commits de merge, puedes usar rebase.

```bash
git rebase upstream/master
```

### 5. Restaurar cambios locales en stash (si aplicó el paso 1)

```bash
git stash pop
```

### 6. Actualizar tu repositorio en GitHub (Origin)

Sube la rama actualizada a tu fork remoto en GitHub.

```bash
git push origin master
```

Nota: Si en tu entorno local existen hooks pre-push que fallan por dependencias nativas del sistema operativo, puedes omitir la verificación pre-push agregando la bandera `--no-verify`.

```bash
git push origin master --no-verify
```

## Sincronización usando GitHub CLI

Si tienes instalada la herramienta oficial de línea de comandos `gh`, puedes vincular el repositorio por defecto y sincronizar directamente.

```bash
gh repo set-default deepseek-ai/deepseek-harness
gh repo sync
```
