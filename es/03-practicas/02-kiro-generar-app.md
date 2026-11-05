# Generar la aplicación con Kiro

## Objetivo

Pedirle a Kiro que construya la aplicación web de jugadores dentro del repositorio que creaste en CodeCommit, revisar lo que genera y subirla a la rama `dev` de ese mismo repositorio.

## Prerrequisitos

- El repositorio de CodeCommit clonado en tu computador, con los archivos iniciales y la rama `dev` (ver [01-codeCommit.md](01-codeCommit.md)).
- Kiro instalado.
- Node.js y npm instalados en tu computador.

## Pasos

### 1. Abrir el repositorio en Kiro

1. En Kiro, abre como proyecto la carpeta del repositorio clonado (por ejemplo `demo-code-football-players`). La aplicación se crea ahí, no en una carpeta nueva.
2. En la terminal, dentro de esa carpeta, comprueba que estás en la rama `dev`:

```bash
git branch
```

Debe aparecer `* dev`. Si no, ejecuta `git checkout dev` o `git switch dev`.

### 2. Pegar el prompt

Abre el chat de Kiro y pega este prompt. Describe lo que quieres, no cómo programarlo:

```text
Quiero una aplicación web para gestionar jugadores del fútbol colombiano. 
El alcance es crear, ver, editar y borrar jugadores.

De cada jugador quiero guardar: foto (opcional; si no hay, se muestran sus 
iniciales), nombre, apellido, apodo, fecha de nacimiento, equipo, posición 
(delantero, mediocampista, defensa o arquero), estatura (slider, 170 por 
defecto), peso (slider, 70 por defecto), pie hábil (derecho por defecto, 
izquierdo o ambidiestro), seguidores y seis estadísticas estilo FIFA 
(velocidad, tiro, pase, regate, defensa y físico), cada una con un slider de 
1 a 99 y valor inicial 50. La edad y el overall (el promedio de las seis 
estadísticas) se calculan, no se guardan.

Los equipos son los del fútbol colombiano, en un archivo aparte de solo 
lectura, con nombre, ciudad, escudo (opcional) y campeonatos ganados. 
Los datos se guardan en archivos JSON.

La pantalla principal muestra a los jugadores en tarjetas: foto a la izquierda 
(las fotos son verticales) y los datos a la derecha, con nombre, apodo, equipo, 
posición, overall y seguidores. Cada tarjeta tiene tres botones solo con icono 
(ver, editar y eliminar) que muestran su nombre al pasar el mouse. Por defecto 
van ordenadas por overall de mayor a menor, y hay un selector para ordenarlas 
también por nombre, equipo o posición. Arriba hay una barra de navegación con 
"Inicio" y un botón "Nuevo jugador".

Además hay un formulario (el mismo para crear y editar) y una pantalla de 
detalle con todos los datos del jugador y sus seis estadísticas.

El diseño debe ser claro, con tonos azules suaves, usando solo las clases de 
Bootstrap, sin escribir CSS propio.

Técnicamente: Express con arquitectura MVC, nombres de campos en inglés 
con snake_case y clases en PascalCase, y textos de la interfaz en español.

Antes de escribir código, muéstrame un resumen de lo que vas a construir.

Ya existen data/players.json y data/teams.json en el repositorio. Úsalos y no los sobrescribas.
Para los botones con icono puedes usar Bootstrap Icons.
La aplicación debe arrancar con "node app.js" y escuchar en el puerto 3000 (o en el que indique la variable PORT).
```

### 3. Revisar el resumen

Kiro mostrará primero un resumen de lo que va a construir. Léelo y comprueba que incluye los campos, las pantallas y los botones que pediste. Si falta algo, díselo en el chat antes de aceptar.

### 4. Iterar

La primera versión rara vez queda perfecta. Pide los ajustes con frases cortas, por ejemplo:

- "El overall debe verse más grande en la tarjeta."
- "Muestra un mensaje cuando no hay jugadores."
- "El botón de eliminar debe pedir confirmación."

### 5. Probar en tu computador

En la terminal, dentro de la carpeta del proyecto:

```bash
npm install
node app.js
```

Abre `http://localhost:3000` y prueba crear, ver, editar y eliminar un jugador. Detén la app con **Ctrl + C**.

### 6. Hacer commits mientras avanzas

Kiro trabaja por tareas (tasks). Cada vez que Kiro y tú terminen una tarea y la app siga funcionando, guarda ese avance con un commit:

```bash
git add .
git commit -m "feat: descripción corta de lo que se hizo"
```

Por ejemplo: `feat: agrega el listado de jugadores` o `feat: agrega el formulario para crear jugadores`. Así, si algo sale mal, puedes volver al último avance que funcionaba.

### 7. Subir todo a la rama dev

Cuando hayas terminado todas las tareas y probado la app, sube los commits a CodeCommit:

```bash
git push origin dev
```

En la consola de CodeCommit, abre el repositorio y elige la rama `dev`: verás el código de la aplicación junto a los archivos iniciales.

## Verificación

- La app abre en `http://localhost:3000` y el CRUD funciona.
- En CodeCommit aparecen `app.js`, `package.json` y `package-lock.json` en la rama `dev`.

## Errores comunes

| Problema | Causa probable | Solución |
|---|---|---|
| Kiro cambió los datos iniciales | No se le indicó que ya existían | Restaura `data/` con `git checkout -- data` y repite la indicación |
| `Cannot find module` | Faltan las dependencias | Ejecuta `npm install` |
| `EADDRINUSE` | Otra copia de la app sigue corriendo | Cierra la otra terminal o detenla con Ctrl + C |
| `git push` pide usuario y contraseña | Es la primera vez en este equipo | Usa las credenciales Git de [00-iam-credential_codecommit.md](00-iam-credential_codecommit.md) |

## Siguiente paso

Lanzar la instancia EC2 (ver [03-lanzar-instancia-ec2.md](03-lanzar-instancia-ec2.md)).
