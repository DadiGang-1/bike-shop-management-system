# Architecture summary
This Spring Boot application uses both MVC and REST controllers. 
Thymeleaf templates are used for the Admin and Shop dashboards, while REST APIs serve all other modules. 
The application interacts with two databases—MySQL (for client, specialists, shop, appointment, and admin data) and MongoDB (for reviews). 
All controllers route requests through a common service layer, which in turn delegates to the appropriate repositories. 
MySQL uses JPA entities for structured relational data while MongoDB uses document models for document-based records.

# Numbered flow of data and control
1. **User Interface Interaction:** Users interact with the application via server-rendered Thymeleaf web dashboards (AdminDashboard, SpecialistsDashboard) or REST API clients (Appointments, ClientDashboard, ClientRecord).
2. **Controller Routing:** Incoming HTTP requests are routed based on URL and method to either Thymeleaf Controllers (for web views) or REST Controllers (for API endpoints) to validate input and coordinate the flow.
3. **Business Logic Execution:** Controllers delegate processing to the Service Layer, which enforces business rules, runs validations, and coordinates workflows across entities.
4. **Repository Abstraction:** The service layer calls Spring Data JPA (for relational entities) or Spring Data MongoDB repositories to abstract data persistence and retrieval.
5. **Database Access:** Repositories query the underlying engines directly, accessing MySQL for structured relational data (users, roles, appointments) and MongoDB for flexible document structures (reviews).
6. **Model Binding:** Retrieved database records are mapped into Java objects, converting relational rows into @Entity JPA classes and BSON collections into @Document models.
7. **Response Rendering:** The resulting models (or DTOs) are returned to the controller to either populate dynamic Thymeleaf HTML templates or serialize into JSON HTTP responses for API clients.

```mermaid
flowchart TB
  subgraph Dashboard
    UIAdmin2[Admin Dashboard]
    UIManager2[Manager Dashboard]
  end
  subgraph Rest_Modules
    RESTApointment[Apointment]
    RESTCD[Client Dashboard]
    RESTCO[Orders DB]
  end
  subgraph Spring_Boot_Application
    subgraph MySQL_Models
        DB1[Clients]
        DB2[Orders]
        DB3[Specialists]
        DB4[Appointments]
        DB5[Shop_locations]
        DB6[Manager]
        DB7[Admin]
        DB8[Payments]
        DB9[Specialists_available_times]
        direction LR
    end
    subgraph MongoDB_Models
        DB01[Reviews]
        DB02[Logs]
        DB03[Messages]
        DB04[Specialist_advices]
        DB05[Confirmation_emails]
        direction LR
    end
    MySQL_Models1[MySQL_Models]
    MongoDB_Models1[MongoDB_Models]

    MySQL_Databases[MySQL_Databases]
    MongoDB_Databases[MongoDB_Databases]

    MySQL_Repositories[MySQL_Repositories]
    MongoDB_Repositories[MongoDB_Repositories]

    Service_Layer[Service_Layer]

    Thymeleaf_Controllers[Thymeleaf_Controllers]
    Rest_Controllers[Rest_Controllers]
  end
  
  MySQL_Models -.JPA_Entities.-> MySQL_Models1
  MongoDB_Models -.Documents.-> MongoDB_Models1

  MySQL_Databases --> MySQL_Models1
  MongoDB_Databases --> MongoDB_Models1

  MySQL_Repositories -.Accesses.-> MySQL_Databases
  MongoDB_Repositories -.Accesses.-> MongoDB_Databases

  Service_Layer -.Uses.-> MySQL_Repositories
  Service_Layer -.Uses.-> MongoDB_Repositories
  
  Thymeleaf_Controllers -.Calls.-> Service_Layer
  Rest_Controllers -.Calls.-> Service_Layer

  Dashboard -.Uses.-> Thymeleaf_Controllers
  Rest_Modules -.Uses JSON API.-> Rest_Controllers


```
