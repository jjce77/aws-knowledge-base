# Desplegar una app Node.js en EC2 y dejarla como servicio

## Objetivo

Preparar una instancia EC2 con Ubuntu, bajar una aplicación Node.js y Express desde CodeCommit, ejecutarla y dejarla corriendo como servicio, de modo que siga activa aunque cierres la terminal o se reinicie la instancia.

## Prerrequisitos

- Una instancia EC2 con Ubuntu en estado Running y conexión por SSH funcionando (ver [03-lanzar-instancia-ec2.md](03-lanzar-instancia-ec2.md)).
- El puerto de la aplicación abierto en el Security Group (en esta guía, el 3000).
- La aplicación cargada en un repositorio de CodeCommit.
- Las credenciales HTTPS de Git del usuario IAM (ver la guía de credenciales Git).
- Estar conectado a la instancia por SSH. El prompt debe verse como `ubuntu@ip-...:~$`.

La aplicación de ejemplo usa Express, no tiene paso de build y guarda sus datos en archivos JSON dentro de la carpeta `data/`. Los nombres que aparecen aquí (`demo-code-football-players`, región `us-east-2`) son los del ejemplo. Reemplázalos por los tuyos.

## Pasos

### 1. Actualizar el sistema

```bash
sudo apt update
sudo apt upgrade -y
```

`apt update` refresca la lista de paquetes y `apt upgrade` instala las actualizaciones. Puede tardar un par de minutos. Si aparece una pantalla preguntando por reiniciar servicios, pulsa Enter para aceptar lo marcado.

### 2. Instalar Node.js

En Ubuntu, `sudo apt install nodejs` instala una versión antigua. Usa el repositorio de NodeSource con la versión mayor que uses en tu computador (comprúebala con `node -v`). En el ejemplo es la 20:

```bash
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt install -y nodejs
```

Si usas otra versión, cambia el `20` por la tuya.

### 3. Verificar Node, npm y Git

```bash
node -v
npm -v
git --version
```

Debes ver un número de versión en cada uno. Si `git` no responde, instálalo con `sudo apt install -y git`.

### 4. Clonar el repositorio desde CodeCommit

1. En CodeCommit, abre el repositorio, pulsa **Clone URL** y elige **Clone HTTPS**. La URL tiene esta forma:
   `https://git-codecommit.REGION.amazonaws.com/v1/repos/NOMBRE-DEL-REPOSITORIO`
2. En la instancia, ve a tu carpeta de usuario y clona:

```bash
cd ~
git clone https://git-codecommit.REGION.amazonaws.com/v1/repos/NOMBRE-DEL-REPOSITORIO
```

3. Git pide **Username** y **Password**. Usa las credenciales HTTPS de Git que generaste en IAM ([00-iam-credential_codecommit.md](00-iam-credential_codecommit.md)), no la contraseña de la consola. Al pegar la contraseña no se ve nada en pantalla.
4. Entra a la carpeta y comprueba el contenido:

```bash
cd NOMBRE-DEL-REPOSITORIO
ls
```

Debes ver archivos de tu aplicación.


### 5. Instalar las dependencias

Las dependencias no vienen en el repositorio. La carpeta `node_modules` no se sube a Git, así que hay que instalarla en la instancia. Como el proyecto tiene `package-lock.json`, usa `npm ci`:

```bash
npm ci --omit=dev
```

| Opción | Qué hace |
|---|---|
| `npm ci` | Instala exactamente las versiones de `package-lock.json`, sin modificarlo. Es más rápido y repetible que `npm install` |
| `--omit=dev` | Salta las dependencias de desarrollo (por ejemplo `jest`), que el servidor no necesita |

Si el proyecto no tiene `package-lock.json`, usa `npm install --omit=dev`.

Comprueba que apareció la carpeta `node_modules` con `ls`.

### 6. Probar la aplicación a mano

Antes de crear el servicio, comprueba que la app arranca. 

```bash
node app.js
```

Debe mostrar un mensaje como `Server running at http://localhost:3000`. Aunque diga localhost, la app es accesible desde afuera si escucha en todas las interfaces, que es lo habitual en Express cuando no se indica un host.

Abre en tu navegador, con `http` y no `https`:

```
http://IP-PUBLICA:3000
```

La IP es la **Public IPv4 address** de la instancia. Cuando confirmes que funciona, detén la app con **Ctrl + C**. Es importante detenerla, porque si sigue corriendo ocupará el puerto 3000 y el servicio no podrá arrancar.

### 7. Crear el servicio con systemd

Para que la app corra sola, se crea un archivo de servicio en `systemd`, que ya viene en Ubuntu.

1. Comprueba la ruta de Node, normalmente `/usr/bin/node`. Sí la ruta es diferente ajusta en el bash del siguiente punto 2 (`ExecStart=/usr/bin/node app.js`):

```bash
which node
```

2. Crea el archivo del servicio. Ajusta `WorkingDirectory` con la ruta de tu proyecto:

```bash
sudo tee /etc/systemd/system/football-players.service > /dev/null <<'EOF'
[Unit]
Description=CRUD Football Players (Node.js)
After=network.target

[Service]
Type=simple
User=ubuntu
WorkingDirectory=/home/ubuntu/NOMBRE-DEL-REPOSITORIO
ExecStart=/usr/bin/node app.js
Restart=always
RestartSec=5
Environment=NODE_ENV=production
Environment=PORT=3000

[Install]
WantedBy=multi-user.target
EOF
```

