```mermaid
    graph TD
        %% Main Repository Structure
        RepoSystem["Environment Configuration System"]
        EnvInvGit["Environments Inventory Git Repository"]
        TemplatesGit["Templates Git Repository"]
        RenderedEPGit["Rendered Templates/Effective Parameters Git Repository"]
        
        %% Repository Relationships
        RepoSystem --> EnvInvGit
        RepoSystem --> TemplatesGit
        RepoSystem --> RenderedEPGit
        
        %% Environments Inventory Structure
        EnvInvGit --> SiteConfig["Site-wide Configurations"]
        EnvInvGit --> Clusters["Cluster Configurations"]
        
        %% Site Configurations
        SiteConfig --> SiteCredentials["Site-wide Credentials"]
        SiteConfig --> SiteParams["Site-wide Parameters"]
        SiteConfig --> SiteProfiles["Site-wide Resource Profiles"]
        
        %% Cluster Configurations
        Clusters --> CloudPassport["Cloud Passport"]
        Clusters --> AppDeployer["App Deployer"]
        Clusters --> ClusterCredentials["Cluster-wide Credentials"]
        Clusters --> ClusterParams["Cluster-wide Parameters"]
        Clusters --> ClusterProfiles["Cluster-wide Resource Profiles"]
        Clusters --> Environments["Environment Configurations"]
        
        %% Environment Configurations
        Environments --> EnvInventory["Environment Inventory"]
        EnvInventory --> EnvCredentials["Environment-wide Credentials"]
        EnvInventory --> EnvParams["Environment-wide Parameters"]
        EnvInventory --> EnvProfiles["Environment-wide Resource Profiles"]
        EnvInventory --> SolutionDesc["Solution Descriptor"]
        EnvInventory --> EnvDefinition["Environment Definition"]
        
        %% Templates Git Structure
        TemplatesGit --> EnvTemplates["Environment Templates"]
        TemplatesGit --> ParamTemplates["Parameter Templates"]
        TemplatesGit --> RPOTemplates["Resource Profile Templates"]
        
        %% Environment Templates
        EnvTemplates --> TemplateDesc["Template Descriptor"]
        EnvTemplates --> CloudTemplates["Cloud Templates"]
        EnvTemplates --> TenantTemplates["Tenant Templates"]
        EnvTemplates --> NSTemplates["Namespace Templates"]
        
        %% Rendered Templates/EP Git Structure
        RenderedEPGit --> RenderedEnvs["Rendered Environment Instances"]
        RenderedEnvs --> EffectiveParams["Effective Parameters Set"]
        RenderedEnvs --> RenderedTemplates["Rendered Templates"]
        
        %% Rendered Templates Structure
        RenderedTemplates --> RenderedCreds["Rendered Credentials"]
        RenderedTemplates --> RenderedNamespaces["Rendered Namespaces"]
        RenderedTemplates --> RenderedProfiles["Rendered Profiles"]
        RenderedTemplates --> RenderedApps["Rendered Applications"]
        
        %% Template Processing Flow
        TemplatesGit -.-> |"Renders to"| RenderedEPGit
        EnvInvGit -.-> |"Provides config for"| RenderedEPGit
        
        classDef repo fill:#ffcccc,stroke:#ff0000,stroke-width:2px
        classDef component fill:#ccffcc,stroke:#00aa00,stroke-width:1px
        classDef rendered fill:#ccccff,stroke:#0000ff,stroke-width:1px
        
        class EnvInvGit,TemplatesGit,RenderedEPGit repo
        class SiteConfig,Clusters,EnvTemplates,ParamTemplates,RPOTemplates,CloudPassport,AppDeployer,EnvInventory,TemplateDesc component
        class RenderedEnvs,EffectiveParams,RenderedTemplates,RenderedCreds,RenderedNamespaces,RenderedProfiles,RenderedApps rendered
```
