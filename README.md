# System architecture

Designed with [PlantUML](https://plantuml.com/)

## Layers

```plantuml
@startuml
component Frontend {
    component UserPanel {
    }
    
    component AdminPanel {
    }
    
    component BackendCommunication {
    }
}

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

DatabaseCommunication ...> Database: SQL

UserLogic             -->  DatabaseCommunication
ProductLogic          -->  DatabaseCommunication
OpinionLogic          -->  DatabaseCommunication
SuggestionLogic       -->  DatabaseCommunication

BackendCommunication  ...> UserLogic: JSON
BackendCommunication  ...> ProductLogic: JSON
BackendCommunication  ...> OpinionLogic: JSON
BackendCommunication  ...> SuggestionLogic: JSON

UserPanel             -->  BackendCommunication
AdminPanel            -->  BackendCommunication

@enduml 
```

## Deployment diagram
```plantuml
@startuml
left to right direction

node Database as database
node Server as server
node Browser as browser

database -- server
server -- browser

@enduml 
```

## Usages diagrams

### Not logged in user
```plantuml
@startuml

left to right direction

actor "Not logged in user" AS customer

rectangle Application {
    customer -- (User account)
    customer -- (Products search)
    customer -- (Product browse)
    
    (User account) --> (Register) : include
    (User account) --> (Log in) : include
    
    (Products search) --> (Display visible products list) : include
    (Products search) --> (Filtering visible products list) : include
    
    (Product browse) --> (Display product details) : include
    (Product browse) --> (Display product opinions) : include
}

@enduml
```

### Logged in user
```plantuml
@startuml

left to right direction

actor "Logged in user" AS customer

rectangle Application {
    customer -- (Products search)
    customer -- (User account)
    customer -- (Product browse)
    
    (Products search) --> (Display visible products list) : include
    (Products search) --> (Filtering visible products list) : include
    
    (Product browse) --> (Display product details) : include
    (Product browse) --> (Display product opinions) : include
    (Product browse) --> (Add opinion) : include
    (Product browse) --> (Add suggestions) : include
    
    (User account) --> (Display user suggestions) : include
    (User account) --> (Display user opinions) : include
}

@enduml
```

### Logged in admin user
```plantuml
@startuml

left to right direction

actor Admin AS admin

rectangle Application {
    admin -- (Products search)
    admin -- (Product browse)
    admin -- (Admin Panel)
    
    (Products search) --> (Display visible products list) : include
    (Products search) --> (Filtering visible products list) : include
    
    (Product browse) --> (Display product details) : include
    (Product browse) --> (Display product opinions) : include
    
    (Admin Panel) --> (Users) : include
    (Admin Panel) --> (Products) : include
    (Admin Panel) --> (Categories) : include
    (Admin Panel) --> (Suggestions) : include
    
    (Users) --> (Display users list) : include
    (Users) --> (Edit user) : include
    (Users) --> (Add user) : include
    
    (Products) --> (Display products list) : include
    (Products) --> (Add product) : include
    (Products) --> (Edit product) : include
    (Products) --> (Remove product) : include
    
    (Categories) --> (Display category list) : include
    (Categories) --> (Add category) : include
    (Categories) --> (Edit category) : include
    (Categories) --> (Remove category) : include
    
    (Suggestions) --> (Display suggestions list) : include
    (Suggestions) --> (React to suggestion) : include
}

@enduml
```