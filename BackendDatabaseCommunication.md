# Backend - Database Communication

```plantuml
@startuml

Database --> DatabaseCommunictionInterface

database Database {
}

interface DatabaseCommunictionInterface {
    + generateUserToken(String email, String passwordHash) : User
    + getUserByToken(String token) : User
    + getUserById(String id) : User
    + createUser(String firstName, String lastName, String email, String passwordHash, String profilePictureUrl, Boolean isAdmin) : User
    
    + getProductBySku(String sku) : Product
    + getAllProducts() : Product[]
    + searchProducts(String searchPhrase) : Product[]
    + getProductsFilterProducts(String categoryName, Integer opinionAvgMin) : Product[]
    + createProduct(Integer authorId, String sku, String ean, String name, String pictureUrl, String description, String, Boolean visible) : Product
    + updateProduct(Integer authorId, String sku, String ean, String name, String pictureUrl, String description, String, Boolean visible) : Product
    + removeProduct(String sku)
    
    + getProductOpinions(String sku) : Opinion[]
    + getProductOpinionAvg(String sku) : OpinionAvg[]
    + addProductOpinion(Integer opinionValue, String opinionDescription, String opinionPicture, String[] advatages, String[] disadvantages)
    
    + getProductSugestions(String sku) : Sugestion[]
    + addSuggestion(Integer productId, Integer userId, String suggestionDescription): Suggestion
    + replySuggestion(Integer suggestionReviewerId, String suggestionStatus, String suggestionReply)
}

note left of DatabaseCommunictionInterface::createUser
    Returns Spring Repository
    item type.
endnote

note left of DatabaseCommunictionInterface::getProductsFilterProducts
    Working on like or between statement
    for non null passed fields.
endnote

@enduml 
```