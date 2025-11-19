# Step 1: Review Inputs

<table>
  <tr>
    <th>Category</th>
    <th>Details</th>
  </tr>

  <tr>
    <td><b>Design Purpose</b></td>
    <td>
      This is a greenfield system in a mature domain. The purpose is to produce a sufficiently detailed design to support the construction of the system.
    </td>
  </tr>

  <tr>
    <td><b>Primary Functional Requirements</b></td>
    <td>
      From the use cases created during phase 1, the primary use cases are listed below:<br><br>
      <b>UC-1:</b> Core functionality that the platform can't work without<br>
      <b>UC-2:</b> Key function that is required in order for the platform to be considered operational<br>
      <b>UC-6:</b> Main purpose of the platform is to implement this function<br><br>
      These three use cases need to be implemented before anything else in order for the platform to work.
    </td>
  </tr>

  <tr>
    <td><b>Quality Attribute Scenarios</b></td>
    <td>
The quality attribute scenarios from phase 1 are listed below, each with its own ranking for importance and difficulty to implement.<br><br>

| Scenario ID | Importance to Customer | Difficulty of Implementation |
|-------------|------------------------|------------------------------|
| QA-1        | High                   | High                         |
| QA-2        | High                   | High                         |
| QA-3        | Medium                 | Medium                       |
| QA-4        | Medium                 | High                         |
| QA-5        | High                   | High                         |

From the table above, the selected drivers are <b>QA-1</b>, <b>QA-2</b>, <b>QA-3</b>, and <b>QA-5</b> because they directly support the primary functional requirements for this iteration.
    </td>
  </tr>

  <tr>
    <td><b>Constraints</b></td>
    <td>
      CRN-1, CRN-3, and CRN-4 are included since they are required for the architectural design. CRN-2 can be deferred to a later iteration.
    </td>
  </tr>

  <tr>
    <td><b>Concerns</b></td>
    <td>
      All of the concerns created from phase 1 are included since they all are required for the architectural design.
    </td>
  </tr>

</table>

# Step 2: Establish Iteration Goal by Selecting Drivers

Since this is the first iteration, **we want to establish the overall system structure**. In this case we must be mindful of all the inputs, but pay special attention to the following:

- QA-1: Security/privacy
- QA-2: Perfomance
- CON-1: Mandatory connectivity
- CON-3: Implement SSO
- CON-4: In sync with university systems data
- CRN-2: Monitoring system and logging failures
- CRN-3: Cloud nativity

**Here is the Context Diagram:**

![Context diagram for step 2 of ADD process iteration 1](/iteration-artifacts/iteration1_context.drawio.png)

# Step 3: Choose One or More Elements of the System to Decompose

Because we are developing a greenfield system, we will be refining the entire AI-Powered Digital Assistant Platform (AIDAP). Therefore, **the platform will be refined through decomposing the entire system**.

# Step 4: Choose One or More Design Concepts That Satisfy the Selected Drivers

| Design Decisions and Locations | Rationale |
|-------------------------------|-----------|
| Logically structure the system using the Rich Internet Application reference architecture (Client Side) | The Rich Internet Application (RIA) architecture supports the development of multi-platform web applications (CON-1, CRN-3) that are rich in features on the client side. Additionally, through the use of an API, it can be configured to run on other devices as well (UC-1) outside of a browser. When used with a browser on mobile/desktop devices, it aligns with making the system more performant and available (QA-2, QA-3) by reducing server round-trips and offloading some of the computation (such as any preprocessing, data validation, etc.) to the client. |
| Logically structure the server part of the system using the Service Application reference architecture (Server Side) | The Service Application architecture is necessary in order to facilitate multi-platform access and provide multiple different kinds of services (UC-1, UC-2). Separating the server logic can also make it easier to ensure the system is secure and private (QA-1) and make it simpler to integrate external systems (UC-6, CON-4). Separating server logic into services also allows the system to have clear modularization. |
| Physically structure the system using the four-tier deployment pattern | Using a four-tier deployment is common for web apps. For our system it is necessary to support multi-modal access and cleanly separate the system’s responsibilities across distinct layers. Introducing a dedicated business logic tier for using the AI model separate from the web server allows the system to handle different interactions without exposing internal services directly (UC-1, UC-2). This separation strengthens security and privacy (QA-1) and improves scalability (QA-5) and flexibility when connecting external systems (UC-6, CON-4). |
| Deploy the system as multi-platform web app using modern technologies: <br>**Frontend:** React Native framework for frontend <br>**Backend:** FastAPI framework for backend | React Native is suitable for the frontend because it can target web, mobile, and embedded interfaces to make multi-platform delivery simple. Its rendering model also supports the interactions required for the RIA architecture (UC-1, UC-2). FastAPI is chosen for the backend because it is a high-performance, asynchronous framework designed for building clean, well-structured services. It also automatically creates API documentation which will be helpful when integrating external services (UC-6, CON-4). |

# Step 5: Instantiate Architectural Elements, Allocate Responsibilities and Define Interfaces

