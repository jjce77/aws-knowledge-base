# AWS CodeCommit

> Repositorios Git privados administrados por AWS.

Revisado: 2026-11-05

## Qué es AWS CodeCommit?

Servicio de control de código fuente totalmente administrado que aloja repositorios privados basados en Git, sin operar servidores propios. Cifrado en tránsito y en reposo, y acceso controlado con IAM.

Estado: cerrado a nuevos clientes en julio de 2024; volvió a disponibilidad general el 24 de noviembre de 2025 (29 regiones, mismo precio, SLA de 99.9%).

## Servicios relacionados

- **CodeBuild**: compila y ejecuta pruebas.
- **CodeDeploy**: despliega a EC2, Lambda, ECS u on-premise.
- **CodePipeline**: orquesta el flujo CI/CD.
- **IAM / CloudTrail**: permisos y auditoría.
- **Simple Notification Service     (SNS) / EventBridge / Lambda**: notificaciones y automatización por eventos.

## Ventajas

- IAM: permisos finos por repositorio, rama y acción.
- Seguridad: cifrado con KMS, auditoría con CloudTrail, VPC endpoints.
- Sin administración: alta disponibilidad y escalado a cargo de AWS.
- Integración nativa: dispara CodePipeline y CodeBuild sin webhooks externos.
- Datos en tu cuenta: el código queda en la región que elijas.
- Costo simple: tarifa por usuario activo.
- Git estándar: fácil de migrar si cambias de plataforma.

## Desventajas

- Futuro incierto: AWS lo cerró a nuevos clientes en 2024, recomendó migrar y lo reactivó en 2025 tras las quejas de los clientes. Hay hoja de ruta, pero su continuidad a largo plazo no está garantizada.
- Colaboración básica: pull requests y revisión de código más limitados que en GitHub o GitLab.
- Sin CI/CD propio: depende de CodeBuild y CodePipeline, más verbosos de configurar.
- Sin gestión de proyectos: no incluye issues, tableros ni wikis.
- Ecosistema pequeño: menos integraciones de terceros (Dependabot, escáneres, asistentes de IA).
- Autenticación menos directa: credenciales Git de IAM, `git-remote-codecommit` o SSH asociado a IAM.

## Frente a alternativas

| Aspecto | CodeCommit | GitHub | GitLab |
|---|---|---|---|
| CI/CD nativo | No (CodeBuild/CodePipeline) | GitHub Actions | GitLab CI/CD |
| Pull/Merge requests | Básicos | Excelentes | Excelentes |
| Issues y proyectos | No | Sí | Sí |
| Control de acceso | IAM | Roles y equipos | Roles y grupos |
| Auto-hospedaje | No | Solo Enterprise Server | Sí |
| Estabilidad del producto | Incierta | Alta | Alta |

Elígelo si tu proyecto vive en AWS y necesitas IAM y datos dentro de tu cuenta. Mantén un espejo (`git clone --mirror`) hacia GitHub o GitLab como plan de salida.

## Sources

- [AWS CodeCommit Returns to General Availability (blog oficial AWS)](https://aws.amazon.com/blogs/devops/aws-codecommit-returns-to-general-availability)
- [CodeCommit User Guide, historial del documento](https://docs.aws.amazon.com/codecommit/latest/userguide/history.html)
- [InfoQ: AWS CodeCommit Returns to General Availability after Backlash](https://infoq.com/news/2025/12/aws-codecommit-ga)
- [AWS CodeCommit (página oficial)](https://aws.amazon.com/es/codecommit/)
