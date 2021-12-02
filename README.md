# Docker Container Scan

In this repository we have a simple ASP .NET web application which loads from a docker container.

We are trying to set up some security in the production pipeline, to avoid pushing breached container images.

The pipeline works out of the box.

## Pipeline

- Open Azure DevOps
- Connect to this repository in GitHub
- Create new pipeline from existing yaml file.
  - Select `azure-pipelines.yml` and that's it!


## What's happenning :grey_question:

The idea behind this code and pipeline, is that you are not able to publish any docker image to the Azure Container Registry, unless it is flawless.

The steps of the pipeline are simple:
- Install trivy in the ubuntu agent dedicated from Azure hosted agents.
- Create an Azure Container Registry.
- Build docker image from Dockerfile contained in the source code.
- Run scan over such image with trivy
- Push the docker image if the scan passes.

> Note: keep in mind that to execute template deployment operations over Azure Portal, you need to create first a `service connection` :electric_plug:.

> Note: The yaml file contains variables whose content might be necessary to change in order to avoid collision in Azure world.