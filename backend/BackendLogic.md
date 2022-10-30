# Backend logic layer

```plantuml
@startuml
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
@enduml 
```

## UserFacade
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
            - PUT /users/update - update user data
            Uses DatabaseCommunication component to data operations.
        endnote
        
    }
}
@enduml 
```

```plantuml
@startuml
component Backend {
    component DatabaseCommunication {
        interface DatabaseCommunictionFacade <<interface>> {
            + getAllUsers() : User[]
            + getUserByToken(token: String) : User
            + createUser(firstName: String, lastName: String, email: String, passwordHash: String, profilePictureUrl: String, isAdmin: Boolean) : User
            + getUserToken(email: String, passwordHash: String) : String
            + addUserToken(userId: Integer, token: String) : String
            + updateUser(userId: Integer, firstName: String, lastName: String, email: String, passwordHash: String, profilePictureUrl: String, isAdmin: Boolean) : User
        }
    }
    component UserLogic {
        note as Authors
            Projectants: 
             - Michał Andrzejczak
             - Mateusz Krasiński
        endnote
        
        UserFacade --> DatabaseCommunictionFacade
        
        interface UserFacade <<interface>> {
            + getAllUsers() : User[]
            + getUserByToken(token: String) : User
            + register(firstName: String, lastName: String, email: String, password: String, profilePictureUrl: String): User
            + registerAdmin(firstName: String, lastName: String, email: String, password: String, profilePictureUrl: String): User
            + login(email: String, password: String): String
            + getUserByToken(token: String): User
            + updateUser(userId: Integer, firstName: String, lastName: String, email: String, passwordHash: String, profilePictureUrl: String, isAdmin: Boolean) : User
        }
    
        note top of UserFacade
            Currently logged in user passed by consutructor. Null if current sessionis for guest.
            Only for admin user can, perform getAllUsers, registerAdmin and updateUser operations
        endnote
    
        note left of UserFacade::register
            Validate password length and complexity.
        endnote
    
        note left of UserFacade::registerAdmin
            Verify that admin user creates next admin.
        endnote
    
        note left of UserFacade::login
            Returns user token or external token.
        endnote
    }
}
@enduml 
```

## ProductFacade
```plantuml
@startuml
component Backend {    
    component ProductLogic {
        note as Authors
            Projectants: 
             - Filip Grzelak
             - Damian Biskupski
        endnote
        note as Description
            Creates endpoints for:
            - GET /products - get products
            - GET /products/all - get all products
            - GET /products/search - search products
            - GET /products/details - get products details
            - POST /products/add - add product
            - PUT /products/edit - edit product
            - DELETE /products/delete - remove product
            - POST /categories/add - add category
            - PUT /categories/edit - edit category
            - DELETE /categories/delete - remove category
            Uses DatabaseCommunication component to data operations.
        endnote
    }
}
@enduml 
```

```plantuml
@startuml
component Backend {
    component DatabaseCommunication {
        interface DatabaseCommunictionFacade <<interface>> {
            + getProductBySku(sku: String) : Product
            + getAllProducts() : Product[]
            + getVisibleProducts() : Product[]
            + getProductsFilterProducts(categoryName: String, searchPhrase: String, opinionAvgMin: Integer, opinionAvgMax: Integer) : Product[]
            + createProduct(authorId: Integer, sku: String, name: String, pictureUrl: String, description: String, categoryNames: String[], visible: Boolean) : Product
            + updateProduct(authorId: Integer, sku: String, name: String, pictureUrl: String, description: String, categoryNames: String[], visible: Boolean) : Product
            + removeProduct(sku: String)

            + createCategory(categoryName: String, visible: Boolean) : Category
            + updateCategory(categoryName: String, visible: Boolean): Category
            + removeCategory(categoryName: String)
        }
    }
    
    component ProductLogic {        
        ProductFacade --> DatabaseCommunictionFacade
        interface ProductFacade <<interface>> {
            + getProductBySku(sku: String) : Product
            + getAllProducts() : Product[]
            + getProducts() : Product[]
            + getProductsFiltered(categoryName: String, searchPhrase: String, opinionAvgMin: Integer, opinionAvgMax: Integer) : Product[]
            + addProduct(sku: String, name: String, pictureUrl: String, description: String, categoryNames: String[], visible: Boolean) : Product
            + editProduct(sku: String, name: String, pictureUrl: String, description: String, categoryNames: String[], visible: Boolean) : Product
            + removeProduct(sku: String)
            
            + addCategory(categoryName: String, visible: Boolean) : Category
            + editCategory(categoryName: String, visible: Boolean) : Category
            + removeCategory(categoryName: String)
        }
    
        note top of ProductFacade
            Currently logged in user passed by consutructor. Null if current sessionis for guest.
            Verify that user passed by constructor is admin and can perform add, edit
            and remove operations.
        endnote
    }
}
@enduml 
```

## SuggestionFacade

```plantuml
@startuml
component Backend {
    component SuggestionLogic {
        note as Authors
            Projectants: 
             - Paweł Wieczorek
             - Julian Woroniecki
        endnote
        note as Description
            Creates endpoints for:
            - GET /suggestions/user - get user suggestions
            - POST /suggestions/add - add suggestion
            - GET /suggestions/get - get all suggestions
            - PUT /suggestions/reply - reply to suggestion
            Uses DatabaseCommunication component to data operations.
        endnote
}
@enduml 
```

```plantuml
@startuml
component Backend {
    component DatabaseCommunication {
        interface DatabaseCommunictionFacade <<interface>> {
            + getAllSuggestions() : Suggestion[]
            + getUserSugestions(userId: Integer) : Sugestion[]
            + addSuggestion(productId: Integer, userId: Integer, suggestionDescription: String) : Suggestion
            + replySuggestion(suggestiontId:Integer, suggestionReviewerId: Integer, suggestionStatus: String, suggestionReply: String)
        }
    }
    
    component SuggestionLogic {
        
        SuggestionFacade --> DatabaseCommunictionFacade
        interface SuggestionFacade <<interface>> {
            + getUserSugestions() : Sugestion[]
            + addSuggestion(product: Product, suggestionDescription: String) : Suggestion
            + getAllSuggestions() : Suggestion[]
            + replySuggestion(suggestiontId:Integer, suggestionStatus: String, suggestionReply: String)
        }
    
        note top of SuggestionFacade
            Currently logged in user passed by consutructor. Null if current sessionis for guest.
            Verify that user passed by constructor is:
             - admin to perform  getAllSuggestions and replySuggestion operations
             - logged in not adminto perform getUserSugestions and addSuggestion.
        endnote
    }
}
@enduml 
```

## OpinionFacade

```plantuml
@startuml
component Backend {
    component OpinionLogic {
        note as Authors
            Projectants: 
             - Paweł Wieczorek
             - Julian Woroniecki
        endnote
        note as Description
            Creates endpoints for:
            - GET /suggestions/user - get user suggestions
            - POST /suggestions/add - add suggestion
            - GET /suggestions/get - get all suggestions
            - PUT /suggestions/reply - reply to suggestion
            - GET /opinions/user - get user opinions
            - GET /opinions/product - get product opinions
            - POST /opinions/add - add opinion
            Uses DatabaseCommunication component to data operations.
        endnote
    }
}
@enduml 
```

```plantuml
@startuml
component Backend {
    component DatabaseCommunication {
        interface DatabaseCommunictionFacade <<interface>> {
            + getProductOpinions(sku: String) : Opinion[]
            + addProductOpinion(opinionValue: Integer, opinionDescription: String, opinionPicture: String, advatages: String[], disadvantages: String[]) : Opinion
            + getUserOpinions(userId: Integer) : Opinion[]
        }
    }
    
    component OpinionLogic {        
        OpinionFacade  --> DatabaseCommunictionFacade
        interface OpinionFacade <<interface>> {
            + getProductOpinions(product: Product) : Opinion[]
            + addProductOpinion(opinionValue: Integer, opinionDescription: String, opinionPicture: String, advatages: String[], disadvantages: String[]) : Opinion
            + getUserOpinions(user: User) : Opinion[]
        }
    
        note top of OpinionFacade
            Currently logged in user passed by consutructor. Null if current sessionis for guest.
            Verify that user passed by constructor is:
             - logged in not adminto perform addProductOpinion and getUserOpinions operation.
        endnote
    }
}
@enduml 
```