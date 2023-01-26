# Backend - Database Communication
## Description
<!--
```plantuml
@startuml
component Backend {
    component DatabaseCommunication {
        note as Authors
            Projectants: 
             - Jakub Mielczarek
             - Jakub Czyżewski
        endnote
        note as Description
            Logic responsible for
            communication with database.
            It allows to:
            - get data
            - update data
            - insert data
            - delete data
            Takes data and remove objects.
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
    
    interface DatabaseCommuniction <<interface>> {
        + getAllUsers() : User[]
        + createUser(firstName: String, lastName: String, email: String, passwordHash: String, profilePictureUrl: String, isAdmin: Boolean) : User
        + updateUser(userId: Integer, firstName: String, lastName: String, email: String, passwordHash: String, profilePictureUrl: String, isAdmin: Boolean) : User
    
        + getProductBySku(sku: String) : Product
        + getAllProducts() : Product[]
        + getVisibleProducts() : Product[]
        + getProductsFilterProducts(categoryName: String, searchPhrase: String, opinionAvgMin: Integer, opinionAvgMax: Integer) : Product[]
        + createProduct(authorId: Integer, sku: String, name: String, pictureUrl: String, description: String, categoryNames: String[], visible: Boolean) : Product
        + updateProduct(authorId: Integer, sku: String, name: String, pictureUrl: String, description: String, categoryNames: String[], visible: Boolean) : Product
        + removeProduct(sku: String)

        + getAllCategories() : Category[]
        + getCategoryByname(name: String) : Category[]
        + createCategory(categoryName: String, visible: Boolean) : Category
        + updateCategory(categoryName: String, visible: Boolean): Category
        + removeCategory(categoryName: String)
        
        + getProductOpinions(sku: String) : Opinion[]
        + addProductOpinion(opinionValue: Integer, userId: Integer, sku: String, opinionDescription: String, opinionPicture: String, advatages: String[], disadvantages: String[]) : Opinion
        + getUserOpinions(userId: Integer) : Opinion[]
        
        + getAllSuggestions() : Suggestion[]
        + getUserSugestions(userId: Integer) : Sugestion[]
        + addSuggestion(sku: String, userId: Integer, suggestionDescription: String) : Suggestion
        + replySuggestion(suggestiontId:Integer, suggestionReviewerId: Integer, suggestionStatus: String, suggestionReply: String)
    }
    note left of DatabaseCommuniction::createUser
        Returns Spring Repository
        item type.
    endnote
    
    note left of DatabaseCommuniction::getAllProducts
        Only this method returns 
        unvisible products
        (for admin).
    endnote
    
    note left of DatabaseCommuniction::getProductsFilterProducts
        Working on like or between 
        statement for non null passed 
        fields.
    endnote
    
    UserLogic                   ..>  DatabaseCommuniction
    ProductLogic                ..>  DatabaseCommuniction
    OpinionLogic                ..>  DatabaseCommuniction
    SuggestionLogic             ..>  DatabaseCommuniction
    DatabaseCommuniction        <|..  DatabaseCommunication
}

@enduml 
```
-->
![](media/api.png)
## Class diagrams
### Relation between data classes
<!--
```plantuml
component Backend {
    component DatabaseCommuniction {
        class Suggestion {
        }
        class User {
            - userId: Integer
            - firstName: String
            - email: String
            - passwordHash: String
            - profilePictureUrl: String
            - admin: Boolean
        }
        class Opinion {
            - opinionId: Integer
            - userId: Integer
            - opinionValue: Integer
            - opinionDescription: String
            - opinionPitureUrl: String
            - advantages: String[]
            - disadvantages: String[]
        }
        class Product {
            - productId: Integer
            - authorId: Integer
            - sku: String
            - pictureUrl: String
            - description: String
            - categoryName: String
        }
        class Category {
            - categoryId: Integer
            - categoryName: String
            - visible: Boolean
        }
        class Suggestion {
            - suggestionId: Integer
            - productId: Integer
            - reviewerId: Integer
            - userId: Integer
            - description: String
        }
        class Review {
            - reviewerId: Integer
            - suggestionId: Integer
            - reply: string
            - status: String
        }
        Product    "1" o-- "0..*" Opinion    : has
        Product    "1" o-- "0..*" Suggestion : has
        Product    "1" *-- "0..*" User       : has autor of
        Product    "1" --- "0..*" Category   : has
        Suggestion "1" o-- "0..*" Review    : has
        User       "1" o-- "0..*" Opinion    : gives
        User       "1" --- "0..*" Suggestion : reviews
        User       "1" o-- "0..*" Suggestion : suggest
        
        UserDatabaseCommuniation        ..> User
        ProductDatabaseCommuniation     ..> Product
        SuggestionDatabaseCommuniation  ..> Suggestion
        OpinionDatabaseCommuniation     ..> Opinion
        CategoryDatabaseCommuniation    ..> Category
        
        DatabaseCommunictionFacadeImplementation ..> User
        DatabaseCommunictionFacadeImplementation ..> Product
        DatabaseCommunictionFacadeImplementation ..> Suggestion
        DatabaseCommunictionFacadeImplementation ..> Opinion
        DatabaseCommunictionFacadeImplementation ..> Category
    }
}
```
-->
![](media/dataClasses.png)

