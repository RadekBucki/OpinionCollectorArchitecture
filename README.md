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

DatabaseCommunication -(0- Database: SQL

UserLogic             -(0- DatabaseCommunication : DatabaseCommunicationFacade
ProductLogic          -(0- DatabaseCommunication : DatabaseCommunicationFacade
OpinionLogic          -(0- DatabaseCommunication : DatabaseCommunicationFacade
SuggestionLogic       -(0- DatabaseCommunication : DatabaseCommunicationFacade

BackendCommunication  -(0- UserLogic       : UserFacade
BackendCommunication  -(0- ProductLogic    : ProductFacade
BackendCommunication  -(0- OpinionLogic    : OpinionFacade
BackendCommunication  -(0- SuggestionLogic : LogicFacade

ProductLogic          -(0- UserLogic : UserFacade
OpinionLogic          -(0- UserLogic : UserFacade
SuggestionLogic       -(0- UserLogic : UserFacade

UserPanel             -(0- BackendCommunication : BackendCommunicationFacade
AdminPanel            -(0- BackendCommunication : BackendCommunicationFacade

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
    customer -- (Manage user)
    customer -- (Search products)
    customer -- (Browse product)
    
    (Manage user) <.. (Register): <<extend>>
    (Manage user) <.. (Log in): <<extend>>
    
    (Search products) <.. (Display visible products list): <<extend>>
    (Search products) <.. (Filter visible products list): <<extend>>
    
    (Browse product) <.. (Display product details): <<extend>>
    (Browse product) <.. (Display product opinions): <<extend>>
}

@enduml
```

### Logged in user
```plantuml
@startuml

left to right direction

actor "Logged in user" AS customer

rectangle Application {
    customer -- (Search products)
    customer -- (Manage user)
    customer -- (Browse product)
    
    (Search products) <.. (Display visible products list): <<extend>>
    (Search products) <.. (Filter visible products list): <<extend>>
    
    (Browse product) <.. (Display product details): <<extend>>
    (Browse product) <.. (Display product opinions): <<extend>>
    (Browse product) <.. (Add opinion): <<extend>>
    (Browse product) <.. (Add suggestions): <<extend>>
    
    (Manage user) <.. (Display user suggestions): <<extend>>
    (Manage user) <.. (Display user opinions): <<extend>>
}

@enduml
```

### Logged in admin user
```plantuml
@startuml

left to right direction

actor Admin AS admin

rectangle Application {
    admin -- (Search products)
    admin -- (Browse product)
    admin --  (Administer users)
    admin --  (Administer products)
    admin --  (Administer categories)
    admin --  (Administer suggestions)
    
    (Search products) <.. (Display visible products list): <<extend>>
    (Search products) <.. (Filter visible products list): <<extend>>
    
    (Browse product) <.. (Display product details): <<extend>>
    (Browse product) <.. (Display product opinions): <<extend>>
    
    (Administer users) <.. (Display users list): <<extend>>
    (Administer users) <.. (Edit user): <<extend>>
    (Administer users) <.. (Add user): <<extend>>
    
    (Administer products) <.. (Display products list): <<extend>>
    (Administer products) <.. (Add product): <<extend>>
    (Administer products) <.. (Edit product): <<extend>>
    (Administer products) <.. (Remove product): <<extend>>
    
    (Administer categories) <.. (Display category list): <<extend>>
    (Administer categories) <.. (Add category): <<extend>>
    (Administer categories) <.. (Edit category): <<extend>>
    (Administer categories) <.. (Remove category): <<extend>>
    
    (Administer suggestions) <.. (Display suggestions list): <<extend>>
    (Administer suggestions) <.. (React to suggestion): <<extend>>
}

@enduml
```