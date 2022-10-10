# Plant diagram
## Example UML Class Diagram
```plantuml
@startuml
Class01 <|-- Class02
Class03 *-- Class04
Class05 o-- Class06
@enduml
```
## Example Logic diagram
```plantuml
@startuml
Frontend -> "Backend" : Hello
"Backend" -> "Database" as Long
' You can also declare:
' "Backend" -> Long as "Database"
Long --> "Backend" : ok
@enduml
````