Qué hace cada línea importante:

| Línea | Para qué sirve |
|---|---|
| `User=ubuntu` | Corre la app con tu usuario, no como root |
| `WorkingDirectory` | La carpeta del proyecto. Es clave porque la app usa rutas relativas |
| `ExecStart` | El comando que arranca la app. Debe coincidir con la ruta que mostró `which node` |
| `Restart=always` | Si la app se cae, systemd la levanta de nuevo a los 5 segundos |
| `Environment=PORT=3000` | Define el puerto en el que escucha la app |
| `WantedBy=multi-user.target` | Permite que arranque al encender la instancia |

3. Carga el archivo, actívalo y arráncalo:

```bash
sudo systemctl daemon-reload
sudo systemctl enable --now football-players
```

### 8. Comprobar el servicio

```bash
sudo systemctl status football-players
```

Debe decir `active (running)`. Sal de esa vista con la tecla `q`. Luego abre de nuevo `http://IP-PUBLICA:3000` en el navegador.

Para una prueba más completa, reinicia la instancia con `sudo reboot`, espera uno o dos minutos y vuelve a abrir la app. Si carga, el servicio arranca solo. Después de reiniciar, la IP pública puede cambiar, así que cópiala de nuevo desde la consola.

## Verificación

- `sudo systemctl status football-players` muestra `active (running)`.
- La app abre en `http://IP-PUBLICA:3000`.
- Puedes crear un jugador y la lista lo muestra.
- Al ejecutar `sudo systemctl restart football-players`, la app vuelve a responder en pocos segundos.

## Comandos útiles del servicio

| Comando | Para qué sirve |
|---|---|
| `sudo systemctl status football-players` | Ver si está corriendo |
| `sudo journalctl -u football-players -f` | Ver los logs en vivo (Ctrl + C para salir) |
| `sudo journalctl -u football-players -n 30` | Ver las últimas 30 líneas de logs |
| `sudo systemctl restart football-players` | Reiniciar después de cambiar el código |
| `sudo systemctl stop football-players` | Detener el servicio |
| `sudo systemctl disable football-players` | Evitar que arranque al encender la instancia |

## Errores comunes

| Problema | Causa probable | Solución |
|---|---|---|
| `Cannot find module 'express'` | No se instalaron las dependencias | Ejecutar `npm ci --omit=dev` dentro de la carpeta del proyecto |
| `Authentication failed` al clonar | Se usó la contraseña de la consola o un usuario incorrecto | Usar las credenciales HTTPS de Git descargadas desde IAM |
| `Repository not found` | La región o el nombre de la URL no coinciden | Copiar de nuevo la URL desde CodeCommit con **Clone HTTPS** |
| `EADDRINUSE: address already in use :::3000` | Otro proceso usa el puerto, por ejemplo `node app.js` lanzado a mano | Detenerlo con Ctrl + C, o buscarlo con `sudo lsof -i :3000` |
| El navegador se queda cargando o da timeout | El puerto 3000 no está abierto en el Security Group, o se usó `https` | Revisar la regla Custom TCP del puerto y abrir la URL con `http` |
| El servicio sale `failed` | Ruta incorrecta en `WorkingDirectory` o `ExecStart`, o falta `npm ci` | Revisar con `sudo journalctl -u football-players -n 30` y corregir el archivo del servicio |
| Cambié el archivo del servicio y no se aplica | systemd no recargó la configuración | Ejecutar `sudo systemctl daemon-reload` y luego `sudo systemctl restart football-players` |
| Las fotos de jugadores existentes salen rotas | Las imágenes no están en el repositorio | Las fotos nuevas que se suban sí funcionan. Para las anteriores, subirlas al repositorio o volver a cargarlas |
| Los datos desaparecen al terminar la instancia | Los datos viven en archivos JSON dentro de la instancia | Es lo esperado en esta práctica. Para conservarlos habría que usar una base de datos o un almacenamiento externo |

## Seguridad

- No escribas las credenciales de Git dentro de la URL del comando.
- Git no guarda la contraseña por defecto, así que cada `git pull` la pedirá de nuevo. No la guardes en archivos sin entender el riesgo: quien acceda a la instancia podría leerla.
- No corras la app como root. El servicio usa el usuario `ubuntu`.
- Abre en el Security Group solo el puerto de la app y el SSH desde tu IP.
- No publiques capturas donde aparezca la IP pública de la instancia o el ID de cuenta.

## Limpieza

Al terminar la demo:

1. Si ya no necesitas el servicio, detenlo y desactívalo: `sudo systemctl disable --now football-players`.
2. Termina la instancia desde EC2 (**Instance state, Terminate instance**), porque detenerla deja el volumen generando cobro.
3. Elimina el grupo de seguridad y el par de claves, y borra el archivo `.pem` de tu computador.
4. Si no vas a usar más el repositorio de CodeCommit ni las credenciales Git, elimínalos desde sus consolas.

## Siguiente paso

Automatizar el despliegue con un script (`deploy.sh`) que haga `git pull`, `npm ci --omit=dev` y reinicie el servicio.

