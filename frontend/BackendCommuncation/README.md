# Frontend - Backend Communication
## Description
<!--
```plantuml
@startuml
    
component Frontend {    
    component BackendCommunication {
        note as Authors
            Projectants: 
             - Tomasz Roske
             - Antoni Jończyk
        endnote
        note as Description
            Description: 
            Create HTTP queries, send it to backend application,
            process answer.
            
            List of queries:
            - GET /users - get all users data
            - POST /users/register - register new user or admin user
            - POST /users/login - get user data and token
            - PUT /users/update/{id} - update user data
            - GET /products/{page} - get products
            - GET /products/all/{page} - get all products
            - POST /products/search - search products
            - GET /products/details - get products details
            - POST /products/add - add product
            - PUT /products/edit - edit product
            - DELETE /products/delete - remove product
            - POST /categories/add - add category
            - PUT /categories/edit - edit category
            - DELETE /categories/delete - remove category
            - GET /categories - get categories
            - GET /categories/all - get categories all
            - GET /suggestions/user - get user suggestions
            - POST /suggestions/add - add suggestion
            - GET /suggestions/get - get all suggestions
            - PUT /suggestions/reply - reply to suggestion
            - GET /opinions/user - get user opinions
            - GET /opinions/product - get product opinions
            - POST /opinions/add - add opinion
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
    component AdminPanel {
    }
    
    component UserPage {
    }
    
    component BackendCommunication as BackendCommunicationComponent {
    }
    
    interface BackendCommunication <<interface>>  {
        + getCategories(): Category[]
        + getAllCategories(): Category[]
        + getProductOpinions(sku: String): Opinion[]
        + getUserOpinions(): Opinion[]
        + getProducts(page: Integer): Page
        + getAllProducts(page: Integer): Page
        + getProductDetails(sku: String): Product
        + getSearchProduct(searchInput: ProductSearch)
        + getAllSuggestions(): Suggestion[]
        + getUserSuggestions(): Suggestion[]
        + getAllUsers(): User[]
        + addCategory(categoryName: String, isVisible: Boolean): Category
        + addOpinion(opinion: Opinion): Opinion
        + addProduct(product: Product): Product
        + addSuggestion(description: String, sku: String): Suggestion
        + userLogin(mail: String, password: String): Token
        + userRegister(user: User): User
        + deleteCategory(categoryName: String): Category
        + deleteCategory(categoryName: String): Category
        + deleteProduct(sku: String): Product
        + editCategory(categoryName: String, isVisible: Boolean): Category
        + editProduct(product: Product): Product
        + replySuggestion(suggestionReply: SuggestionReply): Suggestion
        + userEdit(user: User): User
        + isTokenAvailable(): Boolean
        + userLogout()
    }
}


AdminPanel            ..>   BackendCommunication
UserPage              ..>   BackendCommunication
BackendCommunication  <|... BackendCommunicationComponent

@enduml 
```
-->
![](media/api.png)
## Class diagrams
### GET
<!--
```plantuml
component Frontend {
    circle BackendComunication
    component BackendCommunication {
        enum Get {
            CATEGORIES
            CATEGORIES_ALL
            OPINIONS_PRODUCT
            OPINIONS_USER
            PRODUCTS
            PRODUCTS_ALL
            PRODUCTS_DETAILS
            PRODUCTS_SEARCH
            SUGGESTIONS_ALL
            SUGGESTIONS_USER
            USERS_ALL
        }
        
        class Category {
            + categoryName: String
            + visible: Boolean
        }
        
        class Opinion {
            + advantages: String[]
            + description: String
            + disadvantages: String[]
            + firstName: String
            + opinionValue: Integer
            + opinionAvg: Integer
            + pictureUrl: String
            + sku: String
        }
        
        class ProductGet {
            + categories: Category[]
            + description: String
            + name: String
            + pictureUrl: String
            + sku: String
            + opinionAvg: Integer
            + visible:Boolean
        }
        
        class Page {
            + actualPage: Integer
            + numberOfPages: Integer
            + products: ProductGet[]
        }
        
        class Suggestion {
            + description: String
            + product: ProductGet
            + review: Review
            + reviewer: Reviewer
            + suggestionId: Integer
            + user: User
            + sku: String
        }
        
        class User {
            + email: String
            + firstName: String
            + id: Integer
            + isAdmin: Boolean
            + lastName: String
            + pictureUrl:String
        }
        
        class Review {
            + reply: String
            + status: String
        }
        
        class Reviewer {
            + firstname: String
            + lastname: String
            + profilePictureUrl: String
        }
        
        Suggestion o- Reviewer
        Suggestion o- Review
        Suggestion o- User
        Suggestion o- ProductGet
        
        ProductGet o- Category
        
        Page o- ProductGet
        
        class GetRequest {
            + getCategories(): Category[]
            + getAllCategories(): Category[]
            + getProductOpinions(skuval: String): Opinion[]
            + getProducts(page: Integer): Page
            + getAllProducts(page: Integer): Page
            + getProductDetails(skuval: String): ProductGet
            + getSearchProduct(searchInput: ProductSearch): ProductGet[]
            + getAllSuggestions(): Suggestion[]
            + getUserSuggestions() : Suggestion[]
            + getAllUsers(): User[]
        }
        Get <.. GetRequest
        GetRequest ..> Category
        GetRequest ..> Opinion
        GetRequest ..> Page
        GetRequest ..> ProductGet
        GetRequest ..> Suggestion
        GetRequest ..> User
    }
    BackendComunication - BackendCommunication
}
component Backend {
    component UserLogic {
        class UserController
    }
    component ProductLogic {
        class CategoryController
        class ProductController
    }
    component OpinionLogic {
        class OpinionController
    }
    component SuggestionLogic {
        class SuggestionController
    }
}
        
GetRequest -(0- CategoryController   : Categories
GetRequest -(0- ProductController    : Products
GetRequest -(0- OpinionController    : Opinions
GetRequest -(0- SuggestionController : Suggestions
GetRequest -(0- UserController       : Users
```
-->
![](media/get.png)
### POST
<!--
```plantuml
component Frontend {
    circle BackendComunication
    component BackendCommunication {
        enum Post {
            CATEGORIES_ADD
            OPINIONS_ADD
            PRODUCTS_ADD
            SUGGESTIONS_ADD
            USER_LOGIN
            USER_REGISTER
        }
        
        class Category {
            + categoryName: String
            + visible: Boolean
        }
        
        class Opinion {
            + advantages: String[]
            + description: String
            + disadvantages: String[]
            + firstName: String
            + opinionValue: Integer
            + opinionAvg: Integer
            + pictureUrl: String
            + sku: String
        }
        
        class ProductGet {
            + categories: Category[]
            + description: String
            + name: String
            + pictureUrl: String
            + sku: String
            + opinionAvg: Integer
            + visible:Boolean
        }
        
        class ProductSend {
            + categoryNames: String
            + description: String
            + name: String
            + pictureUrl: String
            + sku: String
            + visible:Boolean
        }
        
        class Page {
            + actualPage: Integer
            + numberOfPages: Integer
            + products: ProductGet[]
        }
        
        class Suggestion {
            + description: String
            + product: ProductGet
            + review: Review
            + reviewer: Reviewer
            + suggestionId: Integer
            + user: User
            + sku: String
        }
        
        class UserLogin {
            + email: String
            + password: String
        }
        
        class User {
            + email: String
            + firstName: String
            + id: Integer
            + isAdmin: Boolean
            + lastName: String
            + pictureUrl:String
        }
        
        class UserEdit {
            + email: String
            + firstName: String
            + isAdmin: Boolean
            + lastName: String
            + password: String
            + pictureUrl:String
        }
        
        class Token {
            + token: String
            + type: String
            + user: User
        }
        
        class Review {
            + reply: String
            + status: String
        }
        
        class Reviewer {
            + firstname: String
            + lastname: String
            + profilePictureUrl: String
        }
        
        Suggestion o- Reviewer
        Suggestion o- Review
        Suggestion o- User
        Suggestion o- ProductGet
        
        ProductGet o- Category
        
        Page o- ProductGet
        
        class PostRequest {
            + addCategory(categoryName: String, isVisible: Boolean): Category
            + addOpinion(opinion: Opinion): Opinion
            + addProduct(product: ProductSend): ProductGet
            + addSuggestion(desc: String, skuval: String): Suggestion
            + userLogin(mail: String, pass: String): Token
            + userRegister(user: UserEdit): User
        }
        Post <.. PostRequest
        PostRequest ..> Category
        PostRequest ..> Opinion
        PostRequest ..> ProductGet
        PostRequest ..> ProductSend
        PostRequest ..> Suggestion
        PostRequest ..> Token
        PostRequest ..> User
        PostRequest ..> UserEdit
        PostRequest ..> UserLogin     
    }
    BackendComunication - BackendCommunication
}
component Backend {
    component UserLogic {
        class UserController
    }
    component ProductLogic {
        class CategoryController
        class ProductController
    }
    component OpinionLogic {
        class OpinionController
    }
    component SuggestionLogic {
        class SuggestionController
    }
}
        
PostRequest -(0- CategoryController   : Categories
PostRequest -(0- ProductController    : Products
PostRequest -(0- OpinionController    : Opinions
PostRequest -(0- SuggestionController : Suggestions
PostRequest -(0- UserController       : Users
```
-->
![](media/post.png)
### PUT
<!--
```plantuml
component Frontend {
    circle BackendComunication
    component BackendCommunication {        
        enum Put {
            CATEGORIES_EDIT
            PRODUCTS_EDIT
            SUGGESTIONS_REPLY
            USER_EDIT
        }
        
        class Category {
            + categoryName: String
            + visible: Boolean
        }
        
        class ProductGet {
            + categories: Category[]
            + description: String
            + name: String
            + pictureUrl: String
            + sku: String
            + opinionAvg: Integer
            + visible:Boolean
        }
        
        class ProductSend {
            + categoryNames: String
            + description: String
            + name: String
            + pictureUrl: String
            + sku: String
            + visible:Boolean
        }
        
        class Suggestion {
            + description: String
            + product: ProductGet
            + review: Review
            + reviewer: Reviewer
            + suggestionId: Integer
            + user: User
            + sku: String
        }
        
        class User {
            + email: String
            + firstName: String
            + id: Integer
            + isAdmin: Boolean
            + lastName: String
            + pictureUrl:String
        }
        
        class Review {
            + reply: String
            + status: String
        }
        
        class Reviewer {
            + firstname: String
            + lastname: String
            + profilePictureUrl: String
        }
        
        Suggestion o- Reviewer
        Suggestion o- Review
        Suggestion o- User
        Suggestion o- ProductGet
        
        ProductGet o- Category
        
        class PutRequest {
            + editCategory(categoryName: String, isVisible: Boolean): Category
            + editProduct(product: ProductSend): ProductGet
            + replySuggestion(sugReply: SuggestionReply): Suggestion
            + userEdit(userId: Integer, user: UserEdit): User
        }
        Put <..PutRequest
        PutRequest ..> Category
        PutRequest ..> ProductGet
        PutRequest ..> ProductSend
        PutRequest ..> Suggestion
        PutRequest ..> User   
    }
    BackendComunication - BackendCommunication
}
component Backend {
    component UserLogic {
        class UserController
    }
    component ProductLogic {
        class CategoryController
        class ProductController
    }
    component SuggestionLogic {
        class SuggestionController
    }
}
        
PutRequest --(0-- CategoryController   : Categories
PutRequest --(0-- ProductController    : Products
PutRequest --(0-- SuggestionController : Suggestions
PutRequest --(0-- UserController       : Users
```
-->
![](media/put.png)
### DELETE
<!--
```plantuml
component Frontend {
    circle BackendComunication
    component BackendCommunication {
        
        enum Delete {
            CATEGORIES_DELETE
            PRODUCTS_DELETE
        }
        
        class Category {
            + categoryName: String
            + visible: Boolean
        }
        
        class ProductGet {
            + categories: Category[]
            + description: String
            + name: String
            + pictureUrl: String
            + sku: String
            + opinionAvg: Integer
            + visible:Boolean
        }
        
        class DeleteRequest {
            + deleteCategory(categoryName: String): Category
            + deleteProduct(skuval: String): Product
        }
        Delete <.. DeleteRequest
        DeleteRequest ..> Category
        DeleteRequest ..> ProductGet
    }
    BackendComunication - BackendCommunication
}
component Backend {
    component ProductLogic {
        class CategoryController
        class ProductController
    }
}
        
DeleteRequest -(0-- CategoryController   : Categories
DeleteRequest -(0-- ProductController    : Products
```
-->
![](media/delete.png)
### MethodRequest
<!--
```plantuml
component Frontend {
    circle BackendComunication
    component BackendCommunication {
        class User {
            + email: String
            + firstName: String
            + id: Integer
            + isAdmin: Boolean
            + lastName: String
            + pictureUrl:String
        }
        
        class MethodRequest {
            + getUser(): User
            + isTokenAvailable(): Boolean
            + userLogout()
        }
        MethodRequest ..> User      
    }
    BackendComunication - BackendCommunication
}
```
-->
![](media/methodRequest.png)