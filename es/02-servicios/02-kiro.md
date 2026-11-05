# Kiro

> IDE y CLI con agentes de IA de AWS, basado en especificaciones.

**Revisado:** 2026-11-05

## Qué es Kiro?

Kiro es el entorno de desarrollo con IA de AWS, ofrecido como **IDE** (construido sobre Code OSS, la base de VS Code) y **CLI**. Funciona con modelos Claude a través de Amazon Bedrock. En lugar de generar código directo desde un prompt, primero produce **especificaciones** (requisitos, diseño y tareas) que revisas antes de escribir código.

## Conceptos clave

| Concepto | Qué es |
|---|---|
| **Specs** | Tres archivos en `.kiro/specs/`: requisitos, diseño y lista de tareas. |
| **Steering** | Archivos en `.kiro/steering/` con las reglas y convenciones del proyecto. |
| **Hooks** | Automatizaciones que se disparan al guardar, crear o cambiar archivos (tests, documentación, revisiones). |
| **Powers** | Paquetes que añaden conocimiento y herramientas por dominio (por ejemplo, AWS SAM). |
| **MCP** | Soporte nativo del Model Context Protocol para conectar fuentes externas. |

## Servicios relacionados

- **Amazon Bedrock**: aloja los modelos que usa Kiro.
- **AWS SAM / Lambda**: despliegue serverless, apoyado por Powers.
- **CloudWatch / X-Ray / ECS**: diagnóstico de errores en vivo con el Power de DevOps.
- **GitHub y otros repos**: se conectan por MCP; las specs se versionan en tu repositorio.
- **IAM**: permisos de las cuentas y recursos que usa el agente.

## Ventajas

- **Planificación primero**: specs revisables en lugar de código improvisado.
- **Decisiones documentadas**: las specs viven en git y explican el porqué del diseño.
- **Automatización con hooks**: tareas repetitivas sin pedirlas cada vez.
- **Reglas persistentes**: el steering mantiene el estilo del proyecto entre sesiones.
- **Integración con AWS**: Powers y MCP para trabajar con tus recursos.
- **Familiar**: basado en VS Code, con plan gratuito para empezar.

## Desventajas

- **Solo modelos de Bedrock**: no puedes usar GPT, Gemini u otros proveedores.
- **Flujo pesado para tareas pequeñas**: las specs añaden pasos cuando solo quieres un cambio rápido.
- **Comunidad más pequeña**: menos recursos que Cursor o GitHub Copilot.
- **Sin soporte JetBrains**: IDE basado en VS Code.
- **Límites de uso y precios cambiantes**: el consumo de planes se mide en créditos o interacciones y las fuentes no coinciden en las cifras; confirma en kiro.dev/pricing.
- **Producto joven**: su alcance y sus planes han cambiado desde el lanzamiento.

> Fuentes de terceros y un artículo de AWS Builder Center. No pude abrir kiro.dev desde aquí: verifica allí precios y modelos disponibles antes de presentarlos.

## Sources

- [Beyond vibe coding: a deep dive into Amazon Kiro and Kiro Powers (AWS Builder Center)](https://builder.aws.com/content/3GnMJw34VD70jLQxljWLw8FGiOL/beyond-vibe-coding-a-deep-dive-into-amazon-kiro-and-kiro-powers)
- [What is Kiro? (Cloudvisor)](https://cloudvisor.co/what-is-kiro/)
- [Kiro Pricing and Free Tier (UsageBar)](https://usagebar.com/blog/kiro-pricing-and-free-tier)
