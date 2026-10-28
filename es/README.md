# README

# AWS Knowledge Base (Español)

Base de conocimiento abierta sobre Amazon Web Services (AWS): guías de servicios, conceptos y prácticas paso a paso. No se requiere experiencia previa en AWS.

[← Volver al inicio](../README.md) · [English version](../en/README.md)

## Contenido

| Sección | Descripción | Estado |
|---|---|---|
| [fundamentos - Conceptos básicos](01-fundamentos/01-conceptos-basicos.md) | Conceptos generales de la nube y de AWS: regiones, responsabilidad compartida, facturación, glosario | Hecho |
| [fundamentos - Modelos de servicios de computación en la nube](01-fundamentos/02-modelos-servicios-de-computacion-en-nube.md) | Conceptos generales de la nube y de AWS: regiones, responsabilidad compartida, facturación, glosario | En construcción |
| servicios | Guías de referencia de cada servicio de AWS (IAM, CodeCommit, EC2 y más) | Próximamente |
| prácticas | Proyectos completos paso a paso que combinan varios servicios | Próximamente |
| charlas | Material preparado para charlas y talleres | Próximamente |

Las imágenes y diagramas están en la carpeta compartida [resources](../resources/).

## Cómo usar este repositorio

1. Si estás empezando, lee primero **fundamentos**.
2. Consulta **servicios** cuando quieras entender un servicio concreto.
3. Sigue una **práctica** cuando quieras ver un proyecto de principio a fin.

Cada práctica indica sus prerrequisitos, los pasos, cómo verificar el resultado, los errores comunes y cómo **limpiar los recursos** al terminar.

## Advertencia: costos

Algunas prácticas crean recursos en AWS (por ejemplo instancias EC2, almacenamiento o bases de datos) que **pueden generar cobros** en tu cuenta, incluso fuera de la capa gratuita.

- Configura una alerta de presupuesto (AWS Budgets) antes de empezar cualquier práctica.
- Sigue la sección de **limpieza** al final de cada práctica para eliminar lo que creaste.
- Revisa con frecuencia tu panel de facturación.

## Advertencia: credenciales

**Nunca subas credenciales ni secretos a este repositorio** (ni a ningún repositorio público):

- Claves de acceso (access key y secret key), incluido el archivo CSV que descarga IAM
- Archivos de par de claves (`.pem`, `.ppk`)
- Archivos `.env` y cualquier archivo con contraseñas o tokens
- Capturas de pantalla que muestren el ID de tu cuenta, correo, claves o direcciones IP públicas

Si expones una clave por error, desactívala o rótala de inmediato en IAM. Borrar el archivo del repositorio **no** es suficiente, porque Git conserva el historial.

## Contribuir

Las correcciones y sugerencias son bienvenidas mediante issues y pull requests. Escribe el contenido con tus propias palabras y enlaza a la documentación oficial de AWS en lugar de copiarla.

## Licencia

Este proyecto está licenciado bajo la [Licencia MIT](../LICENSE).

## Aviso

Este es un proyecto comunitario independiente. No está afiliado, respaldado ni patrocinado por Amazon Web Services, Inc. AWS y los nombres relacionados son marcas registradas de Amazon.com, Inc. o sus afiliadas.
