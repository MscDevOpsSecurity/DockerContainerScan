# Escaneo de contenedores Docker

En este repositorio tenemos una simple aplicación web en ASP .NET, la cual se carga desde un contenedor de Docker.

Estamos intentado configurar algo de seguridad en la pipeline de producción, para evitar publicar imágenes corruptas o con brechas de seguridad.

La pipeline funciona tal cual está.

## Pipeline

- Abre Azure DevOps
- Conectate a este repositorio de GitHub
- Crea una nueva pipeline a partir del archivo yaml existente.
  - Selecciona `azure-pipelines.yml`
  - Configurar en la pipeline la service connection de los tasks Azure Deployment: *Create Azure Container Registry, Build container image, Push container image* . La service connection utilizada tienes que ser una de las configurada en ***Project Setting-> Service Connection*** que apunta a una *Azure Subscription* valida
  - Adaptar la seciones de variables para definir una Azure Container Registry univoco. En particular hay que cambiar las variables *acrHostName* y *acrName*.

- Lanzar la pipeline.


## Qué está ocurriendo :grey_question:

La idea detrás de este código, es que no seas capaz de publicar ninguna imagen de docker al Azure Container Registry, a menos que no tenga fallos de seguridad.

Los pasos de la pipeline son simples:
- Instala trivy en un agente dedicado de ubuntu perteneciente a "Azure hosted agents".
- Crea un Azure Container Registry.
- Compila la imagen docker desde el Dockerfile contenido en el código fuente.
- Ejecuta el escaneo sobre dicha imagen con trivy
- Publica la imagen docker si el escaneo es válido.

> Nota: ten en cuenta que para ejecutar operaciones de despliegue de templates en el portal de Azure, necesitas crear primero una `service connection` :electric_plug:.

> Nota: el archivo yaml tiene variables, cuyo contenido es necesario cambiar para evitar una colisión de nombres en el mundo Azure.