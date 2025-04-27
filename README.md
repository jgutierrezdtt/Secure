# Secure

Proyecto de automatización de seguridad para DevSecOps que integra múltiples herramientas de análisis y proporciona mapeo automático a estándares de seguridad como ASVS y MASVS.

## 🔒 Características

- **Pipeline completo de seguridad**: Integración de análisis SAST, SCA, DAST, IaC y contenedores
- **Mapeo de hallazgos a estándares**: Correlación automática de CWEs a requisitos ASVS/MASVS
- **Informes de cumplimiento**: Generación automática de informes detallados de cumplimiento
- **Soporte para aplicaciones móviles**: Opción para incluir análisis contra MASVS
- **Compatibilidad con herramientas comerciales y open source**: Plantillas para ambos tipos

## 🛠️ Componentes principales

### Pipeline de Seguridad

El pipeline principal (`security-pipeline.yml`) integra todos los análisis de seguridad y genera un informe de cumplimiento:

```yaml
name: Pipeline Completo de Seguridad

on:
  # Ejecución programada todos los lunes a medianoche
  schedule:
    - cron: '0 0 * * 1'
  
  # Ejecución en push a ramas principales
  push:
    branches: [ main, master ]
  
  # Ejecución en pull requests
  pull_request:
    branches: [ main, master ]
  
  # Ejecución manual
  workflow_dispatch:
    inputs:
      run_all_scans:
        type: boolean
        description: 'Ejecutar todos los análisis (incluso los programados)'
        default: false
      include_mobile:
        type: boolean
        description: 'Incluir análisis para aplicaciones móviles'
        default: false

jobs:
  # Lista de análisis incluidos
  sonarqube-scan: {...}    # SAST
  snyk-sca: {...}          # SCA
  dependency-check: {...}  # SCA
  kics-iac: {...}          # IaC
  snyk-container: {...}    # Contenedores
  zap-dast: {...}          # DAST
  syft-sbom: {...}         # SBOM
  snyk-iac: {...}          # IaC
  compliance-analysis: {...} # Análisis de Cumplimiento
  security-summary: {...}  # Resumen final
```

### Análisis de Cumplimiento

El proceso de análisis de cumplimiento (`security-compliance-analysis.yml`) realiza las siguientes acciones:

1. **Recolección de resultados**: Extrae hallazgos de los archivos SARIF generados por las herramientas
2. **Identificación de CWEs**: Extrae los CWEs de los resultados de análisis
3. **Mapeo a requisitos**: Relaciona los CWEs con los requisitos en `asvs.json` y `masvs.json`
4. **Evaluación de cumplimiento**: Determina qué requisitos de seguridad se están incumpliendo
5. **Generación de informes**: Crea un reporte HTML con gráficos y detalles

## 🚀 Uso

### Ejecución manual

Puedes ejecutar el pipeline completo manualmente desde la interfaz de GitHub Actions:

1. Ve a la pestaña "Actions" en tu repositorio
2. Selecciona "Pipeline Completo de Seguridad"
3. Haz clic en "Run workflow"
4. Configura las opciones:
   - **Ejecutar todos los análisis**: Activa todos los análisis (incluso los programados)
   - **Incluir análisis para aplicaciones móviles**: Habilita el mapeo contra MASVS

### Ejecución automática

El pipeline se ejecuta automáticamente:
- En cada push a las ramas `main` o `master`
- En cada pull request a las ramas principales
- Cada lunes a medianoche (análisis programado completo)

### Configuración necesaria

Para utilizar todas las funcionalidades, configura los siguientes secretos en tu repositorio:

```yaml
# Para análisis estático
SONAR_TOKEN: Token de autenticación de SonarQube
SONAR_HOST_URL: URL de tu instancia de SonarQube

# Para análisis de dependencias
SNYK_TOKEN: Token de API de Snyk

# Para análisis dinámico
TEST_APPLICATION_URL: URL de la aplicación en entorno de pruebas

# Para análisis de contenedores
REGISTRY_USERNAME: Usuario del registro de contenedores
REGISTRY_PASSWORD: Contraseña del registro de contenedores
```

## 📊 Informes generados

Después de cada ejecución del pipeline, se generan los siguientes artefactos:

- **Informe de cumplimiento de requisitos**: Documento HTML detallado que muestra:
  - Requisitos de seguridad incumplidos (ASVS/MASVS)
  - Distribución por severidad
  - Detalles de cada hallazgo
  - Gráficos de cumplimiento

- **Resumen en el workflow**: Resumen ejecutivo que muestra:
  - Estado de cada análisis
  - Número de requisitos incumplidos
  - Enlaces a los informes detallados

- **SBOM**: Software Bill of Materials generado con Syft

## 📝 Requisitos de seguridad

El proyecto utiliza dos archivos JSON para mapear CWEs a requisitos de seguridad:

- **`static/asvs.json`**: Requisitos del OWASP Application Security Verification Standard
- **`static/masvs.json`**: Requisitos del OWASP Mobile Application Security Verification Standard

## 🔧 Personalización

Puedes personalizar el pipeline modificando:

- Los umbrales de severidad en cada herramienta
- Las condiciones de fallo del pipeline
- Las herramientas incluidas en cada análisis
- Los mapeos de CWEs a requisitos en los archivos JSON

## 📋 Plantillas de herramientas disponibles

El proyecto incluye plantillas reutilizables para las siguientes herramientas:

### Herramientas comerciales
- **SonarQube**: Análisis estático de código (SAST)
- **Snyk**: Análisis de dependencias (SCA), contenedores e infraestructura como código

### Herramientas open-source
- **OWASP Dependency-Check**: Análisis de composición de software (SCA)
- **Checkmarx KICS**: Análisis de infraestructura como código (IaC)
- **OWASP ZAP**: Análisis dinámico de aplicaciones (DAST)
- **Anchore Syft**: Generación de SBOM (Software Bill Of Materials)

## 🤝 Contribución

Las contribuciones son bienvenidas. Puedes:

1. Añadir soporte para nuevas herramientas
2. Mejorar los mapeos de CWEs a requisitos ASVS/MASVS
3. Optimizar los workflows existentes
4. Añadir nuevas funcionalidades de análisis

## 📄 Licencia

Este proyecto está licenciado bajo la licencia MIT - ver el archivo LICENSE para más detalles.