# Templates de GitHub Actions para Seguridad

Esta carpeta contiene templates reutilizables de GitHub Actions para análisis de seguridad que se pueden incorporar en otros repositorios o pipelines.

## Estructura de Carpetas

- [`commercial/`](./commercial/) - Templates para herramientas comerciales (Sonar, Snyk)
- [`opensource/`](./opensource/) - Templates para herramientas de código abierto (ZAP, KICS, Dependency-Check, Syft)

## Cómo Utilizar Estos Templates

Estos templates están diseñados como workflows reutilizables que se pueden llamar desde otros workflows de GitHub Actions. Para usarlos, deberás:

1. Hacer referencia a este repositorio en tu workflow
2. Llamar al template necesario usando `uses` con la sintaxis `{owner}/{repo}/.github/workflows/{path}@{ref}`

### Ejemplo de Uso

```yaml
name: Mi Pipeline de Seguridad

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  call-sonarqube-scan:
    uses: miorg/Secure/.github/workflows/templates/commercial/sonar-sast.yml@main
    with:
      sonar_project_key: "mi-proyecto"
    secrets:
      sonar_token: ${{ secrets.SONAR_TOKEN }}
      
  call-dependency-check:
    uses: miorg/Secure/.github/workflows/templates/opensource/dependency-check.yml@main
    with:
      project_name: "mi-proyecto"
```

## Requisitos

Cada template puede requerir diferentes secretos o variables de entorno. Consulta la documentación específica de cada template para conocer sus requisitos.