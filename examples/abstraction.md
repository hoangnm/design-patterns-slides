# Abstraction in Object-Oriented Programming

Abstraction is one of the four fundamental principles of object-oriented programming (OOP). It focuses on hiding complex implementation details and showing only the necessary features of an object. Abstraction lets you focus on what an object does rather than how it does it.

## Key Concepts of Abstraction

1. **Simplified Interface**: Exposing only what's necessary to the outside world
2. **Implementation Hiding**: Concealing the complex internal workings
3. **Focus on Behavior**: Emphasizing what an object does, not how it does it
4. **Reduced Complexity**: Making systems easier to understand and use

## Example in Java

```java
// Abstract class representing a generic vehicle
abstract class Vehicle {
    // Common properties
    private String make;
    private String model;
    private int year;
    
    // Constructor
    public Vehicle(String make, String model, int year) {
        this.make = make;
        this.model = model;
        this.year = year;
    }
    
    // Getters
    public String getMake() { return make; }
    public String getModel() { return model; }
    public int getYear() { return year; }
    
    // Abstract methods - defined by subclasses
    public abstract void startEngine();
    public abstract void stopEngine();
    public abstract double calculateFuelEfficiency();
    
    // Concrete method shared by all vehicles
    public void displayInfo() {
        System.out.println("Vehicle: " + year + " " + make + " " + model);
    }
}

// Concrete implementation for a car
class Car extends Vehicle {
    private int fuelTankCapacity;
    private int averageMpg;
    private boolean engineRunning;
    
    public Car(String make, String model, int year, int fuelTankCapacity, int averageMpg) {
        super(make, model, year);
        this.fuelTankCapacity = fuelTankCapacity;
        this.averageMpg = averageMpg;
        this.engineRunning = false;
    }
    
    @Override
    public void startEngine() {
        engineRunning = true;
        System.out.println("Car engine started. Vroom!");
    }
    
    @Override
    public void stopEngine() {
        engineRunning = false;
        System.out.println("Car engine stopped.");
    }
    
    @Override
    public double calculateFuelEfficiency() {
        return averageMpg;
    }
    
    public double getRange() {
        return fuelTankCapacity * averageMpg;
    }
}

// Another concrete implementation
class ElectricCar extends Vehicle {
    private int batteryCapacity;
    private int milesPerCharge;
    private boolean motorRunning;
    
    public ElectricCar(String make, String model, int year, int batteryCapacity, int milesPerCharge) {
        super(make, model, year);
        this.batteryCapacity = batteryCapacity;
        this.milesPerCharge = milesPerCharge;
        this.motorRunning = false;
    }
    
    @Override
    public void startEngine() {
        motorRunning = true;
        System.out.println("Electric motor activated silently.");
    }
    
    @Override
    public void stopEngine() {
        motorRunning = false;
        System.out.println("Electric motor deactivated.");
    }
    
    @Override
    public double calculateFuelEfficiency() {
        // For electric cars, we convert to MPGe (Miles Per Gallon equivalent)
        return milesPerCharge * 0.33; // Conversion factor
    }
    
    public double getRange() {
        return batteryCapacity * milesPerCharge / 100.0;
    }
}

// Client code
public class Main {
    public static void main(String[] args) {
        // We work with the abstraction, not the implementation
        Vehicle gasCar = new Car("Toyota", "Camry", 2022, 14, 35);
        Vehicle electricCar = new ElectricCar("Tesla", "Model 3", 2023, 75, 4);
        
        // Using abstract methods without knowing the implementation details
        System.out.println("Working with gas car:");
        gasCar.displayInfo();
        gasCar.startEngine();
        System.out.println("Fuel efficiency: " + gasCar.calculateFuelEfficiency() + " MPG");
        gasCar.stopEngine();
        
        System.out.println("\nWorking with electric car:");
        electricCar.displayInfo();
        electricCar.startEngine();
        System.out.println("Fuel efficiency: " + electricCar.calculateFuelEfficiency() + " MPGe");
        electricCar.stopEngine();
        
        // Note: We can't access implementation-specific methods through the Vehicle reference
        // System.out.println("Range: " + gasCar.getRange());  // Compilation error
        
        // But we can if we cast to the specific type (breaking abstraction)
        System.out.println("\nBreaking abstraction to access specific implementations:");
        System.out.println("Gas car range: " + ((Car)gasCar).getRange() + " miles");
        System.out.println("Electric car range: " + ((ElectricCar)electricCar).getRange() + " miles");
    }
}
```

## Benefits of Abstraction

1. **Reduced Complexity**: Users only need to understand the simplified interface
2. **Easier Maintenance**: Implementation details can change without affecting client code
3. **Focused Development**: Developers can focus on high-level functionality first
4. **Improved Security**: Internal details and data are hidden from outside access
5. **Code Reusability**: Abstract classes and interfaces promote reusable design

## Real-World Analogy

Think of abstraction like driving a car:
- You interact with a simple interface (steering wheel, pedals, gear shift)
- You don't need to understand the complex mechanics under the hood
- Different car models have different implementations but similar interfaces
- You focus on what the car does (transportation) not how it works internally
