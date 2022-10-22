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
            + getAllUsers() : User[]
            + getUserByToken(String token) : User
            + createUser(String firstName, String lastName, String email, String passwordHash, String profilePictureUrl, Boolean isAdmin) : User
            + getUserToken(String email, String passwordHash) : String
            + addUserToken(Integer userId, String token) : String
            + updateUser(Integer userId, String firstName, String lastName, String email, String passwordHash, String profilePictureUrl, Boolean isAdmin) : User
        }
    
    component BackendLogic {
        note as Authors
            Projectants: 
             - Michał Andrzejczak
             - Mateusz Krasiński
        endnote
        
        UserFacadeInterface ..> DatabaseCommunictionFacadeInterface
        
        interface UserFacadeInterface <<interface>> {
            + getAllUsers() : User[]
            + getUserByToken(String token) : User
            + register(String firstName, String lastName, String email, String password, String profilePictureUrl): User
            + register(String firstName, String lastName, String email, String profilePictureUrl): User
            + registerAdmin(String firstName, String lastName, String email, String password, String profilePictureUrl): User
            + login(String email, String password): String
            + login(String email): String
            + getUserByToken(String token): User
            + updateUser(Integer userId, String firstName, String lastName, String email, String passwordHash, String profilePictureUrl, Boolean isAdmin) : User
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
            + getProductBySku(String sku) : Product
            + getAllProducts() : Product[]
            + getVisibleProducts() : Product[]
            + getProductsFilterProducts(String categoryName, String searchPhrase, Integer opinionAvgMin, Integer opinionAvgMax) : Product[]
            + createProduct(Integer authorId, String sku, String ean, String name, String pictureUrl, String description, String[] categoryNames, Boolean visible) : Product
            + updateProduct(Integer authorId, String sku, String ean, String name, String pictureUrl, String description, String[] categoryNames, Boolean visible) : Product
            + removeProduct(String sku)

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
            + getProductBySku(String sku) : Product
            + getAllProducts() : Product[]
            + getProducts() : Product[]
            + getProductsFiltered(String categoryName, String searchPhrase, Integer opinionAvgMin, Integer opinionAvgMax) : Product[]
            + addProduct(String sku, String name, String pictureUrl, String description, String[] categoryNames, Boolean visible) : Product
            + editProduct(String sku, String name, String pictureUrl, String description, String[] categoryNames, Boolean visible) : Product
            + removeProduct(String sku)
            
            + addCategory(String categoryName, Boolean visible) : Category
            + editCategory(String categoryName, Boolean visible) : Category
            + removeCategory(String categoryName)
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
            + getUserSugestions() : Sugestion[]
            + addSuggestion(Product product, String suggestionDescription) : Suggestion
            + getAllSuggestions() : Suggestion[]
            + replySuggestion(Integer suggestiontId, String suggestionStatus, String suggestionReply)
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
            + getProductOpinions(Product product) : Opinion[]
            + addProductOpinion(Integer opinionValue, String opinionDescription, String opinionPicture, String[] advatages, String[] disadvantages) : Opinion
            + getUserOpinions(User user) : Opinion[]
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