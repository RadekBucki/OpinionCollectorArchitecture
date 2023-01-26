# Backend
## Components diagram
<!--
```plantuml
@startuml
() Users
() Products
() Suggestions
() Opinions
component Backend {
    component UserLogic {
    }
    component ProductLogic {
    }
    component OpinionLogic {
    }
    component SuggestionLogic {
    }
    
    component DatabaseCommunication {
    }
}

component Database {
}

DatabaseCommunication -(0-  Database : SQL

UserLogic       -(0-  DatabaseCommunication : DatabaseCommuniction
ProductLogic    -(0-  DatabaseCommunication : DatabaseCommuniction
OpinionLogic    -(0-  DatabaseCommunication : DatabaseCommuniction
SuggestionLogic -(0-  DatabaseCommunication : DatabaseCommuniction

Users -- UserLogic
Products -- ProductLogic
Suggestions -- SuggestionLogic
Opinions --OpinionLogic
@enduml 
```
-->
![](media/components.png)
## Table of Contents:
1. [Database Communication](DatabaseCommunication)
2. [Users](Users)
3. [Products and Categories](ProductsAndCategories)
4. [Suggestions](Suggestions)
5. [Opinions](Opinions)