| Design Decisions and Locations | Rationale |
|--------------------------------|-----------|
| Use a local database for storing interactions | The system needs to store user interactions, so a local database on the university network is necessary. University-related data can still be accessed through an external database when needed (UC-6). |
| Implementing an external services component | Create an External Services component, a dedicated module responsible for interacting with university systems and other third-party services beyond the core AI response functionality (UC-6, CON-3). |
| Keep operational management component | Use for the monitoring and logging to track system performance and failures, improving maintainability and helping optimize system performance (QA-4, CRN-2). |
|Add a security component | Checks for authentication and authorization to access system (CON-3) and controls data permissions and accessible features (different types of users can get access to more features).|


# Step 6: Sketch Views and Record Design Decisions

## Logical View: RIA + Service Application Architecture

![Reference architecture UML diagram for step 6 of ADD process iteration 1](/iteration-artifacts/iteration1_logical.drawio.png)

## Logical View: Elements Table

| Element / Module | Responsibility |
|-----------------|----------------|
| Presentation Layer CS | Render UI components and handle user interactions. |
| Business Processing Layer CS | Handle client-side logic and input preprocessing. |
| Data Access Layer CS | Manage requests to server-side APIs and local caching. |
| Cross Cutting Layer CS | Provide shared client utilities like logging, input validation, and configuration. |
| Service Layer SS | Expose backend services via APIs and manage requests from clients. |
| Business Layer SS | Implement core AI logic and business rules. |
| Data Layer SS | Manage database access and data persistence. |
| Cross Cutting Layer SS | Provide shared server utilities like logging, security, monitoring, and configuration. |
| UI Module CS | Individual UI components for the RIA frontend. |
| UI Processing Module CS | Handle client-side event processing and interaction logic. |
| Business Logic Module CS | Perform local computations or preprocessing before sending requests to server. |
| Business Entities Module CS | Represent client-side models and data structures. |
| Data Access Module CS | Interface with server APIs and manage local data caching. |
| Service Consumer Module CS | Manage communication with external or internal backend services. |
| Cache Storage DB CS | Temporarily store frequently used data to improve client performance. |
| Operational Management Module CS/SS | Monitor system performance, log events, and track errors across client and server. |
| Security Module CS/SS | Handle authentication, authorization, and data access control on both sides. |
| Communication Module CS/SS | Manage messaging, API requests, and event transmission between client and server. |
| Service Interfaces Module SS | Define endpoints and service contracts for clients and other systems. |
| Message Type Module SS | Standardize request and response formats for communication between modules. |
| Business Logic Module SS | Implement server-side AI processing, decision-making, and business rules. |
| Business Entities Module SS | Represent server-side data models and domain objects. |
| DB Access Module SS | Manage database queries, transactions, and persistence. |
| External Services/API Access Module SS | Handle integration with university systems and third-party services. |
| AIDAP DB SS | Store platform data including AI interaction logs, user data, and system configurations. |
| University Systems DB SS | Access and interact with university-managed databases and services. |

## Deployment View: Four-Tier Deployment Structure

![Deployment UML diagram for step 6 of ADD process iteration 1](/iteration-artifacts/iteration1_deployment.drawio.png)

## Deployment View: Elements Table

| Element / Tier | Responsibility |
|----------------|----------------|
| Client Tier: Rich UI Engine | Render user interface, handle user input, perform local validation, and display AI responses. |
| Web Tier: FastAPI / Service Layer | Receive and validate client requests, manage sessions, and route requests to the Business Logic Tier. |
| Business Logic Tier: AI & Business Modules | Complete AI processing, implement business rules, control workflows, and integrate with external services.|
| DB Server: AIDAP DB, University Systems DB, Cache | Store data and logs, provide fast and reliable data access to Business Logic Tier. |

## Deployment View: Relationships Table

| Relationship | Description |
|--------------|-------------|
| Client Tier → Web Tier | Client sends user requests (input, queries) to the Web Tier and receives processed responses. |
| Web Tier → Business Logic Tier | Web Tier requests the Business Logic Tier and receives results to return to the client. |
| Business Logic Tier → DB Server | Business Logic Tier reads from and writes to database. Also retrieves external information as needed using external services component. |

# Step 7: Perform Analysis of Current Design and Review Iteration Goal and Achievement of Design Purpose

**Progress Table:**

| Not Addressed | Partially Addressed | Completely Addressed | Design Decisions Made During Iteration |
|---------------|---------------------|-----------------------|----------------------------------------|
|               | UC-1                |                       | Used RIA architecture + React framework to support multi-platform client. |
|               | UC-2                |                       | Expected to be implemented in the Service Application architecture’s business layer. |
|               |                     | UC-6                  | Added External Services component and University Systems DB. |
|               | QA-1                |                       | Security component uses SSO. |
|               | QA-2                |                       | RIA reduces server load. |
|               |                     | QA-3                  | Cache storage is included in the architecture. |
|           |          QA-4           |                       | Operational Management module improves maintainability and modifiability. |
|          |        QA-5              |                       | Four-tier deployment supports scalability. |
|               |                     | CON-1                 | RIA supports required connectivity. |
| CON-2         |                     |                       | Not addressed. |
|               |                     | CON-3                 | Security component implements SSO. |
|               | CON-4               |                       | External Services component defines how to access university systems. |
| CRN-1         |                     |                       | Not addressed. |
|               | CRN-2               |                       | Operational Management module provides logs for issue detection. |
|               | CRN-3               |                       | RIA is cloud-native. |
|               | CRN-4               |                       | Deployment diagram shows how requests are handled. |
