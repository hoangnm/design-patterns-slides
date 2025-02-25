# Dependency Inversion Principle Example

## Bad Design (Violating DIP)
```mermaid
classDiagram
    class OrderService {
        -MySQLDatabase database
        +saveOrder(Order)
    }
    class MySQLDatabase {
        +save(Order)
    }
    OrderService --> MySQLDatabase
```

- High-level module (OrderService) depends directly on low-level module (MySQLDatabase)
- Changes to database implementation require changes to OrderService
- Tightly coupled design

## Good Design (Following DIP)
```mermaid
classDiagram
    class OrderService {
        -OrderRepository repository
        +saveOrder(Order)
    }
    class OrderRepository {
        <<interface>>
        +save(Order)
    }
    class MySQLRepository {
        +save(Order)
    }
    class MongoRepository {
        +save(Order)
    }
    
    OrderService --> OrderRepository
    OrderRepository <|.. MySQLRepository
    OrderRepository <|.. MongoRepository
```

- High-level module (OrderService) depends on abstraction (OrderRepository)
- Low-level modules implement the abstraction
- Loose coupling allows easy switching between implementations
- Both high-level and low-level modules depend on abstractions
