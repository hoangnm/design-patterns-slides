```java
        // Bad: High-level module depending on low-level module
        class OrderService {
            private MySQLDatabase database; // Concrete dependency
            
            public void saveOrder(Order order) {
                database.save(order); // Direct dependency on MySQL
            }
        }

        // Good: Using Dependency Inversion
        interface OrderRepository {
            void save(Order order);
        }

        class OrderService {
            private OrderRepository repository; // Abstract dependency
            
            public OrderService(OrderRepository repository) {
                this.repository = repository;
            }
            
            public void saveOrder(Order order) {
                repository.save(order); // Depends on abstraction
            }
        }
        ```
