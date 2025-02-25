# Liskov Substitution Principle Examples

This example demonstrates good and bad implementations of the Liskov Substitution Principle (LSP) using a bird hierarchy.

## Bad Example - Violating LSP

```java
// Base class defining a bird
public class Bird {
    public void fly() {
        // Implementation for flying
        System.out.println("Flying high");
    }
}

// Ostrich inherits from Bird but can't actually fly!
public class Ostrich extends Bird {
    @Override
    public void fly() {
        throw new UnsupportedOperationException("Ostriches can't fly!");
    }
}

// This code will fail for Ostrich
public class BirdManager {
    public void makeBirdFly(Bird bird) {
        // This will throw exception for Ostrich
        bird.fly();
    }
    
    public void letBirdsFly() {
        List<Bird> birds = Arrays.asList(
            new Bird(),
            new Ostrich()  // This will break!
        );
        
        for (Bird bird : birds) {
            makeBirdFly(bird);  // Fails for Ostrich
        }
    }
}
```

Problems with this implementation:
1. Ostrich breaks Bird's behavior contract
2. Client code cannot rely on fly() working
3. Violates "is-a" relationship (not every Bird can fly)
4. Forces client code to check types
5. Makes polymorphism dangerous

## Good Example - Following LSP

```java
// Base interface for all birds
public interface Bird {
    void move();
    void makeSound();
}

// Interface for flying capability
public interface FlyingBird extends Bird {
    void fly();
    void land();
}

// A bird that can fly
public class Sparrow implements FlyingBird {
    @Override
    public void move() {
        System.out.println("Hopping around");
    }
    
    @Override
    public void makeSound() {
        System.out.println("Chirp chirp");
    }
    
    @Override
    public void fly() {
        System.out.println("Flying through air");
    }
    
    @Override
    public void land() {
        System.out.println("Landing on branch");
    }
}

// A bird that cannot fly
public class Ostrich implements Bird {
    @Override
    public void move() {
        System.out.println("Running fast");
    }
    
    @Override
    public void makeSound() {
        System.out.println("Boom boom");
    }
}

// Client code that works correctly
public class BirdManager {
    public void exerciseBird(Bird bird) {
        // All birds can do these
        bird.move();
        bird.makeSound();
    }
    
    public void makeFlyingBirdFly(FlyingBird bird) {
        // Only flying birds will be passed here
        bird.fly();
        bird.land();
    }
}

// Usage example
public class BirdSanctuary {
    public void manageBirds() {
        BirdManager manager = new BirdManager();
        
        Bird ostrich = new Ostrich();
        FlyingBird sparrow = new Sparrow();
        
        // These work fine
        manager.exerciseBird(ostrich);
        manager.exerciseBird(sparrow);
        
        // This is safe - only FlyingBird
        manager.makeFlyingBirdFly(sparrow);
        
        // This won't compile - ostrich is not FlyingBird
        // manager.makeFlyingBirdFly(ostrich); // Compiler error
    }
}
```

Benefits of this implementation:
1. Clear separation of capabilities
2. Compile-time safety
3. No runtime surprises
4. Proper abstraction hierarchy
5. Easy to add new bird types

## Key LSP Principles Demonstrated:

1. **Behavioral Subtyping**:
   - Each bird type has appropriate capabilities
   - No unexpected exceptions or behavior

2. **Contract Adherence**:
   - Base methods work for all subtypes
   - No weakening of promises

3. **Interface Segregation**:
   - Flying separated from basic bird behavior
   - Clients depend only on methods they use

When to Apply LSP:
1. When modeling real-world hierarchies
2. When some subtypes can't support base class behavior
3. When clients need reliable behavior
4. When using inheritance and interfaces
5. When designing extensible systems

Remember: LSP ensures that subtypes can always be used in place of their base types without breaking the program's correctness. The bird example shows how to properly model capabilities that aren't universal to all subtypes.
