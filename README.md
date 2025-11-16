# Builder and Decorator Design Patterns - UML Diagrams

## 1. Builder Pattern

### UML Class Diagram

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

### Sequence Diagram - Building a House

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

### Pattern Structure

```mermaid
graph TB
    subgraph "Builder Pattern Components"
        A[Client/Director] -->|1. Creates| B[Builder]
        B -->|2. Configures step by step| B
        B -->|3. Builds| C[Product]
        C -->|4. Returns| A
    end
    
    style A fill:#e1f5ff
    style B fill:#fff4e1
    style C fill:#e8f5e9
```

---

## 2. Decorator Pattern

### UML Class Diagram

```mermaid
classDiagram
    class Beverage {
        <<interface>>
        +getDescription(): String
        +getCost(): double
    }
    
    class Espresso {
        +getDescription(): String
        +getCost(): double
    }
    
    class HouseBlend {
        +getDescription(): String
        +getCost(): double
    }
    
    class DarkRoast {
        +getDescription(): String
        +getCost(): double
    }
    
    class CondimentDecorator {
        <<abstract>>
        #Beverage beverage
        +CondimentDecorator(beverage: Beverage)
        +getDescription(): String*
        +getCost(): double*
    }
    
    class Milk {
        +Milk(beverage: Beverage)
        +getDescription(): String
        +getCost(): double
    }
    
    class Mocha {
        +Mocha(beverage: Beverage)
        +getDescription(): String
        +getCost(): double
    }
    
    class Whip {
        +Whip(beverage: Beverage)
        +getDescription(): String
        +getCost(): double
    }
    
    class Soy {
        +Soy(beverage: Beverage)
        +getDescription(): String
        +getCost(): double
    }
    
    Beverage <|.. Espresso : implements
    Beverage <|.. HouseBlend : implements
    Beverage <|.. DarkRoast : implements
    Beverage <|.. CondimentDecorator : implements
    CondimentDecorator <|-- Milk : extends
    CondimentDecorator <|-- Mocha : extends
    CondimentDecorator <|-- Whip : extends
    CondimentDecorator <|-- Soy : extends
    CondimentDecorator o-- Beverage : wraps
```

### Sequence Diagram - Decorating a Beverage

```mermaid
sequenceDiagram
    participant Client
    participant Espresso
    participant Mocha
    participant Whip
    
    Client->>Espresso: new Espresso()
    activate Espresso
    Espresso-->>Client: espresso
    deactivate Espresso
    
    Client->>Mocha: new Mocha(espresso)
    activate Mocha
    Mocha-->>Client: mocha wrapped espresso
    deactivate Mocha
    
    Client->>Whip: new Whip(mocha)
    activate Whip
    Whip-->>Client: whip wrapped mocha
    deactivate Whip
    
    Client->>Whip: getDescription()
    activate Whip
    Whip->>Mocha: getDescription()
    activate Mocha
    Mocha->>Espresso: getDescription()
    activate Espresso
    Espresso-->>Mocha: "Espresso"
    deactivate Espresso
    Mocha-->>Whip: "Espresso, Mocha"
    deactivate Mocha
    Whip-->>Client: "Espresso, Mocha, Whip"
    deactivate Whip
    
    Client->>Whip: getCost()
    activate Whip
    Whip->>Mocha: getCost()
    activate Mocha
    Mocha->>Espresso: getCost()
    activate Espresso
    Espresso-->>Mocha: 1.99
    deactivate Espresso
    Mocha-->>Whip: 1.99 + 0.20 = 2.19
    deactivate Mocha
    Whip-->>Client: 2.19 + 0.15 = 2.34
    deactivate Whip
```

### Pattern Structure - Wrapping Concept

```mermaid
graph LR
    subgraph "Decorator Wrapping"
        A[Espresso<br/>$1.99] -->|wrapped by| B[Mocha<br/>+$0.20]
        B -->|wrapped by| C[Whip<br/>+$0.15]
        C -->|wrapped by| D[Caramel<br/>+$0.30]
    end
    
    E[Final Cost:<br/>$2.64] -.->|Total| D
    
    style A fill:#8BC34A
    style B fill:#FFC107
    style C fill:#FF9800
    style D fill:#F44336
    style E fill:#2196F3,color:#fff
```

---

## 3. Pattern Comparison

### Structural Differences

