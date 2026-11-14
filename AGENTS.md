# Guía de Agente y Buenas Prácticas de Git (`Duck-Systems-Fucks`)

Este documento define las reglas operativas y las buenas prácticas de control de versiones (Git) que todo agente de IA y colaborador debe seguir en este repositorio.

---

## 1. Flujo de Ramas (Branching Strategy)

- **Rama principal (`main`)**:
  - Representa el estado estable y funcional del proyecto.
  - **Prohibido** hacer *force push* (`git push --force`) en `main`.
  - Evitar commits directos a `main` para cambios grandes; preferir ramas de trabajo e integrarlas mediante Pull Request / Merge.
- **Nomenclatura de ramas**:
  Usa prefijos descriptivos en minúsculas separados por guiones (`kebab-case`):
  - `feat/<descripcion-corta>`: Nuevas funcionalidades (ej. `feat/user-auth`).
  - `fix/<descripcion-corta>`: Corrección de errores (ej. `fix/login-crash`).
  - `chore/<descripcion-corta>`: Configuración, dependencias o mantenimiento (ej. `chore/setup-linter`).
  - `docs/<descripcion-corta>`: Cambios exclusivos de documentación (ej. `docs/update-readme`).
  - `refactor/<descripcion-corta>`: Refactorización de código sin cambios funcionales.

---

## 2. Convención de Commits (Conventional Commits)

Todos los commits deben ser **atómicos** (un solo propósito lógico por commit) y seguir el formato **Conventional Commits**:

```text
<tipo>(<alcance opcional>): <descripción breve en imperativo>

[cuerpo opcional explicando el "qué" y el "por qué", no el "cómo"]

[footer opcional: referencias a issues o BREAKING CHANGE]
```

### Tipos permitidos:
- `feat`: Nueva característica o funcionalidad.
- `fix`: Corrección de un bug.
- `docs`: Cambios en documentación (`README.md`, `AGENTS.md`, comentarios).
- `style`: Formato, espacios, punto y coma (sin cambios de lógica).
- `refactor`: Cambio de código que no corrige un bug ni agrega una funcionalidad.
- `test`: Agregar o corregir pruebas.
- `chore`: Cambios en scripts de build, configuración, `.gitignore` o dependencias.

### Reglas del mensaje:
- Máximo **72 caracteres** en la primera línea.
- Usar modo imperativo (ej. *"agregar endpoint de usuarios"* / *"add user endpoint"*, no *"agregado"* ni *"agregando"*).
- No terminar la primera línea con punto.

### Ejemplos prácticos de Commits:

**Ejemplo 1: Commit simple (documentación o configuración)**
```bash
git add AGENTS.md
git commit -m "docs: inicializar guia de buenas practicas de git en AGENTS.md"
```

**Ejemplo 2: Nueva funcionalidad con alcance (`scope`)**
```bash
git add src/auth/login.ts
git commit -m "feat(auth): agregar validacion de token JWT en login"
```

**Ejemplo 3: Corrección de bug con cuerpo explicativo**
```bash
git add src/api/users.ts
git commit -m "fix(api): manejar respuesta vacia al consultar perfil de usuario" -m "Evita que la aplicacion lance un TypeError cuando el backend devuelve 204 No Content."
```

---

## 3. Reglas Operativas para el Agente de IA

1. **Verificación previa al commit (`Pre-flight check`)**:
   - Ejecutar siempre `git status` y `git diff` antes de preparar un commit para revisar exactamente qué líneas cambian.
   - Asegurar que el código compile, pase el linter y no contenga `console.log` / `print` de depuración olvidados ni marcadores de conflicto (`<<<<<<<`, `=======`, `>>>>>>>`).
2. **Staging selectivo**:
   - Evitar `git add .` o `git add -A` a ciegas cuando existan archivos temporales o no relacionados. Preferir `git add <archivo>` o `git add -p`.
3. **Seguridad y Secretos (Crítico)**:
   - **NUNCA** confirmar archivos `.env`, credenciales, llaves privadas (`.pem`, `.key`), tokens de API ni dumps de bases de datos.
   - Verificar que `.gitignore` esté actualizado antes de añadir nuevas herramientas o dependencias (ej. `node_modules/`, `__pycache__/`, `.venv/`, `dist/`, `.DS_Store`).
4. **Operaciones destructivas prohibidas sin autorización explícita**:
   - No ejecutar `git reset --hard`, `git clean -fd`, `git push --force`, ni `git branch -D` sin confirmación explícita del usuario.
   - No alterar ni reescribir commits que ya hayan sido publicados en el remoto salvo instrucción directa.
5. **Sincronización y Resolución de Conflictos**:
   - Hacer `git pull --rebase` (o verificar el estado remoto) antes de integrar cambios.
   - En caso de conflicto de merge/rebase, analizar ambas partes antes de resolver; no sobrescribir cambios de otros colaboradores sin entender el contexto.
