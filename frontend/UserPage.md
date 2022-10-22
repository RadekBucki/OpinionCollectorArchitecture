# User PAge

```plantuml
@startuml
    
component Frontend {
    component ViewAndLogic {
        component UserPage {
            note as AuthorsUP
                Projectants: 
                 - Ania Banachowicz
                 - Paweł Dera
            endnote
            component LoginPage {
            }
            component RegistrationPage {
            }
            component PLP {
            }
            component PDP {
            }
            component ClientPanel {
            }
        }
    }
    
    component FrontendBackendCommunication {
    }
}
ViewAndLogic                 -->  FrontendBackendCommunication

@enduml 
```