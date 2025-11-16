# Builder Design Pattern - UML Diagrams

## 1. Builder Pattern - Class Diagram

```mermaid
classDiagram
    class BuilderDemo {
        +main(args: String[])$
    }
    
    class House {
        -String foundation
        -String structure
        -String roof
        -boolean swimmingPool
        ~House(builder: HouseBuilder)
        +toString(): String
    }
    
    class HouseBuilder {
        -String foundation
        -String structure
        -String roof
        -boolean swimmingPool
        +HouseBuilder()
        +setFoundation(foundation: String): HouseBuilder
        +setStructure(structure: String): HouseBuilder
        +setRoof(roof: String): HouseBuilder
        +setSwimmingPool(swimmingPool: boolean): HouseBuilder
        +getFoundation(): String
        +getStructure(): String
        +getRoof(): String
        +hasSwimmingPool(): boolean
        +build(): House
    }
    
    BuilderDemo ..> HouseBuilder : uses
    BuilderDemo ..> House : creates
    HouseBuilder ..> House : builds
    House o-- HouseBuilder : constructed by
```

---

## 2. Sequence Diagram - Building a House

```mermaid
sequenceDiagram
    participant Client as BuilderDemo
    participant Builder as HouseBuilder
    participant Product as House
    
    Client->>Builder: new HouseBuilder()
    activate Builder
    
    Client->>Builder: setFoundation("Concrete")
    Builder-->>Client: return this
    
    Client->>Builder: setStructure("Wood")
    Builder-->>Client: return this
    
    Client->>Builder: setRoof("Tiles")
    Builder-->>Client: return this
    
    Client->>Builder: setSwimmingPool(true)
    Builder-->>Client: return this
    
    Client->>Builder: build()
    Builder->>Product: new House(this)
    activate Product
    Product-->>Builder: House instance
    deactivate Product
    Builder-->>Client: House instance
    deactivate Builder
    
    Client->>Product: toString()
    Product-->>Client: "House [foundation=Concrete, ...]"
```

---

## 3. Builder Pattern Flow

```mermaid
graph TB
    subgraph "Builder Pattern Construction Flow"
        A[Client<br/>BuilderDemo] -->|1. Creates| B[Builder<br/>HouseBuilder]
        B -->|2. setFoundation| B
        B -->|3. setStructure| B
        B -->|4. setRoof| B
        B -->|5. setSwimmingPool| B
        B -->|6. build| C[Product<br/>House]
        C -->|7. Returns| A
    end
    
    style A fill:#e1f5ff
    style B fill:#fff4e1
    style C fill:#e8f5e9
```

---

## 4. Method Chaining Visualization

```mermaid
flowchart LR
    A[new HouseBuilder] --> B[setFoundation]
    B --> C[setStructure]
    C --> D[setRoof]
    D --> E[setSwimmingPool]
    E --> F[build]
    F --> G[House Object]
    
    B -.->|returns this| C
    C -.->|returns this| D
    D -.->|returns this| E
    E -.->|returns this| F
    
    style A fill:#4CAF50,color:#fff
    style G fill:#2196F3,color:#fff
```

---

## 5. Object Creation Comparison

```mermaid
graph TB
    subgraph "Without Builder Pattern - Telescoping Constructor"
        W1[Constructor with all parameters]
        W2["new House(foundation, structure, roof, swimmingPool)"]
        W3[Hard to read<br/>Easy to make mistakes<br/>Cannot skip optional params]
        W1 --> W2 --> W3
    end
    
    subgraph "With Builder Pattern - Fluent Interface"
        B1[Builder with fluent methods]
        B2["new HouseBuilder()<br/>.setFoundation('Concrete')<br/>.setStructure('Wood')<br/>.build()"]
        B3[Easy to read<br/>Can skip optional params<br/>Clear and maintainable]
        B1 --> B2 --> B3
    end
    
    style W3 fill:#f44336,color:#fff
    style B3 fill:#4CAF50,color:#fff
```

---

## 6. Design Explanation

### What is the Builder Pattern?

**Builder Pattern** is a creational design pattern that separates the construction of a complex object from its representation, allowing the same construction process to create different representations.

### Key Components

1. **Product (House)**: 
   - The complex object being built
   - Contains the fields: foundation, structure, roof, swimmingPool
   - Has a package-private constructor that only the Builder can access

2. **Builder (HouseBuilder)**:
   - Provides methods to construct parts of the Product
   - Each setter method returns `this` for method chaining
   - Contains the same fields as the Product
   - Has a `build()` method that creates the final Product

3. **Director/Client (BuilderDemo)**:
   - Uses the Builder to construct the object
   - Calls builder methods in desired order
   - Calls `build()` to get the final object

---

## 7. How Your Code Works

### Step-by-Step Process

