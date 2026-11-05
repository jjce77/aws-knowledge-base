# AWS Identity and Access Management (IAM)

> Controla quién puede entrar a tu cuenta de AWS y qué puede hacer.

**Revisado:** 2026-11-05

## Qué es AWS IAM?

AWS Identity and Access Management (IAM) es el servicio que resuelve la autenticación (quién eres) y la autorización (qué puedes hacer). Cada cuenta nace con un usuario root con acceso total, que no se debe usar en el día a día. Para todo lo demás creas identidades con solo los permisos necesarios. 
**IAM no tiene costo**; pagas únicamente por los servicios que uses con él.

## Conceptos clave

| Concepto | Qué es |
|---|---|
| Usuario root | Dueño de la cuenta, acceso total. Protégelo con MFA y evita usarlo. |
| Usuario IAM | Identidad con credenciales propias dentro de la cuenta. |
| Grupo | Conjunto de usuarios que comparten permisos. |
| Rol | Identidad sin credenciales fijas que se asume de forma temporal (servicios, otras cuentas, usuarios federados). |
| Política | Documento JSON que permite o niega acciones sobre recursos. |
| MFA | Segundo factor de autenticación además de la contraseña. |

## Servicios relacionados

- **IAM Identity Center**: acceso centralizado (SSO) para tu equipo. 
- **STS (Security Token Service)**: entrega credenciales temporales.
- **IAM Access Analyzer**: detecta accesos externos y permisos sin usar. El análisis de acceso externo es gratuito; otras funciones se cobran.
- **CloudTrail**: registra quién hizo qué en tu cuenta.
- **AWS Organizations**: aplica reglas de acceso a varias cuentas.
- **EC2, S3 y el resto de servicios**: los recursos cuyo acceso controla IAM.

## Ventajas

- Gratis: no hay cargo por usar IAM, Identity Center ni STS.
- Mínimo privilegio: das a cada identidad solo lo que necesita.
- Credenciales temporales: los roles evitan claves de larga duración.
- Federación y SSO: usa tu directorio corporativo o Identity Center en vez de crear usuarios sueltos.
- Integración total: funciona con prácticamente todos los servicios de AWS.
- Auditable: combinado con CloudTrail, queda registro de cada acción.

## Desventajas

- Consistencia eventual: un cambio en usuarios, roles o políticas puede tardar en propagarse.
- Curva de aprendizaje: políticas en JSON, roles y permisos pueden ser confusos al empezar.
- Fácil dar permisos de más: por rapidez se suele asignar acceso total, lo que aumenta el riesgo.
- Solo para AWS: gestiona el acceso a AWS, no a tus aplicaciones ni a otras nubes.
- Difícil de gobernar a escala: muchas cuentas y roles exigen herramientas adicionales.

> Los conceptos de grupo, política y MFA y las desventajas son criterio general; la documentación consultada solo detalla identidades, consistencia eventual y costo.

## Sources

- [¿Qué es IAM? (guía del usuario de AWS)](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction)
- [Introducción a IAM (guía del usuario de AWS)](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started.html)
