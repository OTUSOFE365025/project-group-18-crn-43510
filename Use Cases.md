**Use Case Diagram:**

![UML Use Case Diagram](/UML%20Use%20Case%20Diagram.png)

**Use Case Table:**

| ID    | Use Case                             | Description                                                                                                                                       | Associated Requirement ID                                           |
|-------|-------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------|
| UC-1  | Use Multiple Conversational Modes    | User can choose a preferred language and communicate with the AI model through either text or voice using natural language; both modes available on mobile, web, and voice-assistant devices | R1, R4, R5, R7, RS4, RS6, RS9                                     |
| UC-2  | Generate Responses                   | User can generate responses based on internal knowledge, external university system data, previous user interactions and live data provided by the user in a timely manner. | R2, R3, R5, R6, RS5, RS10, RS13, RD1, RD3                        |
| UC-3  | Publish/Update University-Related Material | Teachers and Administration should be able to publish and update university-related material and announcements available to the students through the digital assistant. | RL1, RL2, RL4, RL5, RL6, RL8, RA3                                 |
| UC-4  | Broadcast System Alerts              | System should be able to send alerts to all kinds of users depending on the type of user and notification settings.                               | RS2, RS6, RL2, RL7, RA3, RA4                                       |
| UC-5  | Update and Configure System          | Allow System Maintainers and Administration to deploy or update AI models. Also allow administration to set global policies (data retention, response logging). | R3, R5, RA1, RA2, RM1, RM3, RM5                                    |
| UC-6  | Integrate with University Systems   | Integrate with existing data sources from other University systems. If it fails, it reattempts after a configured period of time. Should also maintain data integrity. | R1, R3, RA6, RD1, RD3, RD4                                         |
