# Admin Panel

```plantuml
@startuml
    
component Frontend {
    component AdminPanel {
        note as AuthorsAP
            Projectants: 
             - Rafał Komorowski
             - Kamil Presler
        endnote
        note as Description
            Component responsible for:
            - Main admin panel page
            - Display list with edit button of:
              - Users
              - Product
              - Suggestions
              - Categories
            - Edit:
              - Users
              - Product
              - Categories
            - Reply suggestions
        endnote
    }
}

@enduml 
```

```plantuml
@startuml
    
component Frontend {
    component AdminPanel {
    }
    
    component BackendCommunication {
    }

    AdminPanel -(0- BackendCommunication
}

@enduml 
```