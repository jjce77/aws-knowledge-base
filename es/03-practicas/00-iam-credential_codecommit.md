# Crear credenciales Git para CodeCommit

## Objetivo

Generar un usuario y una contraseña de Git para clonar y subir código a CodeCommit por HTTPS.

## Prerrequisitos

- El usuario IAM `demo-user-aws` ya creado (ver [00-iam-create-user.md](00-iam-create-user.md)).

## Pasos

1. Abre el servicio **IAM** y entra a **Users**.
2. Selecciona `demo-user-aws`.
3. Abre la pestaña **Security credentials**.
4. Baja hasta **HTTPS Git credentials for AWS CodeCommit** y pulsa **Generate credentials**.
5. Pulsa **Download credentials** y guarda el archivo `.csv` en un lugar seguro, fuera del repositorio.

> Nota: Este usuario y contraseña son distintos a los de inicio de sesión en la consola. La contraseña solo se muestra una vez. Si la pierdes, genera un nuevo par en esta misma sección (máximo dos pares por usuario).

## Siguiente paso

Crear el repositorio en CodeCommit y clonarlo (ver [01-codeCommit.md](01-codeCommit.md)).
