# EnvGene Objects

- [EnvGene Objects](#envgene-objects)
  - [Environment Template Objects](#environment-template-objects)
    - [Template Descriptor](#template-descriptor)
    - [Tenant Template](#tenant-template)
    - [Cloud Template](#cloud-template)
    - [Namespace Template](#namespace-template)
    - [ParameterSet](#parameterset)
    - [Resource Profile Override (in Template)](#resource-profile-override-in-template)
    - [Composite Structure Template](#composite-structure-template)
  - [Environment Instance Objects](#environment-instance-objects)
    - [Tenant](#tenant)
    - [Cloud](#cloud)
    - [Namespace](#namespace)
    - [Application](#application)
    - [Resource Profile Override (in Instance)](#resource-profile-override-in-instance)
    - [Composite Structure](#composite-structure)

## Environment Template Objects

An Environment Template is a file structure within the Envgene Template Repository that describes the structure of a solution — such as which namespaces are part of the solution, as well as environment-agnostic parameters, which are common to a specific type of solution.

The objects that make up the Environment Template extensively use the Jinja template engine. During the generation of an Environment Instance, Envgene renders these templates with environment-specific parameters, allowing a single template to be used for preparing configurations for similar but not entirely identical environment/solution instances.

The template repository can contain multiple Environment Templates describing configurations for different types of environments/solution instances, such as DEV, PROD, and SVT.

When a commit is made to the Template Repository, an artifact is built and published. This artifact contains all the Environment Templates located in the repository.

### Template Descriptor

This object is a describes the structure of a solution, links to solution's components. It has the following structure:

```yaml
tenant: "<path-to-the-tenant-template-file>"
cloud: "<path-to-the-cloud-template-file>"
composite_structure: "<path-to-the-composite-structure-template-file>"
namespaces:
  - template_path: "<path-to-the-namespace-template-file>"
```

[Template Descriptor JSON schema](/schemas/template-descriptor.schema.json)

Any YAML file located in the `/templates/env_templates/` folder is considered a Template Descriptor.

The name of this file serves as the name of the Environment Template. In the Environment Inventory, this name is used to specify which Environment Template from the artifact should be used.

### Tenant Template

This object defines the parameters at the tenant level. It contains configuration for a logical grouping (tenant) that can have multiple environments, clouds, or namespaces. below is strututre of Tenant

```yaml
name: "Field is used to uniquely identify the Tenant"
refistryName: "Field is used to connect with registry"
description: "Description of the tenant"
owners: "Tenant owners"
gitRepository: "Git repository used for the tenancy"
credential: "The identifier for credentials used by the deployment"
labels: "A list of labels that should be applied to the Labels"
globalE2EParameters: "Section with e2e parameters for Namespace that should be used in e2e pipeline"
deployParameters: "Section with Deployment parameters for tenacy"
namespaces:
     tenant: "<path-to-the-tenant-template-file>"
```

[Tenant JSON schema](schemas/tenant.schema.json)

Tenant file will be located in the `/templates/env_templates/tenant.yaml.

### Cloud Template

This object contains all the necessary cloud level paramters.

```yaml
name: "The name of the cloud configuration"
version number : "The version number of the cloud configuration"
apiUrl: "The URL of the API endpoint of the cloud"
apiPort: "The port on which the API runs" 
privateUrl: "The private-facing URL for internal access"
publicUrl: "The public-facing URL for external access"
dashboardUrl: "The URL for accessing the cloud's k8s dashboard"
labels: "A list of labels for categorizing or tagging the cloud"
defaultCredentialsId: "The identifier for credentials used by the deployment"
protocol: "The communication protocol used (https, http)"
dbMode: "The database mode"
databases: "A list of database configurations associated with the deployment"
mergeDeployParametersAndE2EParameters: "Flag indicating whether to merge deployment and end-to-end parameters"
maasConfig:
  credentialsId: "Credentials identifier for MaaS"
  enable: "Flag to enable or disable MaaS"
  maasUrl: "URL for accessing MaaS"
  maasInternalAddress: "Internal address for MaaS"
vaultConfig:
  credentialsId: "Credentials identifier for the vault"
  enable: "Flag to enable or disable vault integration"
  url: "The vault service URL"
dbaasConfigs:
  - credentialsId: "Credentials identifier for DBaaS"
    enable: "Flag to enable or disable DBaaS"
    apiUrl: "API URL for DBaaS"
    aggregatorUrl: "URL for the DBaaS aggregator"
consulConfig:
  tokenSecret: "Secret token for Consul authentication"
  enabled: "Flag to enable or disable Consul integration"
  publicUrl: "The public URL for accessing Consul"
  internalUrl: "The internal URL for accessing Consul"
deployParameters: "Parameters related to the deployment process"
e2eParameters: "E2E Parameters for the cloud"
technicalConfigurationParameters: "Technical parameters for the cloud"
deployParameterSets: "A list of deployment parameter sets for the cloud"
e2eParameterSets: "A list of e2e parameter sets for the cloud"
technicalConfigurationParameterSets: "A list of technical parameter sets for the cloud"
```
[Cloud JSON schema](schemas/cloud.schema.json)

Cloud file will be located in the `/templates/env_templates/cloud.yaml.

### Namespace Template

This object defines parameters at the namespace level

```yaml
name: "Field is used to uniquely identify the Namespace for the Environment"
credentialsId: "The identifier for credentials used by the deployment"
version: "The version number of the namespace configuration"
isServerSideMerge: "Enables server-side merge of parameters"
labels: "A list of labels that should be applied to the Namespace in K8s"
cleanInstallApprovalRequired: "Is approval of clean install required for this namespace"
mergeDeployParametersAndE2EParameters: "Should deployment parameters be merged with e2e parameters"
profile:
  name: "<<filename>> of Resource profile override"
  baseline: "Baseline name of Resource profile override (dev, dev-ha, prod, prod-nonha)"
deployParameters: "Section with Deployment parameters for Namespace's applications"
e2eParameters: "Section with e2e parameters for Namespace that should be used in e2e pipeline"
technicalConfigurationParameters: "Section with Technical/Admin parameters for Namespace's application"
deployParameterSets: "Section with list of Deployment parameter sets <<filename>> for Namespace's applications"
e2eParameterSets: "Section with list of e2e parameter sets <<filename>> for Namespace that should be used in e2e pipeline"
technicalConfigurationParameterSets: "Section with list of Technical/Admin parameter sets <<filename>> for Namespace's applications"
---
[Cloud JSON schema](schemas/namespace.schema.json)

Namespace file will be located in the `/templates/env_templates/.

### ParameterSet

This object represents a container for a group of parameters that can be applied to namespaces or applications

```yaml
name: "Name of the parameter set"
version: "Version number of the specific parameter set"
parameters: "Parameters of the Parameter Set"
applications:
  - appName: "Name of the application"
    parameters: "Application level parameters"
---

[Cloud JSON schema](schemas/paramset.schema.json)

Paramset file will be located in the `/templates/env_templates/Inventory/.

### Resource Profile Override (in Template)

This object defines resource profile parameters

```yaml
name: "Name of the resource profile"
version: "Version of the resource profile"
baseline: "Baseline from where we override resource profile (dev, prod, prod-nonha, dev-ha)"
description: "Optional description field to provide brief about the resource profile"
applications:
  - name: "Name of the application"
    version: "Version of the application"
    sd: "Solution Descriptor"
    services:
      - name: "Name of the service"
        parameters:
          - name: "Parameter name"
            value: "Parameter value"
---

[Cloud JSON schema](schemas/resource-profile.schema.json)

Resource profile file will be located in the `/templates/env_templates/Profile/.

### Composite Structure Template

This is a Jinja template file used to render the [Composite Structure](#composite-structure) object.

Example:

```yaml
name: "{{ current_env.cloudNameWithCluster }}-composite-structure"
baseline:
  name: "{{ current_env.environmentName }}-core"
  type: "namespace"
satellites:
  - name: "{{ current_env.environmentName }}-bss"
    type: "namespace"
  - name: "{{ current_env.environmentName }}-oss"
    type: "namespace"
```

## Environment Instance Objects

An Environment Instance is a file structure within the Envgene Instance Repository that describes the configuration for a specific environment/solution instance.  

It is generated during the rendering process of an Environment Template. During this rendering process, environment-agnostic parameters from the Environment Template are combined with environment-specific parameters, such as Cloud Passport, environment-specific ParameterSet, environment-specific Resource Profile Overrides, to produce a set of parameters specific to a particular environment/solution instance.  

The Environment Inventory is mandatory for creating an Environment Instance. It is a configuration file that describes a specific environment, including which Environment Template artifact to use and which environment-specific parameters to apply during rendering. It serves as the "recipe" for creating an Environment Instance.  

The Environment Instance has a human-readable structure and is not directly used by parameter consumers. For parameter consumers, a consumer-specific structure is generated based on the Environment Instance. For example, for ArgoCD, an Effective Set is generated.

EnvGene adds the following header to all auto-generated objects (all Environment Instance objects are auto-generated):

```yaml
# The contents of this file is generated from template artifact: <environment-template-artifact>.
# Contents will be overwritten by next generation.
# Please modify this contents only for development purposes or as workaround.
```

> [!NOTE]
> The \<environment-template-artifact> placeholder is automatically replaced with the name of the EnvGene Environment Template artifact used for generation.

EnvGene sorts every Environment Instance object according to its JSON schema. This ensures that when objects are modified (e.g., when applying a new template version), the repository commits remain human-readable.

### Tenant

TBD

### Cloud

TBD

### Namespace

TBD

### Application

This object defines application level parameters 

```yaml
name: "The name of the application"
deployParameters: "A collection of key-value pairs specifying deployment (to manifest rendering) parameters"
technicalConfigurationParameters: "A collection of key-value pairs specifying technical configuration (to configure application behavior without redeployment) parameters"
```
[Cloud JSON schema](schemas/application-profile.schema.json)

Application file will be located in the `/templates/env_templates/Namespace/Application.

### Resource Profile Override (in Instance)

TBD

### Composite Structure

This object describes the composite structure of a solution. It contains information about which namespace hosts the core applications that offer essential tools and services for business microservices (`baseline`), and which namespace contains the applications that consume these services (`satellites`). It has the following structure:

```yaml
name: <composite-structure-name>
# Envgene automatically adds `version`` attribute regardless of what is specified in the template
# Envgene always sets the value to 0
# If the attribute already exists in the template, it will be overwritten.
version: 0
# Envgene automatically adds `id`` attribute regardless of what is specified in the template
# Envgene sets this attribute's value to what is specified in `baseline.name`
# If the attribute already exists in the template, it will be overwritten
id: baseline.name
baseline:
  name: <baseline-namespace>
  type: "namespace"
satellites:
  - name: <satellite-namespace-1>
    type: "namespace"
  - name: <satellite-namespace-2>
    type: "namespace"
```

The Composite Structure is located in the path `/configuration/environments/<CLUSTER-NAME>/<ENV-NAME>/composite-structure.yml`

[Composite Structure JSON schema](/schemas/composite-structure.schema.json)

Example:

```yaml
name: "clusterA-env-1-composite-structure"
version: 0
id: "env-1-core"
baseline:
  name: "env-1-core"
  type: "namespace"
satellites:
  - name: "env-1-bss"
    type: "namespace"
  - name: "env-1-oss"
    type: "namespace"
```
