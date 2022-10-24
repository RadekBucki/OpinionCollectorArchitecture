# Backend logic layer

```plantuml
@startuml
component Backend {
    component BackendDatabaseCommunication {
        interface DatabaseCommunictionFacadeInterface <<interface>>
    }
    
    component BackendLogic {
        interface UserFacadeInterface <<interface>>
        interface ProductFacadeInterface <<interface>>
        interface SuggestionFacadeInterface <<interface>>
        interface OpinionFacadeInterface <<interface>>
        
        UserFacadeInterface       --> DatabaseCommunictionFacadeInterface
        ProductFacadeInterface    --> DatabaseCommunictionFacadeInterface
        SuggestionFacadeInterface --> DatabaseCommunictionFacadeInterface
        OpinionFacadeInterface    --> DatabaseCommunictionFacadeInterface
    }
}
@enduml 
```

## UserFacade

```plantuml
@startuml
component Backend {
    component BackendDatabaseCommunication {
        interface DatabaseCommunictionFacadeInterface <<interface>> {
            + getAllUsers() : User[]
            + getUserByToken(token: String) : User
            + createUser(firstName: String, lastName: String, email: String, passwordHash: String, profilePictureUrl: String, isAdmin: Boolean) : User
            + getUserToken(email: String, passwordHash: String) : String
            + addUserToken(userId: Integer, token: String) : String
            + updateUser(userId: Integer, firstName: String, lastName: String, email: String, passwordHash: String, profilePictureUrl: String, isAdmin: Boolean) : User
        }
    }
    component BackendLogic {
        note as Authors
            Projectants: 
             - Michał Andrzejczak
             - Mateusz Krasiński
        endnote
        
        UserFacadeInterface --> DatabaseCommunictionFacadeInterface
        
        interface UserFacadeInterface <<interface>> {
            + getAllUsers() : User[]
            + getUserByToken(token: String) : User
            + register(firstName: String, lastName: String, email: String, password: String, profilePictureUrl: String): User
            + registerAdmin(firstName: String, lastName: String, email: String, password: String, profilePictureUrl: String): User
            + login(email: String, password: String): String
            + getUserByToken(token: String): User
            + updateUser(userId: Integer, firstName: String, lastName: String, email: String, passwordHash: String, profilePictureUrl: String, isAdmin: Boolean) : User
        }
    
        note top of UserFacadeInterface
            Currently logged in user passed by consutructor. Null if current sessionis for guest.
            Only for admin user can, perform getAllUsers, registerAdmin and updateUser operations
        endnote
    
        note left of UserFacadeInterface::register
            Validate password length and complexity.
        endnote
    
        note left of UserFacadeInterface::registerAdmin
            Verify that admin user creates next admin.
        endnote
    
        note left of UserFacadeInterface::login
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
    component BackendDatabaseCommunication {
        interface DatabaseCommunictionFacadeInterface <<interface>> {
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
    
    component BackendLogic {
        note as Authors
            Projectants: 
             - Filip Grzelak
             - Damian Biskupski
        endnote
        
        ProductFacadeInterface  --> DatabaseCommunictionFacadeInterface
        interface ProductFacadeInterface <<interface>> {
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
    
        note top of ProductFacadeInterface
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
    component BackendDatabaseCommunication {
        interface DatabaseCommunictionFacadeInterface <<interface>> {
            + getAllSuggestions() : Suggestion[]
            + getUserSugestions(userId: Integer) : Sugestion[]
            + addSuggestion(productId: Integer, userId: Integer, suggestionDescription: String) : Suggestion
            + replySuggestion(suggestiontId:Integer, suggestionReviewerId: Integer, suggestionStatus: String, suggestionReply: String)
        }
    }
    
    component BackendLogic {
        note as Authors
            Projectants: 
             - Paweł Wieczorek
             - Julian Woroniecki
        endnote
        
        SuggestionFacadeInterface  --> DatabaseCommunictionFacadeInterface
        interface SuggestionFacadeInterface <<interface>> {
            + getUserSugestions() : Sugestion[]
            + addSuggestion(product: Product, suggestionDescription: String) : Suggestion
            + getAllSuggestions() : Suggestion[]
            + replySuggestion(suggestiontId:Integer, suggestionStatus: String, suggestionReply: String)
        }
    
        note top of SuggestionFacadeInterface
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
    component BackendDatabaseCommunication {
        interface DatabaseCommunictionFacadeInterface <<interface>> {
            + getProductOpinions(sku: String) : Opinion[]
            + addProductOpinion(opinionValue: Integer, opinionDescription: String, opinionPicture: String, advatages: String[], disadvantages: String[]) : Opinion
            + getUserOpinions(userId: Integer) : Opinion[]
        }
    }
    
    component BackendLogic {
        note as Authors
            Projectants: 
             - Paweł Wieczorek
             - Julian Woroniecki
        endnote
        
        OpinionFacadeInterface  --> DatabaseCommunictionFacadeInterface
        interface OpinionFacadeInterface <<interface>> {
            + getProductOpinions(product: Product) : Opinion[]
            + addProductOpinion(opinionValue: Integer, opinionDescription: String, opinionPicture: String, advatages: String[], disadvantages: String[]) : Opinion
            + getUserOpinions(user: User) : Opinion[]
        }
    
        note top of OpinionFacadeInterface
            Currently logged in user passed by consutructor. Null if current sessionis for guest.
            Verify that user passed by constructor is:
             - logged in not adminto perform addProductOpinion and getUserOpinions operation.
        endnote
    }
}
@enduml 
```