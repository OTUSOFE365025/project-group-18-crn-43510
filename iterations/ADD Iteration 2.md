# Step 2: Establish Iteration Goal by Selecting Drivers

The goal of this iteration is to define the major components that support the primary functionality. In this case we will be considering mainly the **primary use cases:**
- UC-1
- UC-2
- UC-6

# Step 3: Choose One or More Elements of the System to Decompose

The elements that will be refined for this iteration are the modules located in the different layers of the shown in the reference architecture.

# Step 4: Choose One or More Design Concepts That Satisfy the Selected Drivers

| Design Decisions and Locations | Rationale |
|-------------------------------|-----------|
| Implement Pipe and Filter architecture pattern (Server Side) | This design concept will function as the core of generating AI model respones. The system will processes data in stages to allow modular and maintainable business logic on the server side. Each stage handles a specific task (ex. tokenization, preprocessing, etc.). Also helps with scalability (UC-2, QA-4, QA-5, CRN-1). |
| Implement Client-Side Caching (Client Side) | Used in order to reduce server load and improve response times. (UC-2, QA-2, QA-3, CON-2). |
Implement Client-Side Preprocessing (Client Side) | Performs tasks like input normalization, lightweight tokenization, or filtering before sending data to the server. This will reduce server load, improve user-perceived performance, and provide faster response times. Can also be used to error check. (UC-1, QA-2, QA-3, CRN-1). |
| Use AIDAP Database for Session/Context Storage (Server Side) | Stores user session data and conversation context persistently, enabling multi-turn conversations and personalization, and recovery using backups. That way the system is consistent for each user across different sessions and devices (UC-2, QA-4, CON-2). |


# Step 5: Instantiate Architectural Elements, Allocate Responsibilities and Define Interfaces

| Design Decisions and Locations | Rationale |
|-------------------------------|-----------|
| Create Pipe and Filter Pattern Modules (Server Side) | Implements the pipe-and-filter architecture as modular server-side component. Essentially the new module handles all of the data processing (UC-2, QA-4, QA-5, CRN-1). |
| Use an Input Preprocessor module for Client Preprocessing (Client Side) | Handles the client pre-processing tasks (UC-1, QA-2, QA-3, CRN-1). |
| Use Adapter Pattern for External Integrations (Server Side) | Using the External Services module, create an adapter module to make sure to provide a uniform interface for integrating with university and external systems, simplifying maintenance and enabling support for multiple external APIs (UC-6, CON-3). |
| Use Strategy Pattern for Multi-Modal Inputs (Client Side) | Create a strategy module which focuses on dynamic handling of text and voice inputs across platforms, keeping the system flexible and extensible (UC-1).|

# Step 6: Sketch Views and Record Design Decisions

## Logical View: Specify modules that directly support the primary use cases

![UML Logical Diagram for modules that support the primary functionality](/iterations/iteration-artifacts/iteration2_logical.drawio.png)

## Logical View: Updated Elements & Reponsibilities Table (New Additions)

| Element | Responsibility |
|--------|----------------|
| Response Generation SS | Generates final AI/system responses from processed input. |
| Session Management SS | Tracks user sessions and conversation context. Persists session state to AIDAP DB and provides enriched context to business modules on the server side |
| Pipe and Filter Processing SS | Implements modular processing pipeline for AI input: stages include input validation, tokenization, intent/entity detection, business rules, and response preparation. Provides processed data to ResponseGeneration. |
| ExternalAdapter SS | Integrates with external university and third-party systems via adapters. Provides a uniform interface for external API calls and transforms external data for business modules. |
| Input Preprocessor CS| Handles client-side preprocessing: input normalization, lightweight tokenization, and initial validation. Sends processed input to the Service Layer for further business processing. |
| Processing Strategy CS | Dynamically selects input processing strategies (text, voice, or multi-modal). |
| Client Caching CS | Temporarily stores frequently used data locally (in-memory or browser storage). Reduces round-trips to the server by retrieving cached responses. |
| Client Interface CS | Handles client-side API calls to the Service Layer. Sends requests for AI responses or other server-side services, receives data, and passes it. |


## Primary Use Cases: Sequence Diagrams

**UC-1 & UC-2 Sequence Diagram: Use multiple conversation modes, Generate responses**

![Sequence Diagram 1 for UC-1, UC-2](/iterations/iteration-artifacts/iteration2_sequence1.drawio.png)

table

**UC-6 Sequence Diagram: Integrate with University Systems**

![Sequence Diagram 2 for UC-6](/iterations/iteration-artifacts/iteration2_sequence2.drawio.png)

table

# Step 7: Perform Analysis of Current Design and Review Iteration Goal and Achievement of Design Purpose

table