### Relation between facade and classes which are using it
<!--
```plantuml
@startuml
top to bottom direction
component Backend {
    circle DatabaseCommunictionFacade
    component DatabaseCommunication {
        class DatabaseCommunictionFacadeImplementation {
            + getAllUsers() : User[]
            + createUser(firstName: String, lastName: String, email: String, passwordHash: String, profilePictureUrl: String, isAdmin: Boolean) : User
            + updateUser(userId: Integer, firstName: String, lastName: String, email: String, passwordHash: String, profilePictureUrl: String, isAdmin: Boolean) : User
        
            + getProductBySku(sku: String) : Product
            + getAllProducts() : Product[]
            + getVisibleProducts() : Product[]
            + getProductsFilterProducts(categoryName: String, searchPhrase: String, opinionAvgMin: Integer, opinionAvgMax: Integer) : Product[]
            + createProduct(authorId: Integer, sku: String, name: String, pictureUrl: String, description: String, categoryNames: String[], visible: Boolean) : Product
            + updateProduct(authorId: Integer, sku: String, name: String, pictureUrl: String, description: String, categoryNames: String[], visible: Boolean) : Product
            + removeProduct(sku: String)
        
            + getAllCategories() : Category[]
            + getCategoryByname(name: String) : Category[]
            + createCategory(categoryName: String, visible: Boolean) : Category
            + updateCategory(categoryName: String, visible: Boolean): Category
            + removeCategory(categoryName: String)
            
            + getProductOpinions(sku: String) : Opinion[]
            + addProductOpinion(opinionValue: Integer, userId: Integer, sku: String, opinionDescription: String, opinionPicture: String, advatages: String[], disadvantages: String[]) : Opinion
            + getUserOpinions(userId: Integer) : Opinion[]
            
            + getAllSuggestions() : Suggestion[]
            + getUserSugestions(userId: Integer) : Sugestion[]
            + addSuggestion(sku: String, userId: Integer, suggestionDescription: String) : Suggestion
            + replySuggestion(suggestiontId:Integer, suggestionReviewerId: Integer, suggestionStatus: String, suggestionReply: String)
        }
        
        DatabaseCommunictionFacade -- DatabaseCommunictionFacadeImplementation
        
        class UserDatabaseCommuniation {
            + getAllUsers() : User[]
            + createUser(firstName: String, lastName: String, email: String, passwordHash: String, profilePictureUrl: String, isAdmin: Boolean) : User
            + updateUser(userId: Integer, firstName: String, lastName: String, email: String, passwordHash: String, profilePictureUrl: String, isAdmin: Boolean) : User
        }
        class ProductDatabaseCommuniation {
            + getProductBySku(sku: String) : Product
            + getAllProducts() : Product[]
            + getVisibleProducts() : Product[]
            + getProductsFilterProducts(categoryName: String, searchPhrase: String, opinionAvgMin: Integer, opinionAvgMax: Integer) : Product[]
            + createProduct(authorId: Integer, sku: String, name: String, pictureUrl: String, description: String, categoryNames: String[], visible: Boolean) : Product
            + updateProduct(authorId: Integer, sku: String, name: String, pictureUrl: String, description: String, categoryNames: String[], visible: Boolean) : Product
            + removeProduct(sku: String)
        }
        class SuggestionDatabaseCommuniation {
            + getAllSuggestions() : Suggestion[]
            + getUserSugestions(userId: Integer) : Sugestion[]
            + addSuggestion(sku: String, userId: Integer, suggestionDescription: String) : Suggestion
            + replySuggestion(suggestiontId:Integer, suggestionReviewerId: Integer, suggestionStatus: String, suggestionReply: String)
        }
        class OpinionDatabaseCommuniation {
            + getProductOpinions(sku: String) : Opinion[]
            + addProductOpinion(opinionValue: Integer, userId: Integer, sku: String, opinionDescription: String, opinionPicture: String, advatages: String[], disadvantages: String[]) : Opinion
            + getUserOpinions(userId: Integer) : Opinion[]
        }
        class CategoryDatabaseCommuniation {
            + getAllCategories() : Category[]
            + getCategoryByname(name: String) : Category[]
            + createCategory(categoryName: String, visible: Boolean) : Category
            + updateCategory(categoryName: String, visible: Boolean): Category
            + removeCategory(categoryName: String)
        }
        DatabaseCommunictionFacadeImplementation o--- UserDatabaseCommuniation
        DatabaseCommunictionFacadeImplementation o--- ProductDatabaseCommuniation
        DatabaseCommunictionFacadeImplementation o-- SuggestionDatabaseCommuniation 
        DatabaseCommunictionFacadeImplementation o-- OpinionDatabaseCommuniation
        DatabaseCommunictionFacadeImplementation o-- CategoryDatabaseCommuniation
    }
}

component Database {
}
UserDatabaseCommuniation        -(0- Database : SQL
ProductDatabaseCommuniation     -(0- Database : SQL
SuggestionDatabaseCommuniation  -(0- Database : SQL
OpinionDatabaseCommuniation     -(0- Database : SQL
CategoryDatabaseCommuniation    -(0- Database : SQL
@enduml 
```
-->
![](media/dataClasses.png)