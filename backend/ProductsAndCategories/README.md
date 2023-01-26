# Backend - ProductLogic
## Description
<!--
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
    component ProductLogic {
    }
}

interface Products <<interface>> {
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
    + getCategories() : Category[]
    + getAllCategories(() : Category[]
}

BackendCommunication ..>   Products
Products             <|... ProductLogic

@enduml 
```
-->
![](media/api.png)
## Class diagrams
<!--
```plantuml
circle Products
component Backend {
    component ProductLogic {
        class ProductController {
            + getProductDetails(sku: String) : Product
            + getAllProducts(page: Integer) : Product[]
            + getProducts(page: Integer) : Product[]
            + searchProducts(categoryName: String, searchPhrase: String, opinionAvgMin: Integer, opinionAvgMax: Integer) : Product[]
            + addProduct(sku: String, name: String, pictureUrl: String, description: String, categoryNames: String[], visible: Boolean) : Product
            + editProduct(sku: String, name: String, pictureUrl: String, description: String, categoryNames: String[], visible: Boolean) : Product
            + removeProduct(sku: String)
        }
        class CategoryController {
            + addCategory(categoryName: String, visible: Boolean) : Category
            + editCategory(categoryName: String, visible: Boolean) : Category
            + removeCategory(categoryName: String)
            + getCategories() : Category[]
            + getAllCategories(() : Category[]
        }
        circle ProductFacade
        class ProductFacadeImpl {
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
            + getCategories() : Category[]
            + getAllCategories(() : Category[]
        }
        ProductFacade -- ProductFacadeImpl
        CategoryController "1" o-- "1" ProductFacadeImpl
        ProductController  "1" o-- "1" ProductFacadeImpl
        Products -- ProductController
        Products -- CategoryController
        
        class Mapper {
            +map (object: Object) : Object
        }
        CategoryController ..> Mapper
        ProductController  ..> Mapper
    }
    component DatabaseCommunication {
        class DatabaseCommunictionFacadeImplementation
        class Product
        class Category
    }
    ProductFacadeImpl -(0- DatabaseCommunictionFacadeImplementation : DatabaseCommunication
    ProductFacadeImpl ..>  Product
    ProductFacadeImpl ..>  Category
    
    component UserLogic {
        class UserFacadeImpl {
            + getUserByToken(token: String) : User
        }
    }
    ProductController  -(0-- UserFacadeImpl  : UserAuth
    CategoryController -(0-- UserFacadeImpl : UserAuth
}
```
-->
![](media/class.png)