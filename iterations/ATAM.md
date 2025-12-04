# 1. Introduction

This document presents an ATAM (Architecture Tradeoff Analysis Method) evaluation of the Event Ticket Management System. The goal is to analyze how well the system's architecture supports critical quality attributes: Availability, Performance, Scalability, and Security.
This assessment identifies architectural risks, non-risks, tradeoff points, and sensitivity points.

# 2. Business Drivers
Primary business goals:
-Support high-demand ticket sales without downtime.

-Prevent lost revenue, oversold seats, or failed transactions.

-Protect customer and administrative data from unauthorized access.

-Enable the system to scale as event volume and traffic increase.

-Maintain a system architecture that is extendable and maintainable.

Key stakeholders:
-Customers (ticket buyers)

-Event organizers / admins

-Developers

-Operations / DevOps

# 3. Architecture Overview
Logical Architecture

Presentation Layer
-Web front-end interface

-Interacts with back-end via REST APIs

Application Layer
-Stateless Django-based API servers

-Business logic: seat locking, order workflow, payment logic

Data Layer
-Relational database (PostgreSQL/MySQL)

-Stores events, users, seats, and orders

Deployment Architecture (After ADD Iteration 3)
                        +----------------------+
                        |     Load Balancer    |
                        |   (Nginx / HAProxy)  |
                        +----------+-----------+
                                   |
         -----------------------------------------------------
         |                                                   |
 +--------------------------+                  +--------------------------+
 |   Backend Server A       |   <replicated>   |   Backend Server B       |
 |  (Django API, Stateless) |                  |  (Django API, Stateless) |
 +-----------+--------------+                  +-----------+--------------+
             |                                                |
             |                                                |
      +------+----------------+                       +-------+----------------+
      |    Message Queue      |     <replicated>       |      Database Primary  |
      |  (Redis/RabbitMQ)     |----------------------->|   (Write Operations)   |
      +------------------------+                       +-----------+-----------+
                                                                   |
                                                       +-----------+-----------+
                                                       |   Database Replica   |
                                                       |   (Read Operations)  |
                                                       +----------------------+

Key Architectural Mechanisms
-Stateless REST APIs

-Load balancing across replicas

-Message queue for ticket purchases & reservations

-Primary–replica database setup

-RBAC + authentication + audit logging

 # 4. Architectural Approaches Identified

-Three-tier layered architecture

-Load-balanced, replicated application servers

-Active redundancy and failover mechanisms

-Message queue for asynchronous purchase processing

-Database primary–replica separation

-Security middleware + RBAC + logging

# 5. Quality Attribute Utility Tree
-Availability

-Performance

-Scalability

-Security

Detailed Scenarios
| ID | Quality Attribute |	Scenario	| Importance	| Difficulty |
| QA-3 |	Availability |	Backend fails during purchase and system recovers in <30 seconds with no lost transactions | High |	High |
| QA-4 | Performance	| 500 users concurrently purchase/search; 95% of requests return in <2 seconds| High | High |
| QA-5 | Scalability | During event release, system load increases 3×; system scales horizontally with no downtime | High | Medium |
| QA-6 | Security | Unauthorized attempt to modify ticket/event data is blocked and logged 100% of the time	| Medium–High |	Medium |

# 6. Analysis of Architectural Approaches
Availability (QA-3)
Supporting Architecture:
-Load balancing

-Multiple backend replicas

-Message queue for durability

-Database primary-to-replica failover

Considerations:

-Queue guarantees in-flight transactions are not lost

-Need automated or rapid DB failover to meet <30 second goal

Performance (QA-4)

Supporting Architecture:

-Read replica database for high-frequency reads

-Load balancing reduces CPU pressure

-Queue reduces synchronous processing load

Considerations:

-DB indexes and optimized queries critical

-Workers must scale if purchase rate increases

Scalability (QA-5)

Supporting Architecture:

-Stateless backend servers → easy horizontal scaling

-Load balancer supports autoscaling

-Queue decouples request handling from DB load

Considerations:

-Queue throughput must match peak expected workload

-Database primary remains a potential bottleneck

Security (QA-6)

Supporting Architecture:

-RBAC middleware

-Authentication (session/JWT)

-Audit logging service

Considerations:

-Must ensure consistent use of middleware across all admin endpoints

-Logging and monitoring needed to detect suspicious behavior

# 7. Scenario Analysis
Availability Scenario Analysis (QA-3)

Scenario: Backend fails during purchase; system must recover in <30 seconds.

Path:
User → Load Balancer → Backend → Message Queue → Worker → DB Primary

Risks / Sensitivities:

-Reliability of queue persistence

-Time needed for DB failover

-In-progress transactions during backend crash

Performance Scenario Analysis (QA-4)

Scenario: 500 concurrent users purchasing tickets.

Paths:

-Read-heavy operations → DB Replica

-Write operations → Queue → DB Primary

Risks / Sensitivities:

-DB read/write performance under heavy load

-Worker concurrency

-Query optimization / indexing

Scalability Scenario Analysis (QA-5)

Scenario: Event release causes traffic to triple.

Risks / Sensitivities:

-Time to spin up new backend instances

-Queue throughput and latency

-DB primary write saturation

Security Scenario Analysis (QA-6)

Scenario: Unauthorized user attempts to change ticket price.

Path:
User → Auth Middleware → RBAC → Business Logic → Audit Log

Risks / Sensitivities:

-Inconsistent RBAC enforcement

-Incorrect permission mappings

-Missing audit logs

# 8. Risks, Non-Risks, Sensitivities, and Tradeoffs
Identified Risks

R1: DB failover may exceed 30-second requirement.

R2: Message queue could be a single bottleneck.

R3: Backend crashes may interrupt in-progress transactions.

R4: RBAC implementation errors could lead to unauthorized changes.

Non-Risks

NR1: Horizontal scaling of stateless backend servers is well supported.

NR2: Layered architecture promotes maintainability and extension.

Sensitivity Points

Database indexing and tuning

Queue durability configuration

Load balancer health check intervals

Authorization middleware placement

Tradeoff Points

Performance vs Consistency: Replica reads may be slightly stale.

Availability vs Complexity: Replication and failover add complexity to deployment.

Scalability vs Cost: More replicas = higher operational cost.

# 9. Summary of ATAM Findings

The proposed architecture provides strong support for the system’s main quality attributes:

High availability via redundancy and load balancing

Performance improvements via read replicas and async processing

Scalability through stateless backend replication and queueing

Strong security model with RBAC and audit logging

Remaining concerns include database failover strategy, queue scalability, and consistent RBAC enforcement.