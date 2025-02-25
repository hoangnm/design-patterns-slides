```java
        // Inheritance approach (less flexible)
        class Animal {
            void move() { }
        }
        class Bird extends Animal {
            void fly() { }
        }

        // Composition approach (more flexible)
        interface Movable {
            void move();
        }
        class FlyBehavior {
            void fly() { }
        }
        class Bird {
            private Movable movable;
            private FlyBehavior flyBehavior;
        }
        ```