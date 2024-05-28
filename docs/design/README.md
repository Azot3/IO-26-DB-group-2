
#Бізнес-об'єктивна модель

This project involves the development of:
- Business Object Model
- Entity-Relationship (ER) Diagram
- Relational Schema

## Business Object Model

```plantuml
@startuml

entity Customer {
    * id
    * name
    * email
    * password
}

entity AccessLevel {
    * id
    * role
    * permissions
    * description
}

entity Assignment {
    * id
    * title
    * dueDate
}

entity Inquiry {
    * id
    * title
    * timestamp
    * details
}

entity Record {
    * id
    * title
    * timestamp
}

entity AssistanceRequest {
    * id
    * resolved
    * category
}

entity UserMessage {
    * id
    * content
    * timestamp
}

entity AdminReply {
    * id
    * content
    * timestamp
}

entity DataExport {
    * id
    * status
    * timestamp
}

Customer "1" - "0..*" AccessLevel: has
Customer "1" - "0..*" Assignment: works_on
Customer "1" - "0..*" Inquiry: raises
Customer "1" - "0..*" AssistanceRequest: submits

Inquiry "0..*" - "1" Record: recorded_in
AssistanceRequest "0..*" - "1" Record: related_to
AssistanceRequest "1" - "0..*" UserMessage: contains
AssistanceRequest "1" - "0..*" AdminReply: addressed_by
Record "0..*" - "1" DataExport: exported_to

@enduml
```

###ER-діаграма

```plantuml
@startuml
namespace Records {  
    entity DataExport <<ENTITY>> { 
        + id: int
        --
        status: bool
        timestamp: datetime
    }
   
    entity Record <<ENTITY>> {
        + id: int
        --
        title: string
        timestamp: datetime
    }
}

namespace Support {   
    entity AssistanceRequest <<ENTITY>> {
        + id: int
        --
        resolved: bool
        category: string
    }
    
    entity AdminReply <<ENTITY>> {
        + id: int
        --
        content: string
        timestamp: datetime
    }
    
    entity UserMessage <<ENTITY>> { 
        + id: int
        --
        content: string
        timestamp: datetime
    }
}

namespace Tasks {
    entity "Assignment" as assignment <<ENTITY>> {
        + id: int
        --
        title: string
        dueDate: datetime
    }
}

namespace UserManagement {        
    entity "Customer" as customer <<ENTITY>> {
        + id: int
        --
        name: string
        email: string
        password: string
    }
        
    entity "AccessLevel" as accessLevel <<ENTITY>> {
        + id: int
        --
        role: string
        permissions: string
        description: string
    }

    entity "Inquiry" as inquiry <<ENTITY>> {
        + id: int
        --
        title: string
        timestamp: datetime
        details: string
    }
}

customer "0..*" -- "1" accessLevel
customer "0..*" -- "1" assignment
customer "0..*" -- "1" inquiry
customer "1" -- "0..*" AssistanceRequest
inquiry "0..*" -- "1" Record
AssistanceRequest "1" -- "0..*" UserMessage
AssistanceRequest "1" -- "0..*" AdminReply
Record "1" -- "0..*" DataExport

@enduml
```

####Реляційна схема роботи 

<img src="./lab4_scheme.jpg">
