# Components diagram
<!--
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

UserLogic             -(0- DatabaseCommunication : DatabaseCommunication
ProductLogic          -(0- DatabaseCommunication : DatabaseCommunication
OpinionLogic          -(0- DatabaseCommunication : DatabaseCommunication
SuggestionLogic       -(0- DatabaseCommunication : DatabaseCommunication

BackendCommunication  -(0- UserLogic       : Users
BackendCommunication  -(0- ProductLogic    : Products
BackendCommunication  -(0- OpinionLogic    : Opinions
BackendCommunication  -(0- SuggestionLogic : Suggestions

ProductLogic          -(0- UserLogic : UserAuth
OpinionLogic          -(0- UserLogic : UserAuth
SuggestionLogic       -(0- UserLogic : UserAuth

UserPanel             -(0- BackendCommunication : BackendCommunication
AdminPanel            -(0- BackendCommunication : BackendCommunication

@enduml 
```
-->
![](media/components.png)

# Packages diagram
<!--
```plantuml
@startuml
package Frontend {
    package UserView {
        package UserRegistration
        package UserLogin
        package UserPanel {
            package UserData
            package UserOpinions
            package UserSuggestions
        }
        package ProductDetailPage
        package ProductListPage
    }
    
    package AdminView {
        package Products
        package Categories
        package Suggestions
        package Users
    }
    
    package BackendCommunication {
        package GetRequests
        package PostRequests
        package PutRequests
        package DeleteRequests
    }
}

package Backend {
    package UserLogic {
        package User
        package Security
    }
    package ProductLogic {
        package Category
        package Product
    }
    package OpinionLogic {
    }
    package SuggestionLogic {
    }
    
    package DatabaseCommunication {
        package Category as DbCategory
        package Opinion as DbOpinion
        package Product as DbProduct
        package Suggestion as DbSuggestion
        package User as DbUser
    }
}

package Database

AdminView ..> BackendCommunication : <<import>>

UserRegistration  ...> PostRequests : <<import>>
UserLogin         ...> PostRequests : <<import>>
UserOpinions      ...> GetRequests  : <<import>>
UserSuggestions   ...> GetRequests  : <<import>>
ProductListPage   ...> GetRequests  : <<import>>
ProductDetailPage ...> GetRequests  : <<import>>
ProductDetailPage ...> PostRequests : <<import>>

BackendCommunication ...> User            : <<import>>
BackendCommunication ...> Category        : <<import>>
BackendCommunication ...> Product         : <<import>>
BackendCommunication ...> OpinionLogic    : <<import>>
BackendCommunication ...> SuggestionLogic : <<import>>

User            ...> Security : <<import>>
Category        ...> Security : <<import>>
Product         ...> Security : <<import>>
OpinionLogic    ...> Security : <<import>>
SuggestionLogic ...> Security : <<import>>

User            ...> DatabaseCommunication : <<import>>
Security            ...> DatabaseCommunication : <<import>>
Category        ...> DatabaseCommunication : <<import>>
Product         ...> DatabaseCommunication : <<import>>
OpinionLogic    ...> DatabaseCommunication : <<import>>
SuggestionLogic ...> DatabaseCommunication : <<import>>

DbCategory   ...> Database : <<import>>
DbOpinion    ...> Database : <<import>>
DbProduct    ...> Database : <<import>>
DbSuggestion ...> Database : <<import>>
DbUser       ...> Database : <<import>>

@enduml 
```
-->
![](media/packages.png)

# Deployment diagram
<!--
```plantuml
@startuml

node "Database Server" <<device>> as database {
    component Database
}
node Server <<device>> as server {
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
}
node Browser as browser {
    component Frontend {
        component UserPanel {
        }
        
        component AdminPanel {
        }
        
        component BackendCommunication {
        }
    }
}

DatabaseCommunication -(0- Database: SQL

UserLogic             -(0- DatabaseCommunication : DatabaseCommunication
ProductLogic          -(0- DatabaseCommunication : DatabaseCommunication
OpinionLogic          -(0- DatabaseCommunication : DatabaseCommunication
SuggestionLogic       -(0- DatabaseCommunication : DatabaseCommunication

BackendCommunication  -(0- UserLogic       : Users
BackendCommunication  -(0- ProductLogic    : Products
BackendCommunication  -(0- OpinionLogic    : Opinions
BackendCommunication  -(0- SuggestionLogic : Logics

ProductLogic          -(0- UserLogic : UserAuth
OpinionLogic          -(0- UserLogic : UserAuth
SuggestionLogic       -(0- UserLogic : UserAuth

UserPanel             -(0- BackendCommunication : BackendCommunication
AdminPanel            -(0- BackendCommunication : BackendCommunication

@enduml 
```
-->
![](media/deployment.png)