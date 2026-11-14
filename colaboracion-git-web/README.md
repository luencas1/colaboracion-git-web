# Colaborar con Git — proyecto base para dos personas

Web estática (HTML + CSS, sin dependencias) que explica cómo colaborar con Git:
repositorio compartido (invitaciones) y fork. Además, sirve de **proyecto de práctica**:
dos personas la mejoran juntas aplicando lo que explica.

```
colaboracion-git-web/
├── index.html   # contenido de la web
├── styles.css   # estilos (modo claro y oscuro)
└── README.md    # estas instrucciones
```

Para verla, abre `index.html` en el navegador:
`open index.html` (macOS) · `xdg-open index.html` (Linux) · `start index.html` (Windows).

---

## Roles

| Persona | Rol | Tarjeta que edita en `index.html` |
|---|---|---|
| **A** | Dueño: crea el repo, invita y revisa | `PERSONA A` (sección *Colaboradores*) |
| **B** | Colaborador (o contribuidor con fork) | `PERSONA B` (sección *Colaboradores*) |

> Cada persona edita **solo su propia parte**. Así pueden trabajar a la vez sin conflictos.

---

## Opción 1 — Repositorio compartido (invitación)

**Persona A (dueño):**

```bash
cd colaboracion-git-web
git init
git add .
git commit -m "Proyecto base de colaboración"
# Crea un repo vacío en GitHub y copia su URL
git remote add origin https://github.com/USUARIO-A/colaboracion-git-web.git
git push -u origin main
```

En GitHub: **Settings → Collaborators → Add people** → usuario de B.

**Persona B (colaborador):** acepta la invitación en `github.com/notifications` y luego:

```bash
git clone https://github.com/USUARIO-A/colaboracion-git-web.git
cd colaboracion-git-web
git switch -c feature/tarjeta-USUARIO-B
# Edita tu tarjeta en index.html
git commit -am "Agrega tarjeta de USUARIO-B"
git push -u origin feature/tarjeta-USUARIO-B
```

**Persona A revisa e integra:**

```bash
git fetch
git log --oneline main..origin/feature/tarjeta-USUARIO-B
git diff main...origin/feature/tarjeta-USUARIO-B
git merge origin/feature/tarjeta-USUARIO-B
git push
git push origin --delete feature/tarjeta-USUARIO-B
```

**Persona B se actualiza:**

```bash
git switch main
git pull
git fetch --prune
git branch -d feature/tarjeta-USUARIO-B
```

---

## Opción 2 — Fork (sin invitación)

**Persona B:** en el repo de A pulsa **Fork → Create fork** y luego:

```bash
git clone https://github.com/USUARIO-B/colaboracion-git-web.git
cd colaboracion-git-web
git remote add upstream https://github.com/USUARIO-A/colaboracion-git-web.git
git remote -v                       # debe mostrar origin y upstream
git switch -c feature/tarjeta-USUARIO-B
# Edita tu tarjeta en index.html
git commit -am "Agrega tarjeta de USUARIO-B"
git push -u origin feature/tarjeta-USUARIO-B
```

En tu fork: **Compare & pull request** → revisa que la base sea `USUARIO-A/... main` → **Create pull request**.

**Persona A:** revisa el PR en GitHub y pulsa **Merge pull request**.

**Persona B mantiene su fork al día:**

```bash
git switch main
git fetch upstream
git merge upstream/main
git push origin main
```

---

## Retos para seguir practicando

Repártanse los retos: **una rama por reto** y que **la otra persona revise** antes de integrar.

1. **A:** agrega una sección nueva *"Glosario"* (origin, upstream, fast-forward, PR).
2. **B:** agrega una fila nueva a la tabla de *Errores comunes*.
3. **A:** cambia el color de acento (`--accent` en `styles.css`).
4. **B:** agrega un enlace al glosario en el menú de navegación.
5. **Provoquen un conflicto a propósito:** los dos cambian el mismo `<h1>` en ramas distintas, integren una y resuelvan el conflicto en la segunda.

## Acuerdos del equipo

- Nada directo a `main`.
- Ramas con prefijo: `feature/`, `fix/`, `docs/`.
- `git pull` antes de empezar.
- Commits pequeños con mensajes claros.
- Otra persona revisa antes de integrar.
- Borrar ramas ya integradas (local y remota).
