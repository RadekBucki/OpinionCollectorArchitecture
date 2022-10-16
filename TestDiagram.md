# Plant diagram
## Example UML Class Diagram
```plantuml
@startuml
class Class01 {
    - attibute1
    + attribute2
    + function1()
    + function2()
    - attribute3
    + function(3)
}
Class01 <|-- Class02
Class01 <- Class03
Class03 *-- Class04
Class03 <- Class05
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
