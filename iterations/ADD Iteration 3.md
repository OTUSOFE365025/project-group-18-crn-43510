Include in this file the 7 steps for Iteration 3
# Step 2: Establish Iteration Goal by Selecting Drivers
Quality Attribute Scenario Chosen: Availability
-QA-3
-QA-5
-QA-4

# Step 3:Elements to Refine
- Application Server (Django backend)

- Database Server (PostgreSQL / MySQL depending on your repo)

- Message Processing Components
(e.g., seat reservations, event creation, transaction queue)

# Step 4: 
| Design Decisions and Locations | Rationale |
|-------------------------------|-----------|
| Introduce Active Redundancy for backend servers | Allows immediate failover with no downtime. |
| Introduce Message Queue (RabbitMQ / Redis Queue) | Ensures transactions (purchases, holds) aren’t lost during failover. Guarantees ordering.|
| Load Balancing Cluster (Nginx / HAProxy) | Distributes user requests across backend replicas. |
| Database Replication (Primary–Replica) | Ensures availability & reduces read overload. |

# Step 5:
| Element | Responsibility |
|-------------------------------|-----------|
| LoadBalancer | Distribute incoming requests; detect failed backends; provide a stable public endpoint |
| Backend Server Replicas | Serve API logic; stateless to support autoscaling & failover |
| Message queue | persist and sequence reservation and checkout operations to create smooth peak writes |
| Queue Worker Service | Process queue events to aensure atomic seat locking |
| Primary database | Make sure that the primary database can store all orders, seat locks and payments |
| Authentification service | This is to validate tokens by handling RBAC checks making the software secure |

# Step 6:
                        +----------------------+
                        |     Load Balancer    |
                        | (HAProxy / Nginx)    |
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
      | (Redis/RabbitMQ Node) |----------------------->|  (Write Operations)   |
      +------------------------+                       +-----------+-----------+
                                                                   |
                                                       +-----------+-----------+
                                                       |   Database Replica   |
                                                       |  (Read Operations)   |
                                                       +----------------------+


# step 7
| Driver | Adressed | Design decisions that support it |
|-------------------------------|-----------|-----------------------------------|
| QA-3 | Partially Addressed | Backend replication, load balancer, message queue, DB replica |
| QA-4 Performance | Partially Addressed | Read-replica DB, traffic distribution, async queue processing |
| QA-5 Scalability | Partially Addressed | Stateless servers, autoscaling capability, load balancer |
| QA-6 Security | Partially Addressed | RBAC, security middleware, audit logging |