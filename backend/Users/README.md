# Backend - User Logic
## Description
<!--
```plantuml
@startuml
component Backend {
    component UserLogic {
        note as Authors
            Projectants: 
             - Michał Andrzejczak
             - Mateusz Krasiński
        endnote
        note as Description
            Creates endpoints for:
            - GET /users - get all users data
            - POST /users/register - register new user or admin user
            - GET /users/login - get user data and token
            - PUT /users/update/{id} - update user data
            Uses DatabaseCommunication component to data operations.
        endnote
        
    }
}
@enduml 
```
-->
![](media/description.png)
## API
<!--
```plantuml
@startuml
component Frontend {
    component BackendCommunication {
    }
}

component Backend {
    component UserLogic {
    }
}

interface Users <<interface>> {
    + getAllUsers() : User[]
    + register(firstName: String, lastName: String, email: String, password: String, profilePictureUrl: String, isAdmin: Boolean): User
    + login(email: String, password: String): String
    + updateUser(userId: Integer, firstName: String, lastName: String, email: String, passwordHash: String, profilePictureUrl: String, isAdmin: Boolean) : User
}

BackendCommunication ..>   Users
Users                 <|... UserLogic

@enduml 
```
-->
![](media/api1.png)
<!--
```plantuml
@startuml
    component UserLogic {
    }
    component ProductLogic {
    }
    component OpinionLogic {
    }
    component SuggestionLogic {
    }
    
    interface UserAuth <<interface>> {
        + getUserByToken(token: String) : User
    }
    
    note left of UserAuth
        Used to Authentication
        given token on endpoints.
    endnote
    
    ProductLogic    ..>  UserAuth
    OpinionLogic    ..>  UserAuth
    SuggestionLogic ..>  UserAuth
    UserAuth      <|.. UserLogic
@enduml 
```
-->
![](media/api2.png)
## Class diagrams
<!--
```plantuml
circle Users
component Backend {
    component UserLogic {
        class UserController {
            + getAllUsers() : User[]
            + register(firstName: String, lastName: String, email: String, password: String, profilePictureUrl: String, isAdmin: Boolean): User
            + login(email: String, password: String): String
            + update(userId: Integer, firstName: String, lastName: String, email: String, passwordHash: String, profilePictureUrl: String, isAdmin: Boolean) : User
        }
        circle UserFacade
        circle UserAuth
        class UserFacadeImpl {
            + getAllUsers() : User[]
            + register(firstName: String, lastName: String, email: String, password: String, profilePictureUrl: String): User
            + registerAdmin(firstName: String, lastName: String, email: String, password: String, profilePictureUrl: String): User
            + login(email: String, password: String): String
            + updateUser(userId: Integer, firstName: String, lastName: String, email: String, passwordHash: String, profilePictureUrl: String, isAdmin: Boolean) : User
            + getUserByToken(token: String) : User
        }
        UserFacade -- UserFacadeImpl
        UserAuth   -- UserFacadeImpl
        Users -- UserController
        
        UserController "1" o-- "1" UserFacadeImpl
        
        class Mapper {
            +map (object: Object) : Object
        }
        UserController ..> Mapper
    }
    
    component DatabaseCommunication {
        class DatabaseCommunictionFacadeImplementation
        class User
    }
    UserFacadeImpl -(0- DatabaseCommunictionFacadeImplementation : DatabaseCommunication
    UserFacadeImpl ..> User
    
}
```
-->
![](media/class.png)