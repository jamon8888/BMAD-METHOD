# Architecture Patterns Reference

Common architecture patterns with pros, cons, and when to use them.

## Monolithic Architecture

**Description**: All application components in a single, unified codebase deployed as one unit.

**Pros**:
- Simple to develop and test initially
- Easy to deploy (single artifact)
- No network latency between components
- Straightforward debugging
- Lower operational complexity

**Cons**:
- Scalability limitations (scale entire app, not individual components)
- Deployment risk (all or nothing)
- Technology lock-in (one tech stack)
- Team coupling (coordination required for releases)
- Large codebase becomes hard to maintain

**When to Use**:
- Early stage startups (pre-product-market fit)
- Small teams (< 10 engineers)
- Simple applications with limited complexity
- Rapid prototyping and iteration

**When to Avoid**:
- Large teams (> 50 engineers)
- High-scale requirements
- Need for independent deployment
- Multiple team autonomy requirements

---

## Microservices Architecture

**Description**: Application broken into small, independently deployable services communicating over network.

**Pros**:
- Independent scalability per service
- Technology diversity (different services, different stacks)
- Team autonomy (own services end-to-end)
- Fault isolation (one service failure doesn't take down all)
- Independent deployment and release cycles

**Cons**:
- High operational complexity
- Network latency and reliability issues
- Distributed system challenges (transactions, consistency)
- Testing complexity
- Significant infrastructure overhead
- Requires mature DevOps practices

**When to Use**:
- Large engineering teams (> 50 engineers)
- Different scaling needs per component
- Need for team autonomy
- Different technology requirements per domain
- Mature DevOps culture

**When to Avoid**:
- Small teams (< 20 engineers)
- Simple applications
- Limited operational expertise
- Early stage / unproven product

---

## Modular Monolith

**Description**: Single deployable unit with well-defined module boundaries and clear interfaces.

**Pros**:
- Simplicity of monolith deployment
- Clear boundaries for future extraction
- Lower operational complexity than microservices
- Can scale team with good modularity
- Easier testing than microservices

**Cons**:
- Still deploy entire application
- Scaling limitations (but better than pure monolith)
- Requires discipline to maintain boundaries
- Technology still somewhat locked-in

**When to Use**:
- Growing teams (15-50 engineers)
- Transitioning from monolith to microservices
- Want modularity without operational complexity
- Clear domain boundaries exist

**Best Practice**: Start here, extract to microservices only when needed.

---

## Event-Driven Architecture

**Description**: Services communicate through asynchronous events/messages rather than synchronous calls.

**Pros**:
- Loose coupling between services
- High scalability
- Resilience (consumers can fail independently)
- Event replay and audit trail
- Easy to add new consumers

**Cons**:
- Eventual consistency challenges
- Debugging complexity
- Event schema evolution
- Need for message broker infrastructure
- Harder to reason about system behavior

**When to Use**:
- High-volume asynchronous processing
- Need for loose coupling
- Event sourcing requirements
- Real-time data streaming
- Audit trail requirements

**Tools**: Kafka, RabbitMQ, AWS SNS/SQS, Google Pub/Sub

---

## Serverless Architecture

**Description**: Application logic runs in stateless compute containers managed by cloud provider.

**Pros**:
- Zero server management
- Auto-scaling out of the box
- Pay per execution (cost efficient at low scale)
- Fast deployment
- Built-in high availability

**Cons**:
- Cold start latency
- Vendor lock-in
- Local testing challenges
- Limited execution duration
- Debugging complexity
- Can be expensive at high scale

**When to Use**:
- Event-driven workloads
- Irregular/unpredictable traffic
- Small teams with limited ops expertise
- APIs and backend functions
- Data processing pipelines

**When to Avoid**:
- Long-running processes
- Predictable high-volume traffic
- Latency-sensitive applications
- Need for portability

**Providers**: AWS Lambda, Google Cloud Functions, Azure Functions

---

## Layered Architecture (N-Tier)

**Description**: Application organized into logical layers (presentation, business logic, data access).

**Pros**:
- Clear separation of concerns
- Easy to understand
- Standard pattern (well-known)
- Layer-level testing

**Cons**:
- Can become tightly coupled
- Changes ripple across layers
- May not map well to modern deployment

**Layers**:
1. Presentation Layer (UI)
2. Application Layer (Business Logic)
3. Domain Layer (Business Rules)
4. Data Access Layer (Database)

---

## Hexagonal Architecture (Ports and Adapters)

**Description**: Core business logic isolated from external concerns through ports and adapters.

**Pros**:
- Business logic independent of frameworks
- Easy to swap implementations
- Highly testable
- Clear boundaries

**Cons**:
- More upfront design
- Additional abstraction layers
- Overkill for simple applications

**When to Use**:
- Complex business rules
- Need for multiple adapters (web, CLI, API)
- Long-term maintainability priority

---

## CQRS (Command Query Responsibility Segregation)

**Description**: Separate read and write models for data.

**Pros**:
- Optimized reads and writes independently
- Scalability (scale reads differently than writes)
- Complex query support
- Audit trail built-in

**Cons**:
- Increased complexity
- Eventual consistency
- More infrastructure
- Harder to implement correctly

**When to Use**:
- Read/write load imbalance
- Complex reporting requirements
- Event sourcing
- High-scale applications

---

## Decision Framework

### Start Here: Modular Monolith
- Simplicity of deployment
- Modularity for growth
- Can extract later if needed

### Transition to Microservices When:
- Team > 50 engineers
- Clear service boundaries
- Different scaling needs
- Mature DevOps practices

### Consider Event-Driven When:
- Asynchronous processing
- High volume events
- Need for loose coupling
- Real-time requirements

### Consider Serverless When:
- Small team
- Irregular traffic
- Event-driven workloads
- Want zero ops

---

**Key Principle**: Choose the simplest architecture that meets your needs. You can always add complexity later, but removing complexity is nearly impossible.
