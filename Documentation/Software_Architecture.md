# Architecture logiciel

## Table des matières

- [Achitechture globale](#TotalArchitecture)
- [Services](#ServicesChapter)
    - [Dépendance du core](#CoreDependance)
    - [Repository](#RepositoryDependance)
    - [Ui](#UiDependance)
- [Modules](#ModulesChapter)
    - [Module d'Authenfication](#AuthModule)
    - [Module Clients](#CustomerModule)
    - [Module Responsalbe](#ManagerModule)
---

## Architecture globale <a id="TotalArchitecture"/>

``` mermaid
flowchart LR
    idCore <--> ModuleManager
    Program --> idCore{Core}
    ModuleManager --Call---> AuthModule
    idCore-->Common
    AuthModule --Include----> Common
    idCore ---> Repository{{Repository}}
    Repository <---> Database[(Database)]
    idCore --> IdConfig>Config]
    Repository -.Need.-> IdConfig
    ModuleManager --Call--> CustomerModule
    CustomerModule --Include----> Common
    CustomerModule & AuthModule ----> UiService
    AuthModule --> Repository
    CustomerModule --Include----> Repository
    ModuleManager --Include----> ManagerModule
    ManagerModule --Include----> Repository
    ManagerModule --Include----> UiService
    ManagerModule --Include----> Common
```

---

## Services <a id="ServicesChapter">

### Dépendance du Core <a id="CoreDependance"/>

``` mermaid
flowchart LR
    idCore <--> ModuleManager
    idCore-->Common
    idCore ---> Repository{{Repository}}
    idCore --> IdConfig>Config]
```

### Repository <a id="RepositoryDependance">

``` mermaid
flowchart LR
    Repository <--> Database[(Database)]
    Repository -.Include.-> Config
```


### UI <a id="UiDependance">

``` mermaid
flowchart LR
    CustomerModule & AuthModule ----> UiService
```

---

## Modules <a id="ModulesChapter">

### Module d'authentification <a id="AuthModule"/>

``` mermaid
flowchart LR
    ModuleManager --Call--> AuthModule
    AuthModule --Include--> Common
    AuthModule --> UiService
    AuthModule --> Repository
```
---

### Module clients <a id="CustomerModule"/>

``` mermaid
flowchart LR
ModuleManager --Call--> CustomerModule
CustomerModule --Include--> Common
CustomerModule --> UiService
CustomerModule --> Repository
```

### Module Responsable <a id="ManagerModule"/>

``` mermaid
flowchart LR
    ModuleManager --Include----> ManagerModule
    ManagerModule --Include----> Repository
    ManagerModule --Include----> UiService
    ManagerModule --Include----> Common
```