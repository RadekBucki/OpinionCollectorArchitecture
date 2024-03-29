# Sequence diagram for Display visible products list
<!--
```plantuml
@startuml
    note right of ":Database"
        Display visible products list
    endnote
    
    actor       "Logged in user" as User
    participant ":UserPanel"             as UserFE
    participant ":BackendCommunication"  as BeComm
    participant ":ProductLogic"          as Product
    participant ":DatabaseCommunication" as DbComm
    participant ":Database"
    
    User    -> UserFE      : Open products page
    activate UserFE
    UserFE  -> BeComm      : getProducts(1)
    activate BeComm
    BeComm  -> Product     : getProduct(1)
    activate Product
    Product -> DbComm      : getVisibleProducts()
    activate DbComm
    DbComm  -> ":Database" : SQL SELECT
    activate ":Database"
    
    return Data
    return Product[]
    return Product[]
    return Product[]
    return Products on products page
    
@enduml
```
-->
![](media/sequence1.png)
# Sequence diagram for Display product details
<!--
```plantuml
@startuml
    note right of ":Database"
        Display product details
    endnote
    
    actor       "Logged in user" as User
    participant ":UserPanel"             as UserFE
    participant ":BackendCommunication"  as BeComm
    participant ":ProductLogic"          as Product
    participant ":DatabaseCommunication" as DbComm
    participant ":Database"
    
    User    -> UserFE      : Open product details page
    activate UserFE
    UserFE  -> BeComm      : getProductDetails(sku)
    activate BeComm
    BeComm  -> Product     : getProductsDetails(sku)
    activate Product
    Product -> DbComm      : getProductBySku(sku)
    activate DbComm
    DbComm  -> ":Database" : SQL SELECT
    activate ":Database"
    alt No product with given sku
        ":Database" -/-> DbComm : Empty list
        DbComm  -/-> Product    : Exception
        Product -/-> BeComm     : Exception
        BeComm  -/-> UserFE     : Exception
        UserFE  -/-> User       : Display error
    else Product found
        return Data
        return Product
        return Product
        return Product
        return Product details
    end
    
@enduml
```
-->
![](media/sequence2.png)
# Sequence diagram for Add opinion
<!--
```plantuml
```plantuml
@startuml
    note right of ":Database"
        Add opinion
    endnote
    
    actor       "Logged in user" as User
    participant ":UserPanel"             as UserFE
    participant ":BackendCommunication"  as BeComm
    participant ":OpinionLogic"          as Opinion
    participant ":DatabaseCommunication" as DbComm
    participant ":Database"
    
    User    -> UserFE              : Add product opinion
    activate UserFE
    UserFE  -> BeComm              : addOpinion(opinion)
    activate BeComm
    BeComm  -> Opinion             : addOpinion(opinion)
    activate Opinion
    alt Form validation failed
        UserFE -/-> User            : Display error
    else Form validation successful
        alt Authorization failed
            Opinion -/-> BeComm         : Exception
            BeComm -/-> UserFE          : Exception
            UserFE -/-> User            : Display error
        else Request validation failed
            Opinion -/-> BeComm         : Exception
            BeComm -/-> UserFE          : Exception
            UserFE -/-> User            : Display error
        else Request validation positive and Authorization successful
            Opinion -> DbComm          : addProductOpinion(opinion)
            activate DbComm
            DbComm  -> ":Database"     : SQL INSERT
            activate ":Database"
            alt Database error
                ":Database" -/-> DbComm : Exception
                DbComm -/-> Opinion     : Exception
                Opinion -/-> BeComm     : Exception
                BeComm -/-> UserFE      : Exception
                UserFE -/-> User        : Display error
            else Database successresponse
                ":Database" -/-> DbComm : Data
                deactivate ":Database"
                DbComm -/-> Opinion     : Opinion
                deactivate DbComm
                Opinion -/-> BeComm     : Opinion
                deactivate Opinion
                BeComm -/-> UserFE      : Opinion
                deactivate BeComm
                UserFE -/-> User        : Display success
                deactivate UserFE
            end
        end
    end
    
@enduml
```
-->
![](media/sequence3.png)
# Activity diagram for add opinion
<!--
```plantuml
@startuml

start
:Get Opinion from form filled by user;
if (Is form validation successful?) then ([yes])
    :Call to backend with Opinion;
    if (Is non admin user?) then ([yes])
        if (Is validation succesful?) then ([yes])
            :Return success;
            :Handle and return response;
            :Display success;
            stop;
        else ([no])
            :Return error;
        endif;
    else ([no])
        :Return error;
    endif;
else ([no])
    :Return error;
endif;
:Handle and return response;
end;

@enduml
```
-->
![](media/activity.png)

# State machine diagram
<!--
```plantuml
@startuml
[*] -> Unauthorized

state Unauthorized {
  Unauthorized -/-> Authorizing : Request
}

state Authorizing {
  Authorizing -/-> Authorized : Request authorized
  Authorizing -/-> Unauthorized : Request failed
}

state Authorized {
  Authorized -/-> Unauthorized : Token expired
}

