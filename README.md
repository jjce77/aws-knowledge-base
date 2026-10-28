# AWS Knowledge Base

- An open knowledge base about Amazon Web Services (AWS): service guides, core concepts and step-by-step hands-on labs. No prior AWS experience required.

- Una base de conocimiento abierta sobre Amazon Web Services (AWS): guías de servicios, conceptos y prácticas paso a paso. No se requiere experiencia previa en AWS.

## Choose your language / Elige tu idioma

| Language | Entry point |
|---|---|
| English | [en/README.md](en/README.md) |
| Español | [es/README.md](es/README.md) |

## Repository structure

Mains directories:
```
.
├── en/            English content (fundamentals, services, labs, talks)
├── es/            Contenido en español (fundamentos, servicios, prácticas, charlas)
└── resources/     Shared images and diagrams used by both languages
```

Subdirectories:

### EN/
- **01-fundamentals**: general cloud and AWS concepts that apply to any service.
- **02-services**: reference guides for individual AWS services.
- **03-labs**: complete, step-by-step projects that combine several services.
- **04-talks**: material prepared for talks and workshops.

### ES/
- **01-fundamentos**: conceptos generales de nube y AWS que se aplican a cualquier servicio.
- **02-servicios**: guías de referencia para servicios individuales de AWS.
- **03-practicas**: proyectos completos paso a paso que combinan varios servicios.
- **04-charlas**: material elaborado para charlas y talleres.

## Warning: costs

Some labs create AWS resources (for example EC2 instances, storage or databases) that can generate charges to your account, even outside the Free Tier:

- Set up a billing alert (AWS Budgets) for your account.
- Clean up your resources at the end of each lab to avoid charges.
- Review your billing dashboard regularly.


Algunos laboratorios crean recursos de AWS (por ejemplo instancias EC2, almacenamiento o bases de datos) que pueden generar cargos en tu cuenta, incluso fuera de la Capa gratuita (Free Tier):

- Configura una alerta de facturación (AWS Budgets) para tu cuenta.
- Elimina los recursos al finalizar cada laboratorio para evitar cargos.
- Revisa periódicamente tu panel de facturación.


## Warning: credentials

**Never upload credentials or secrets to any repository**:

- Access keys and secret keys (including the CSV file downloaded from IAM)
- Key pair files (`.pem`, `.ppk`)
- `.env` files and any file with passwords or tokens
- Screenshots showing your AWS account ID, email, keys or public IP addresses

If you ever expose a key by mistake, deactivate or rotate it in IAM immediately. Deleting the file from the repository is **not** enough, because Git keeps the history.


**Nunca subas credenciales o secretos a cualquier repositorio**:

- Claves de acceso y claves secretas (incluido el archivo CSV descargado de IAM)
- Archivos de pares de claves (`.pem`, `.ppk`)
- Archivos `.env` y cualquier archivo que contenga contraseñas o tokens
- Capturas de pantalla que muestren el ID de tu cuenta de AWS, correo electrónico, claves o direcciones IP públicas

>**Nota:**
    - If you ever expose a key by mistake, deactivate or change  it in IAM immediately. Deleting the file from the repository is not enough, because Git keeps the history.
    - Si alguna vez expones una clave por error, desactívala o cambiala de inmediato en IAM. Eliminar el archivo del repositorio no es suficiente, ya que Git conserva el historial.

## Contributing

- Corrections and suggestions are welcome through issues and pull requests. Write content in your own words and link to the official AWS documentation instead of copying it.


- Las correcciones y sugerencias son bienvenidas mediante issues y pull requests. Escribe el contenido con tus propias palabras y enlaza a la documentación oficial de AWS en lugar de copiarla.

## License

- This project is licensed under the [MIT License](LICENSE).

- Este proyecto está licenciado bajo la [Licencia MIT](LICENSE).

## Disclaimer

- This is an independent project. It is not affiliated with, endorsed by or sponsored by Amazon Web Services, Inc. AWS and related names are trademarks of Amazon.com, Inc. or its affiliates.

- Este es un proyecto independiente. No está afiliado, respaldado ni patrocinado por Amazon Web Services, Inc. AWS y los nombres relacionados son marcas registradas de Amazon.com, Inc. o sus afiliadas.