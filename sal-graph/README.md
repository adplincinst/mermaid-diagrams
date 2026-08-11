



```mermaid
%%{init: {"flowchart": {"wrappingWidth": 9999, 'nodeSpacing': 50, 'rankSpacing': 350}}}%%

graph TD
classDef cmd  fill:#add8e6,stroke:#333,stroke-width:1px;
classDef subcmd fill:#f8d7da,stroke:#f5c6cb,color:#721c24;

    sal:::cmd
    sal-init-cmd:::subcmd
    sal-validate-cmd:::subcmd
    sal-build-cmd:::subcmd
    sal-run-cmd:::subcmd
    sal-push-cmd:::subcmd
    sal-pull-cmd:::subcmd
    sal-clone-cmd:::subcmd
    sal-import-cmd:::subcmd
    sal-salmodule-cmd:::subcmd

    

    sal -->|subcmd| sal-init-cmd[init]
    sal -->|subcmd| sal-validate-cmd[validate]
    sal -->|subcmd| sal-build-cmd[build]
    sal --> sal-run-cmd[run]
    sal -->|subcmd| sal-push-cmd[push]
    sal -->|subcmd| sal-pull-cmd[pull]
    sal -->|subcmd| sal-clone-cmd[clone]
    sal -->|subcmd| sal-import-cmd[import]
    sal -->|subcmd| sal-salmodule-cmd[salmodule]
    
    

    sal-salmodule-cmd -->|implements| salmodule-spec
    sal-import-cmd -->|owl:imports| accepted-proto-schemes
    sal-init-cmd --> converts[converts...]
    converts -->|from| local-git-project[Local Git Repository]
    converts -->|to| local-sal-project[Local SAL Project] 
    sal-push-cmd -->|creates| oci-image
    sal-push-cmd -->|to| user-oci-registry[User's OCI Registry]
    sal-pull-cmd -->|source| user-oci-registry
    sal-pull-cmd -->|destination| dot-sal-data-dir
    sal-clone-cmd -->|source| shared-sal-project
    sal-clone-cmd -->|destination| local-sal-project
    local-sal-project -->|produces| sal-data-product[SAL Data Product]
    dot-sal-dir -->|contains| ontology-jsonld-file[ontology.jsonld]
    dot-sal-dir -->|contains| ns-prefix-version-jsonld-file[ns-prefix-versions.jsonld]
    dot-sal-dir -->|contains| dot-sal-data-dir[.sal/data directory]
    local-sal-project -->|reproducible through| git-commit-hash[Git Commit Hash]
    local-sal-project -->|versioned by| git:::cmd
    git -->|subcmd| git-commit[commit]:::subcmd
    local-sal-project -->|identified by| dot-sal-dir[.sal directory]
    sal-run-cmd -->|orchestrates| salmodule-docker-image
    sal-run-cmd -->|updates| apache-iceberg-files
    sal-build-cmd -->|depends on| sal-validate-cmd
    sal-build-cmd -->|updates| apache-iceberg-files

   
    sal-runtime-artifacts -->|contained in|dot-sal-data-dir
    subgraph sal-runtime-artifacts[SAL Runtime Artifacts]
        caddr-media-files[Content Addressable Files]
        apache-iceberg-files[Apache Iceberg data files]
    end

    subgraph shared-sal-project[Shared SAL Project]
        remote-git-repository[Remote Git Repository]
        remote-oci-image[Remote OCI Image]
    end
    sal-runtime-artifacts -->|OCI layers in| oci-image
    sal-project-base-iri[SAL Project Base IRI] -->|"based on"| git-remote-origin-url
    git-commit-hash -->|produced by| git-commit
    git -->|versions| managed-artifacts
    
    subgraph managed-artifacts[SAL Managed Artifacts]
         sal-project-src-files[Source Files - *.ttl;*.jsonld]
         ontology-jsonld-file
         ns-prefix-version-jsonld-file
    end
    subgraph dist-artifacts[SAL Distribution Artifacts]
        oci-image[OCI Image]
        salmodule-docker-image[SAL Module Docker Image]         
    end
    oci-image -->|embodiment of| sal-data-product
    salmodule-docker-image-->|implements| salmodule-spec
    subgraph salmodule-spec[SAL Module CLI Specification]
         salmodule-spec-cmd-salmodule[salmodule]
         salmodule-spec-cmd-salmodule -->|subcommand| salmodule-spec-cmd-ontology[ontology]

         salmodule-spec-cmd-salmodule -->|subcommand| salmodule-spec-cmd-run[run]
         
    end
    git -->|remote| git-remote-origin[origin]
    git-remote-origin -->|has url| git-remote-origin-url[origin URL]
    

    managed-artifacts -->|contain|rdf-entities[RDF Entities]
    rdf-entities -->|base IRI| sal-project-base-iri
    sal-run-cmd -->|expects one or more| salmodule-task-subclass-instance[SAL Module Task Subclass Instance]
    salmodule-task-subclass-instance -->|instantiates| salmodule-task-subclass-definition[SAL Module Task Subclass Definition-s]
    salmodule-task-subclass-definition -->|defined in| salmodule-remote-git-repo
    salmodule-uri -->|based on| git-remote-url
    salmodule-uri -->|references| salmodule-remote-git-repo
    subgraph salmodule-remote-git-repo[SAL_Module_Remote_Git_Repository]
         git-remote-url[Git Remote URL]
         salmodule-dockerfile[Dockerfile]
         salmodule-dockerfile -->|builds to| salmodule-docker-image
    end
    http-uri -->|references| external-ontology[Ontologies]
    oci-uri -->|references| other-sal-data-product[Other SAL Data Product]
    sal-validate-cmd -->|validates| managed-artifacts
    
    
    accepted-proto-schemes -->|referenced in| sal-project-src-files
    subgraph accepted-proto-schemes[Accepted IRI Protocol Schemes]
        http-uri[http:// or https://]
        salmodule-uri[salmodule://]
        oci-uri[oci://]
    end

 
```