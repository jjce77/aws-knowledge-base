# Paso a paso: Crear un repositorio en CodeCommit, clonarlo y subir los archivos iniciales

## Prerrequisitos
- Usuario IAM con permisos de CodeCommit (ver guía de crear usuario IAM)
- Credenciales HTTPS de Git generadas (ver guía de credenciales Git)
- Git instalado

## 1. Crear el repositorio
1. Entra a la consola de AWS y abre **CodeCommit**.
2. Verifica que estás en la **región** correcta (arriba a la derecha). Esa región define el host de la URL (por ejemplo `us-east-2`).
3. Clic en **Crear repositorio**.
4. Escribe el **nombre** (ejemplo: `demo-code-football-players`) y una descripción opcional.
5. Opcional: agrega **tags** (por ejemplo `Proyecto=Demo`).
6. Clic en **Crear**.

## 2. Copiar la URL de clonación
1. Vuelve a **CodeCommit** y abre tu repositorio.
2. Clic en **Clonar URL > Clonar HTTPS**.
3. La URL tiene este formato:
   `https://git-codecommit.us-east-2.amazonaws.com/v1/repos/demo-code-football-players`

## 3. Clonar el repositorio
En una terminal (GIT Bash, CMD o PowerShell) para este documento se usó GIT Bash:

```bash
git clone https://git-codecommit.us-east-2.amazonaws.com/v1/repos/demo-code-football-players
```

- Si no tienes credenciales guardadas, Git Credential Manager abrirá una ventana. Ingresa el **usuario y contraseña HTTPS de Git** del [Configurar credenciales de CodeCommit](00-iam-credential_codecommit.md).
- Si ya las tenías guardadas de otro repo, no pedirá nada y clonará directo.
- Verás el aviso *"You appear to have cloned an empty repository"*. Es normal.

## 4. Entrar a la carpeta y configurar tu identidad
```bash
cd demo-code-football-players
git config user.name "Tu Nombre"
git config user.email "tu@correo.com"
```

## 5. Archivos, primeros commits y push

**.gitignore**
```bash
cat > .gitignore << 'EOF'
# Node dependencies
node_modules/
*.log

# Environment variables
.env*

# Editors and IDEs
.vscode/
.idea/

# OS generated files
.DS_Store
Thumbs.db

# Production build outputs 
dist/
build/

EOF


git add .gitignore
git commit -m "chore: add .gitignore"
```




**README.md**
```bash
cat > README.md <<'EOF'
# Demo Football Players

CRUD demo app for managing football players. Built to demonstrate basic Create, Read, Update, and Delete operations


EOF

git add README.md
git commit -m "docs: add README with project overview and usage"

```


**Crear archivos seed JSON**

```bash
mkdir data

cat > data/players.json << 'EOF'
[{"id": 1, "photo": null, "first_name": "Jose L", "last_name": "Rodriguez", "nickname": "El Puma", "birth_date": "1998-05-14", "team_id": "mil", "position": "forward", "height_cm": 178, "weight_kg": 72, "preferred_foot": "right", "followers": 120, "stats": {"speed": 82, "shooting": 78, "passing": 65, "dribbling": 80, "defending": 35, "physical": 70}}, {"id": 2, "photo": null, "first_name": "Carlos", "last_name": "Gómez", "nickname": "La Muralla", "birth_date": "1995-11-02", "team_id": "mil", "position": "defender", "height_cm": 186, "weight_kg": 81, "preferred_foot": "left", "followers": 85, "stats": {"speed": 62, "shooting": 40, "passing": 68, "dribbling": 50, "defending": 84, "physical": 80}}]
EOF

cat > data/teams.json << 'EOF'
[{"id": "mil", "name": "Millonarios", "city": "Bogotá", "shield": null, "championships_won": 16}]
EOF



git add data/players.json data/teams.json
git commit -m "feat(data): add seed data for players and teams"

```

Subir a repositorio
```bash
git push -u origin master
```
Después del push, los archivos aparecerán en la consola de CodeCommit.

---

## Errores comunes (para una sección de troubleshooting)

| Error | Causa probable |
|---|---|
| **403** al clonar | Usaste la contraseña de la consola en vez de las credenciales HTTPS de Git, o el usuario no tiene permisos de CodeCommit. |
| No pide credenciales | Git Credential Manager ya tenía unas guardadas de otro repo. |
| Repo no encontrado | La región de la URL no coincide con la del repo. |

