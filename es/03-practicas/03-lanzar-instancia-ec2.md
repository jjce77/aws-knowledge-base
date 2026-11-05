# Lanzar una instancia EC2 y conectarse por SSH

## Objetivo

Crear una instancia EC2 con Ubuntu Server, abrir el puerto 22 (SSH) solo para tu IP y el puerto de la aplicación, y conectarte a ella desde tu computador.

## Prerrequisitos

- Sesión iniciada en la consola como `demo-user-aws`, no como root.
- La aplicación ya cargada en el repositorio de CodeCommit.
- Saber en qué puerto escucha tu app Express (en esta guía se usa el 3000 como ejemplo).
- Una terminal con SSH. En Windows sirve Git Bash o PowerShell.

Si tu consola está en español, estos son los nombres equivalentes que aparecen en la guía:

| Inglés | Español |
|---|---|
| Launch instance | Lanzar instancia |
| Instances | Instancias |
| Running | En ejecución |
| Security | Seguridad |
| Security groups | Grupos de seguridad |
| Edit inbound rules | Editar reglas de entrada |
| My IP | Mi IP |
| Anywhere | Cualquier lugar |
| Custom | Personalizado |
| Custom TCP | TCP personalizado |
| Save rules | Guardar reglas |
| Terminate instance | Terminar instancia |

## Pasos

### 1. Abrir EC2 y verificar la región

1. En la barra de búsqueda de la consola escribe `EC2` y abre el servicio.
2. Arriba a la derecha comprueba la región. Usa la misma donde está tu repositorio de CodeCommit (por ejemplo `us-east-2`, Ohio).
3. Pulsa **Launch instance**.

### 2. Nombre de la instancia

En **Name** escribe un nombre legible, por ejemplo `demo-code-football-players-server`. Este nombre se guarda como el tag `Name` y es lo que verás en la lista de instancias.

### 3. Imagen del sistema (AMI)

1. En **Application and OS Images** elige **Ubuntu**.
2. Selecciona **Ubuntu Server 24.04 LTS** (o la versión LTS más reciente que ofrezca la consola) con arquitectura **64-bit (x86)**.
>Nota: Comprueba que aparece la etiqueta de apta para la capa gratuita (free tier).


### 4. Tipo de instancia

1. En **Instance type** elige **t3.micro**, para la capa gratuita.
2. Si tu proyecto hace un build pesado dentro de la instancia (frontend con webpack o Vite, TypeScript grande, dependencias nativas), elige **t3.small**.


### 5. Par de claves

1. En **Key pair (login)** pulsa **Create new key pair**.
2. En **Key pair name** escribe `demo-code-football-players-key`.
3. Tipo: **RSA**.
4. Formato: **.pem**.  **
5. Pulsa **Create key pair**.

> ** Usa `.ppk` solo si vas a conectarte con PuTTY.

> Nota: El archivo se descarga automáticamente solo una vez. Si lo pierdes, no hay forma de recuperarlo. Guárdalo en una carpeta fuera del repositorio.

### 6. Configuración de red

Deja los valores por defecto del asistente. 

El asistente creará un grupo de seguridad con un nombre como `launch-wizard-2` y una regla SSH abierta a todo Internet (`0.0.0.0/0`). Esa regla se corrige mas adelante por seguridad.

### 7. Almacenamiento

Deja el volumen por defecto (8 GiB, gp3). Es suficiente para esta práctica.


### 8. Revisar y lanzar

1. En el panel **Summary** de la derecha verifica: 1 instancia, la imagen Ubuntu Server, el tipo de instancia (t3.micro) y el nuevo grupo de seguridad.
2. Pulsa **Launch instance**.
3. Pulsa **View all instances**.
4. Selecciona tu instancia y espera a que **Instance state** sea **Running**. Puede tardar uno o dos minutos.

### 9. Ajustar el Security Group

1. En la lista de instancias, selecciona tu instancia.
2. Abre la pestaña **Security** y pulsa el enlace del grupo de seguridad.
3. En la nueva ventana, pulsa **Edit inbound rules**.
4. En la regla de tipo **SSH**, cambia **Source type** a **My IP**. Debe aparecer tu IP con `/32` al final.
5. Comprueba que la IP sea IPv4, es decir, cuatro números separados por puntos, por ejemplo `203.0.113.25/32`. 
6. Si no existe una regla para el puerto de tu app, pulsa **Add rule** y configura:
   - Type: **Custom TCP**
   - Port range: `3000` (el puerto de tu app)
   - Source type: **Anywhere** (`0.0.0.0/0`), para poder abrir la app desde el navegador
7. Pulsa **Save rules**.

Los cambios se aplican al instante, sin reiniciar la instancia. Si luego pones Nginx delante de la app, abrirás el puerto 80 en lugar del 3000.


### 10. Conectarte por SSH

1. En la lista de instancias, selecciona la tuya y copia la **Public IPv4 address**.
2. En una terminal en tu computador, reemplaza la ruta de la llave y la IP por las tuyas:

```bash
chmod 400 /ruta/a/demo-code-football-players-key.pem
ssh -i /ruta/a/demo-code-football-players-key.pem ubuntu@IP-PUBLICA
```
> Nota: chmod aplica mas para linux

