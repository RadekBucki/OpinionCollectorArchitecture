# Main functionalities diagrams
## Sequence diagram for Display visible products list
```plantuml
@startuml
    note right of DB
        Display visible products list
    endnote
    
    actor       User
    participant ":UserPage"              as UserFE
    participant ":BackendCommunication"  as BeComm
    participant ":ProductLogic"          as Product
    participant ":DatabaseCommunication" as DbComm
    participant ":Database"              as DB
    
    User    -> UserFE  : Open products page
    activate UserFE
    UserFE  -> BeComm  : getProducts(1)
    activate BeComm
    BeComm  -> Product : getProduct(1)
    activate Product
    Product -> DbComm  : getVisibleProducts()
    activate DbComm
    DbComm  -> DB      : SQL SELECT
    activate DB
    
    return Data
    return Product[]
    return Product[]
    return Product[]
    return Products on products page
    
@enduml
```
## Sequence diagram for Add opinion
```plantuml
```plantuml
@startuml
    note right of DB
        Add opinion
    endnote
    
    actor       "Logged in User" as User
    participant ":UserPage"              as UserFE
    participant ":BackendCommunication"  as BeComm
    participant ":OpinionLogic"          as Opinion
    participant ":DatabaseCommunication" as DbComm
    participant ":Database"              as DB
    
    User    -> UserFE  : Add product opinion
    activate UserFE
    UserFE  -> BeComm  : addOpinion(opinion)
    activate BeComm
    alt Authorization failed
        BeComm --> UserFE : Exception
        UserFE --> User   : Display error
    else Request validation failed
        BeComm --> UserFE : Exception
        UserFE --> User   : Display error
    else Request validation positive
        BeComm  -> Opinion : addOpinion(opinion)
        activate Opinion
        Opinion -> DbComm  : addProductOpinion(opinion)
        activate DbComm
        DbComm  -> DB      : SQL INSERT
        activate DB
        alt Database error
            DB --> DbComm      : Exception
            DbComm --> Opinion : Exception
            Opinion --> BeComm : Exception
            BeComm --> UserFE  : Exception
            UserFE --> User    : Display error
        else Database successresponse
            DB --> DbComm      : Data
            deactivate DB
            DbComm --> Opinion : Opinion
            deactivate DbComm
            Opinion --> BeComm : Opinion
            deactivate Opinion
            BeComm --> UserFE  : Opinion
            deactivate BeComm
            UserFE --> User    : Display success
            deactivate UserFE
        end
    end
    
@enduml
```