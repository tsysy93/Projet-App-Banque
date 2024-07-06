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

Schéma global de l'architecture du logiciel

> Ce schéma est global, il permet uniquement de voir l'ensemble des liaisons entre les services et les modules

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

Cette partie concerne uniquement l'explication ainsi que l'explication de chaque service

### Dépendance du Core <a id="CoreDependance"/>

Ce service est le coeur logiciel, celui-ci a pour rôle d'initialiser ainsi que de manager tous les autres services.

> Les services présents dans le coeur logiciel doivent être uniques (singleton)

``` mermaid
flowchart LR
    idCore <--> ModuleManager
    idCore-->Common
    idCore ---> Repository{{Repository}}
    idCore --> IdConfig>Config]
```

### Repository <a id="RepositoryDependance">

Ce service est le manager des données il possède différents rôles :
 - Lié le logiciel à ça base de données
 - Créer les nouveaux objets
 - Supprimer les objets
 - Modifier les objets

``` mermaid
flowchart LR
    Repository <--> Database[(Database)]
    Repository -.Include.-> Config
```

### UI <a id="UiDependance">

Ce service regroupé uniquement tous les composants graphique du logiciel

``` mermaid
flowchart LR
    CustomerModule & AuthModule ----> UiService
```

---

## Modules <a id="ModulesChapter">

### Module d'authentification <a id="AuthModule"/>

Ce module permet à l'utilisateur de se connecter, ainsi que d'identifier ces droits dans l'application

``` mermaid
flowchart LR
    ModuleManager --Call--> AuthModule
    AuthModule --Include--> Common
    AuthModule --> UiService
    AuthModule --> Repository
```
---

### Module clients <a id="CustomerModule"/>

Ce module permet à un banquier d'accéder au dossier de ses clients

``` mermaid
flowchart LR
ModuleManager --Call--> CustomerModule
CustomerModule --Include--> Common
CustomerModule --> UiService
CustomerModule --> Repository
```

### Module Responsable <a id="ManagerModule"/>

Ce module permet à un responsable de consulter les dossiers, les résultats, les demandes d'un subordonner

``` mermaid
flowchart LR
    ModuleManager --Include----> ManagerModule
    ManagerModule --Include----> Repository
    ManagerModule --Include----> UiService
    ManagerModule --Include----> Common
```