# Admin Panel

```plantuml
@startuml
    
component Frontend {
    component ViewAndLogic {
        component AdminPanel {
            note as AuthorsAP
                Projectants: 
                 - Rafał Komorowski
                 - Kamil Presler
            endnote
            component MainPage {
            }
            component ListPage {
            }
            component EditPage {
            }
        }
    }
    
    component FrontendBackendCommunication {
    }
}
ViewAndLogic                 -->  FrontendBackendCommunication

@enduml 
```