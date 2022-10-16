# Backend logic layer

```plantuml
@startuml
component backend {
    interface DatabaseCommunictionInterface
    
    interface CustomerFacadeInterface
    interface ProductFacadeInterface
    interface SugestionFacadeInterface
    interface OpinionFacadeInterface
    
    CustomerFacadeInterface  ..> DatabaseCommunictionInterface
    ProductFacadeInterface   ..> DatabaseCommunictionInterface
    SugestionFacadeInterface ..> DatabaseCommunictionInterface
    OpinionFacadeInterface   ..> DatabaseCommunictionInterface
}
@enduml 
```