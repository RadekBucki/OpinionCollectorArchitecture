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
        
        UserFacadeInterface       ..> DatabaseCommunictionFacadeInterface
        ProductFacadeInterface    ..> DatabaseCommunictionFacadeInterface
        SuggestionFacadeInterface ..> DatabaseCommunictionFacadeInterface
        OpinionFacadeInterface    ..> DatabaseCommunictionFacadeInterface
    }
}
@enduml 
```

## UserFacade

```plantuml
@startuml
component Backend {
    component BackendDatabaseCommunication {
        interface DatabaseCommunictionFacadeInterface <<interface>>{
            + generateUserToken(String email, String passwordHash) : String
            + getUserByToken(String token) : User
            + getUserById(String id) : User
            + createUser(String firstName, String lastName, String email, String passwordHash, String profilePictureUrl, Boolean isAdmin) : User
            + updateUser(Integer userId, String firstName, String lastName, String email, String passwordHash, String profilePictureUrl, Boolean isAdmin) : User
        }
    }
    
    component BackendLogic {
        note as Authors
            Projectants: 
             - Michał Andrzejczak
             - Mateusz Krasiński
        endnote
        
        UserFacadeInterface ..> DatabaseCommunictionFacadeInterface
        
        interface UserFacadeInterface <<interface>> {
            - user : User
            + getAllUsers() : User[]
            + register(String firstName, String lastName, String email, String password, String profilePictureUrl): User
            + registerAdmin(User userCreatedNewAdmin, String firstName, String lastName, String email, String password, String profilePictureUrl): User
            + login(String email, String password): String
            + getUserByToken(String token): User
            + updateUser(Integer userId, String firstName, String lastName, String email, String passwordHash, String profilePictureUrl, Boolean isAdmin) : User
            + addUserToken(Integer userId, String token)
        }
    
        note top of UserFacadeInterface
            Only for admin user ca perform getAllUsers, registerAdmin and updateUser operations
        endnote
    
        note left of UserFacadeInterface::register
            Validate password length and complexity.
        endnote
    
        note left of UserFacadeInterface::registerAdmin
            Verify that admin user creates next admin.
        endnote
    
        note left of UserFacadeInterface::login
            Returns user token.
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
            + getProductBySku(String sku) : Product
            + getAllProducts() : Product[]
            + searchProducts(String searchPhrase) : Product[]
            + getProductsFilterProducts(String categoryName, Integer opinionAvgMin, Integer opinionAvgMax) : Product[]
            + createProduct(Integer authorId, String sku, String ean, String name, String pictureUrl, String description, String[] categoryNames, Boolean visible) : Product
            + updateProduct(Integer authorId, String sku, String ean, String name, String pictureUrl, String description, String[] categoryNames, Boolean visible) : Product
            + removeProduct(String sku)
            
            + getCategories(): Category[]
            + createCategory(String categoryName, Boolean visible) : Category
            + updateCategory(String categoryName, Boolean visible): Category
            + removeCategory(String categoryName)
        }
    }
    
    component BackendLogic {
        note as Authors
            Projectants: 
             - Filip Grzelak
             - Damian Biskupski
        endnote
        
        ProductFacadeInterface  ..> DatabaseCommunictionFacadeInterface
        interface ProductFacadeInterface <<interface>> {
            - user: User
            + getAllProducts()
            + getProducts()
            + getProductsFiltered(String categoryName, Integer opinionAvgMin, Integer opinionAvgMax)
            + addProduct(String sku, String ean, String name, String pictureUrl, String description, String[] categoryNames, Boolean visible) : Product
            + editProduct(String sku, String ean, String name, String pictureUrl, String description, String[] categoryNames, Boolean visible) : Product
            + removeProduct(String sku)
            
            + addCategory(String categoryName, Boolean visible)
            + editCategory(String categoryName, Boolean visible)
            + removeCategory(String categoryName)
        }
    
        note left of ProductFacadeInterface
            Verify that user
            passed by constructor
            is admin and can perform
            add and edit operations.
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
            + getUserSugestions(Integer userId) : Sugestion[]
            + addSuggestion(Integer productId, Integer userId, String suggestionDescription) : Suggestion
            + replySuggestion(Integer suggestiontId, Integer suggestionReviewerId, String suggestionStatus, String suggestionReply)
        }
    }
    
    component BackendLogic {
        note as Authors
            Projectants: 
             - Paweł Wieczorek
             - Julian Woroniecki
        endnote
        
        SuggestionFacadeInterface  ..> DatabaseCommunictionFacadeInterface
        interface SuggestionFacadeInterface <<interface>> {
            - user: User
            + getUserSugestions() : Sugestion[]
            + addSuggestion(Product product, String suggestionDescription) : Suggestion
            + getAllSuggestions() : Suggestion[]
            + replySuggestion(Integer suggestiontId, String suggestionStatus, String suggestionReply)
        }
    
        note left of SuggestionFacadeInterface
            Verify that user passed
            by constructor is:
             - admin to perform 
            getAllSuggestions and
            replySuggestion operations
             - logged in not admin
             to perform getUserSugestions
             and addSuggestion.
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
            + getProductOpinions(String sku) : Opinion[]
            + addProductOpinion(Integer opinionValue, String opinionDescription, String opinionPicture, String[] advatages, String[] disadvantages) : Opinion
            + getUserOpinions(Integer userId) : Opinion[]
        }
    }
    
    component BackendLogic {
        note as Authors
            Projectants: 
             - Paweł Wieczorek
             - Julian Woroniecki
        endnote
        
        OpinionFacadeInterface  ..> DatabaseCommunictionFacadeInterface
        interface OpinionFacadeInterface <<interface>> {
            - user: User
            + getProductOpinions(Product product) : Opinion[]
            + addProductOpinion(Integer opinionValue, String opinionDescription, String opinionPicture, String[] advatages, String[] disadvantages) : Opinion
            + getUserOpinions(User user) : Opinion[]
        }
    
        note left of OpinionFacadeInterface
            Verify that user passed
            by constructor is:
             - logged in not admin
             to perform addProductOpinion
             and getUserOpinions operation.
        endnote
    }
}
@enduml 
```