```mermaid
stateDiagram-v2
    [*] --> CreateBuilder: new HouseBuilder()
    CreateBuilder --> SetFoundation: setFoundation("Concrete")
    SetFoundation --> SetStructure: setStructure("Wood")
    SetStructure --> SetRoof: setRoof("Tiles")
    SetRoof --> SetSwimmingPool: setSwimmingPool(true)
    SetSwimmingPool --> Build: build()
    Build --> HouseCreated: new House(builder)
    HouseCreated --> [*]
    
    note right of CreateBuilder
        Builder instance created
        All fields are null/false
    end note
    
    note right of SetFoundation
        Each setter returns 'this'
        Enables method chaining
    end note
    
    note right of Build
        Calls House constructor
        Passes builder to House
        House extracts values
    end note
```

---

## 8. Advantages & Disadvantages

### ✅ Advantages

```mermaid
mindmap
    root((Builder Pattern<br/>Advantages))
        Readability
            Fluent interface
            Self-documenting code
            Clear intent
        Flexibility
            Optional parameters
            Any order of calls
            Skip unwanted features
        Immutability
            Object built once
            Cannot modify after creation
            Thread-safe
        Maintenance
            Easy to add new parameters
            No constructor explosion
            Centralized construction logic
```

### ❌ Disadvantages

| Disadvantage | Description |
|-------------|-------------|
| **More Code** | Need to create separate Builder class |
| **Complexity** | Overkill for simple objects |
| **Memory** | Two objects created (Builder + Product) |
| **Learning Curve** | Developers need to understand the pattern |

---

## 9. When to Use Builder Pattern

```mermaid
flowchart TD
    Start{Designing<br/>a class?}
    
    Start --> Q1{Has many<br/>parameters?}
    Q1 -->|No <br/>< 3 params| Simple[Use Simple<br/>Constructor]
    Q1 -->|Yes <br/>> 4 params| Q2{Many optional<br/>parameters?}
    
    Q2 -->|No| Overload[Use Constructor<br/>Overloading]
    Q2 -->|Yes| Q3{Need immutable<br/>object?}
    
    Q3 -->|No| Setters[Use Setters]
    Q3 -->|Yes| Builder[✅ Use Builder<br/>Pattern]
    
    style Builder fill:#4CAF50,color:#fff,stroke:#2E7D32,stroke-width:3px
    style Simple fill:#2196F3,color:#fff
    style Start fill:#FF9800,color:#fff
```

---

## 10. Real-World Examples

### Common Use Cases

```mermaid
mindmap
    root((Builder Pattern<br/>Use Cases))
        Java APIs
            StringBuilder
            StringBuffer
            Calendar.Builder
            HttpRequest.Builder
        Database
            SQL Query Builders
            Hibernate Criteria
            JOOQ Query Builder
        Configuration
            Spring Boot Config
            Properties Builder
            JSON/XML Builders
        UI Frameworks
            Android AlertDialog
            JavaFX Builders
            HTML Builders
        Testing
            Test Data Builders
            Mock Object Builders
            Fixture Builders
```

---

## 11. Your Implementation Analysis

### Current Code Structure

**House.java** (Product):
```java
- Private fields (foundation, structure, roof, swimmingPool)
- Package-private constructor (accepts HouseBuilder)
- toString() method for display
```

**HouseBuilder.java** (Builder):
```java
- Same fields as House
- Public setter methods returning 'this'
- Getter methods for field access
- build() method creating House instance
```

**BuilderDemo.java** (Client):
```java
- Creates HouseBuilder instances
- Chains setter methods
- Calls build() to get House
- Demonstrates 5 different configurations
```

### Improvement Suggestions

```mermaid
graph LR
    subgraph "Current Implementation"
        C1[Separate Builder Class]
        C2[Package-private Constructor]
        C3[Getter methods in Builder]
    end
    
    subgraph "Recommended Improvements"
        R1[Make Builder static nested class]
        R2[Make House fields final]
        R3[Remove getters, use direct access]
        R4[Add validation in build method]
        R5[Add required parameters to Builder constructor]
    end
    
    C1 -.-> R1
    C2 -.-> R2
    C3 -.-> R3
    
    style R1 fill:#4CAF50,color:#fff
    style R2 fill:#4CAF50,color:#fff
    style R3 fill:#4CAF50,color:#fff
    style R4 fill:#4CAF50,color:#fff
    style R5 fill:#4CAF50,color:#fff
```

---

## 12. Summary

### Builder Pattern Overview

| Aspect | Description |
|--------|-------------|
| **Pattern Type** | Creational |
| **Purpose** | Construct complex objects step by step |
| **Problem Solved** | Telescoping constructor problem |
| **Key Benefit** | Readable, flexible object creation |
| **Trade-off** | More code for better maintainability |
| **Best For** | Classes with 4+ parameters, many optional fields |

### Your Implementation

✅ **Working correctly** - Demonstrates all Builder pattern principles  
✅ **Method chaining** - Fluent interface implemented  
✅ **Separation of concerns** - Builder and Product are separate  
✅ **Flexible construction** - Can skip optional parameters  

---
