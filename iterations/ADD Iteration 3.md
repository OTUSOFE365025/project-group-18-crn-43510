Include in this file the 7 steps for Iteration 3
# Step 2: Establish Iteration Goal by Selecting Drivers
Quality Attribute Scenario Chosen: Availability
-QA-3

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