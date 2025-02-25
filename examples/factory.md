```java
    // Product interface
    interface Vehicle {
        void drive();
    }

    // Concrete products
    class Car implements Vehicle {
        @Override
        public void drive() {
            System.out.println("Driving a car");
        }
    }

    class Bike implements Vehicle {
        @Override
        public void drive() {
            System.out.println("Riding a bike");
        }
    }

    // Factory
    class VehicleFactory {
        public Vehicle createVehicle(String type) {
            switch(type.toLowerCase()) {
                case "car":
                    return new Car();
                case "bike":
                    return new Bike();
                default:
                    throw new IllegalArgumentException("Vehicle type not supported");
            }
        }
    }

    // Usage example
    class Main {
        public static void main(String[] args) {
            VehicleFactory factory = new VehicleFactory();
            
            Vehicle car = factory.createVehicle("car");
            car.drive();  // Output: Driving a car
            
            Vehicle bike = factory.createVehicle("bike");
            bike.drive(); // Output: Riding a bike
        }
    }
    ```