@enduml
```
-->
![](media/stateMachine.png)
# Interaction overview diagram

<!---
```plantuml
@startuml
Start

:Display visible products list
{{
    actor       User
    participant ":UserPanel"             as UserFE
    participant ":BackendCommunication"  as BeComm
    participant ":ProductLogic"          as Product
    participant ":DatabaseCommunication" as DbComm
    participant ":Database"
    
    User    -> UserFE      : Open product details page
    activate UserFE
    UserFE  -> BeComm      : getProductDetails(sku)
    activate BeComm
    BeComm  -> Product     : getProductsDetails(sku)
    activate Product
    Product -> DbComm      : getProductBySku(sku)
    activate DbComm
    DbComm  -> ":Database" : SQL SELECT
    activate ":Database"
    alt No product with given sku
        ":Database" -> DbComm : Empty list
        DbComm  -> Product    : Exception
        Product -> BeComm     : Exception
        BeComm  -> UserFE     : Exception
        UserFE  -> User       : Display error
    else Product found
        return Data
        return Product
        return Product
        return Product
        return Product details
    end
}}
;
if () then ([display product details])
    :Display product details
    {{
        actor       User
        participant ":UserPanel"             as UserFE
        participant ":BackendCommunication"  as BeComm
        participant ":ProductLogic"          as Product
        participant ":DatabaseCommunication" as DbComm
        participant ":Database"
        
        User    -> UserFE      : Open product details page
        activate UserFE
        UserFE  -> BeComm      : getProductDetails(sku)
        activate BeComm
        BeComm  -> Product     : getProductsDetails(sku)
        activate Product
        Product -> DbComm      : getProductBySku(sku)
        activate DbComm
        DbComm  -> ":Database" : SQL SELECT
        activate ":Database"
        alt No product with given sku
            ":Database" -/-> DbComm : Empty list
            DbComm  -/-> Product    : Exception
            Product -/-> BeComm     : Exception
            BeComm  -/-> UserFE     : Exception
            UserFE  -/-> User       : Display error
        else Product found
            return Data
            return Product
            return Product
            return Product
            return Product details
        end
    }}
    ;
    if () then ([add opinion])
        :Add opinion
        {{
            actor       "Logged in User" as User
            participant ":UserPanel"             as UserFE
            participant ":BackendCommunication"  as BeComm
            participant ":OpinionLogic"          as Opinion
            participant ":DatabaseCommunication" as DbComm
            participant ":Database"
            
            User    -> UserFE              : Add product opinion
            activate UserFE
            UserFE  -> BeComm              : addOpinion(opinion)
            activate BeComm
            BeComm  -> Opinion             : addOpinion(opinion)
            activate Opinion
            alt Authorization failed
                Opinion -/-> BeComm         : Exception
                BeComm -/-> UserFE          : Exception
                UserFE -/-> User            : Display error
            else Request validation failed
                Opinion -/-> BeComm         : Exception
                BeComm -/-> UserFE          : Exception
                UserFE -/-> User            : Display error
            else Request validation positive
                Opinion -> DbComm          : addProductOpinion(opinion)
                activate DbComm
                DbComm  -> ":Database"     : SQL INSERT
                activate ":Database"
                alt Database error
                    ":Database" -/-> DbComm : Exception
                    DbComm -/-> Opinion     : Exception
                    Opinion -/-> BeComm     : Exception
                    BeComm -/-> UserFE      : Exception
                    UserFE -/-> User        : Display error
                else Database successresponse
                    ":Database" -/-> DbComm : Data
                    deactivate ":Database"
                    DbComm -/-> Opinion     : Opinion
                    deactivate DbComm
                    Opinion -/-> BeComm     : Opinion
                    deactivate Opinion
                    BeComm -/-> UserFE      : Opinion
                    deactivate BeComm
                    UserFE -/-> User        : Display success
                    deactivate UserFE
                end
            end
        }}
        ;
    else ([back to products list])
        stop;
    endif;
else ([not displaing detail of any product])
    stop;
    endif;
Stop
@enduml
```
--->
![](media/interactionOverview.png)

# Structural diagram for Product parts
<!--
```plantuml
card Product {
    card sku
    card name
    card description
    card opinionAvg
    card pictureUrl
    card visible
    card user
    
    card category
    card opinion
    card suggestion
}
sku "1" -- "1" name
sku "1" -- "1" description
sku "1" -- "1" opinionAvg
sku "1" -- "1" pictureUrl
sku "1" -- "1" visible
sku "1" -- "1" user

sku "1" -- "0..*" category
sku "1" -- "0..*" opinion
sku "1" -- "0..*" suggestion
```
-->
![](media/structural.png)

# Timing diagram for get product list
<!--
```plantuml
@startuml
robust "Backend" as BE
robust "Frontend" as FE
scale 100 as 30 pixels

@0
BE is Waiting
FE is Idle

@16
BE is Waiting
FE is Processing

@2630
FE -> BE : Get product list
BE is Processing
FE is Waiting

@3380
BE is Waiting
FE is Processing

@3900
BE is Waiting
FE is Processing

@enduml
```
-->
![](media/timing.png)