```mermaid
graph TB
    subgraph "Builder Pattern - Creation Time"
        B1[Client] -->|Step 1| B2[Builder]
        B2 -->|Step 2| B2
        B2 -->|Step 3| B2
        B2 -->|Build| B3[Immutable Product]
    end
    
    subgraph "Decorator Pattern - Runtime"
        D1[Component] -->|Wrap| D2[Decorator 1]
        D2 -->|Wrap| D3[Decorator 2]
        D3 -->|Wrap| D4[Decorator 3]
        D4 -.->|Can add more| D5[Decorator N]
    end
    
    style B3 fill:#4CAF50,color:#fff
    style D1 fill:#2196F3,color:#fff
    style D4 fill:#FF5722,color:#fff
```

### When to Use Each Pattern

```mermaid
flowchart TD
    Start{Need to create<br/>or modify object?}
    
    Start -->|Create| Q1{Object has many<br/>optional parameters?}
    Start -->|Modify| Q2{Add features<br/>at runtime?}
    
    Q1 -->|Yes| Builder[Use Builder Pattern]
    Q1 -->|No| Simple[Use Simple Constructor]
    
    Q2 -->|Yes| Decorator[Use Decorator Pattern]
    Q2 -->|No| Inheritance[Consider Inheritance]
    
    Builder -->|Benefits| B1[• Readable code<br/>• Immutable objects<br/>• Step-by-step construction]
    Decorator -->|Benefits| D1[• Flexible extension<br/>• Runtime composition<br/>• Open/Closed Principle]
    
    style Builder fill:#4CAF50,color:#fff
    style Decorator fill:#2196F3,color:#fff
    style Start fill:#FF9800,color:#fff
```

---

## 4. Design Explanation

### Builder Pattern

**Purpose**: Separate the construction of a complex object from its representation.

**Key Components**:
1. **Product (House)**: The complex object being built
2. **Builder (HouseBuilder)**: Provides methods to construct parts of the Product
3. **Director (BuilderDemo)**: Constructs the object using the Builder interface

**How It Works**:
```
Client → Builder.setFoundation()
      → Builder.setStructure()
      → Builder.setRoof()
      → Builder.build() → Product (House)
```

**Advantages**:
- ✅ Handles telescoping constructor problem
- ✅ Immutable objects
- ✅ Readable, fluent interface
- ✅ Flexible object construction
- ✅ Step-by-step construction

**Disadvantages**:
- ❌ More code (need Builder class)
- ❌ Cannot change object after creation

---

### Decorator Pattern

**Purpose**: Attach additional responsibilities to an object dynamically.

**Key Components**:
1. **Component (Beverage)**: Interface for objects that can have responsibilities added
2. **Concrete Component (Espresso, HouseBlend)**: Objects to which additional responsibilities can be attached
3. **Decorator (CondimentDecorator)**: Maintains a reference to a Component object
4. **Concrete Decorators (Milk, Mocha, Whip)**: Add responsibilities to the component

**How It Works**:
```
Base Component (Espresso)
    ↓ wrapped by
Decorator 1 (Milk) - adds $0.10
    ↓ wrapped by
Decorator 2 (Mocha) - adds $0.20
    ↓ wrapped by
Decorator 3 (Whip) - adds $0.15
    
Total: $1.99 + $0.10 + $0.20 + $0.15 = $2.44
```

**Advantages**:
- ✅ More flexible than inheritance
- ✅ Add/remove responsibilities dynamically
- ✅ Combine decorators in any order
- ✅ Open/Closed Principle
- ✅ Single Responsibility Principle

**Disadvantages**:
- ❌ Many small objects
- ❌ Complexity in debugging
- ❌ Order of decorators matters

---

## 5. Real-World Examples

### Builder Pattern Examples
```mermaid
mindmap
    root((Builder<br/>Pattern))
        StringBuilder
            Append characters
            Build string
        HTTPRequest
            Set URL
            Set headers
            Set body
            Build request
        SQL Query
            SELECT
            FROM
            WHERE
            Build query
        UI Components
            Set properties
            Set style
            Build component
```

### Decorator Pattern Examples
```mermaid
mindmap
    root((Decorator<br/>Pattern))
        Java I/O
            FileInputStream
            BufferedInputStream
            DataInputStream
        UI Components
            ScrollPane
            BorderPane
            PaddingPane
        Middleware
            Authentication
            Logging
            Compression
        Text Formatting
            Bold
            Italic
            Underline
```

---

## 6. Summary Table

| Aspect | Builder Pattern | Decorator Pattern |
|--------|----------------|-------------------|
| **Type** | Creational | Structural |
| **Purpose** | Construct complex objects | Add features dynamically |
| **Timing** | Creation time | Runtime |
| **Flexibility** | Configure before build | Wrap anytime |
| **Immutability** | Usually immutable | Wraps existing objects |
| **Use Case** | Many optional parameters | Extend functionality |
| **Example** | Building a House | Adding toppings to Coffee |

---