- La primera vez, SSH pregunta si confías en el servidor. Escribe `yes`.
- Si aparece el prompt `ubuntu@ip-...:~$`, ya estás dentro.
- En Git Bash, la unidad C se escribe `/c/Users/...` y la F, `/f/...`.
- En Windows, si SSH dice que el archivo de la llave tiene permisos demasiado abiertos, copia el `.pem` a `C:\Users\TU-USUARIO\.ssh\` y usa esa ruta. En una unidad distinta de C, `chmod` puede no tener efecto.

## Verificación

Dentro de la instancia ejecuta:

```bash
whoami
cat /etc/os-release
```

Debe mostrar `ubuntu` y Ubuntu como sistema operativo. Para salir de la sesión escribe `exit`.

La AWS CLI no viene preinstalada en Ubuntu (compruébalo con `aws --version`). Si decides usar un rol IAM para que la instancia lea CodeCommit, habrá que instalarla en la siguiente guía.

## Alternativa: conectarte desde el navegador

En la lista de instancias selecciona la tuya, pulsa **Connect** y elige **EC2 Instance Connect**. Abre una terminal en el navegador sin necesidad del archivo `.pem`. Funciona solo si el puerto 22 admite las IP de AWS para esa región; con la regla **My IP** suele fallar. Para esta práctica usa SSH desde tu computador.

## Errores comunes

| Problema | Causa probable | Solución |
|---|---|---|
| **My IP** muestra una dirección larga con dos puntos (IPv6) | La consola detectó tu IPv6, pero la instancia se conecta por IPv4 | Ver la sección siguiente, "Si My IP muestra una IPv6" |
| `Connection timed out` al hacer SSH | Tu IP cambió, usas una VPN, la regla del puerto 22 no existe, o hay una IPv6 en la regla | En EC2, Security Groups, edita la regla SSH y vuelve a elegir **My IP**. Verifica que sea IPv4 y que la instancia tenga Public IPv4 |
| `Permission denied (publickey)` | Usuario incorrecto o llave equivocada | Usar el usuario `ubuntu` (no `ec2-user`, que es de Amazon Linux) y la llave elegida al lanzar la instancia |
| `UNPROTECTED PRIVATE KEY FILE` | El archivo `.pem` tiene permisos muy abiertos | Ejecutar `chmod 400` sobre el archivo, o moverlo a la carpeta `.ssh` en Windows |
| `Could not resolve hostname` | La IP está mal copiada o la instancia está detenida | Copiar de nuevo la Public IPv4 desde la consola |
| La instancia no tiene Public IPv4 | Se dejó **Auto-assign public IP** en Disable | Recrear la instancia con la opción habilitada, o asociar una Elastic IP |
| Perdí el archivo `.pem` | Solo se descarga una vez | Crear un nuevo par de claves y una nueva instancia |
| No encuentro la IP después de reiniciar | La IP pública cambia al detener y encender la instancia | Copiar la nueva IP, o usar una Elastic IP |
| La app no abre en el navegador | El puerto de la app no está abierto en el Security Group, o la app no está corriendo | Revisar la regla Custom TCP y que la app escuche en `0.0.0.0` |

### Si My IP muestra una IPv6

Algunas conexiones tienen IPv6 y la consola la usa para **My IP**. Como la instancia se conecta por IPv4, esa regla no te dejará entrar. Hay dos salidas:

1. Obtener tu IPv4 desde tu computador y usarla con **Custom**:
   ```bash
   curl -4 https://checkip.amazonaws.com
   ```
   Luego, en la regla SSH, elige **Custom**, escribe la IPv4 con `/32` y guarda.
2. Si no logras obtenerla, deja el SSH temporalmente en **Anywhere**, conéctate a la instancia y ejecuta:
   ```bash
   echo $SSH_CONNECTION
   ```
   El primer valor es tu IPv4 pública tal como la ve AWS. Úsala en la regla SSH con **Custom** y `/32`. No dejes el puerto 22 abierto a todos más tiempo del necesario.

Si usas una VPN, la IP que ve AWS es la de la VPN. Toma la IPv4 con la VPN en el mismo estado en que te vas a conectar.

## Seguridad

- No subas el archivo `.pem` al repositorio. Guárdalo fuera de la carpeta del proyecto.
- Nunca dejes el puerto 22 abierto a `0.0.0.0/0` más allá de unos minutos. Usa **My IP**.
- No publiques capturas donde se vea la IP pública de la instancia, tu IP, ni el ID de cuenta.
- Abre solo los puertos que necesitas. Cada puerto abierto es una puerta más.

## Costos y limpieza

Una instancia encendida consume horas o créditos de tu cuenta, aunque no la uses. Al terminar la demo:

1. En EC2, Instances, selecciona la instancia y elige **Instance state, Terminate instance**. Detenerla no es suficiente si ya no la vas a usar, porque el volumen de disco sigue generando cobro.
2. Cuando esté en estado **Terminated**, ve a **Security Groups** y elimina el grupo de seguridad que creó el asistente.
3. En **Key Pairs** elimina `demo-code-football-players-key` y borra el archivo `.pem` de tu computador.
4. Si asociaste una Elastic IP, libérala en **Elastic IPs, Release**, porque una Elastic IP sin uso genera cobro.
5. Si lanzaste más de una instancia por error, termina también las demás.
6. Revisa el panel de facturación para confirmar que no quedan recursos activos.

## Siguiente paso

Preparar la instancia: instalar Node.js y Git, configurar el acceso a CodeCommit y descargar la aplicación.