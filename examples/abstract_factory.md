```java
    // Abstract products
    interface Button {
        void render();
    }
    
    interface Checkbox {
        void toggle();
    }
    
    // Concrete products for Light theme
    class LightButton implements Button {
        @Override
        public void render() {
            System.out.println("Rendering light theme button");
        }
    }
    
    class LightCheckbox implements Checkbox {
        @Override
        public void toggle() {
            System.out.println("Toggling light theme checkbox");
        }
    }
    
    // Concrete products for Dark theme
    class DarkButton implements Button {
        @Override
        public void render() {
            System.out.println("Rendering dark theme button");
        }
    }
    
    class DarkCheckbox implements Checkbox {
        @Override
        public void toggle() {
            System.out.println("Toggling dark theme checkbox");
        }
    }
    
    // Abstract factory
    interface UIFactory {
        Button createButton();
        Checkbox createCheckbox();
    }
    
    // Concrete factories
    class LightThemeFactory implements UIFactory {
        @Override
        public Button createButton() {
            return new LightButton();
        }
        
        @Override
        public Checkbox createCheckbox() {
            return new LightCheckbox();
        }
    }
    
    class DarkThemeFactory implements UIFactory {
        @Override
        public Button createButton() {
            return new DarkButton();
        }
        
        @Override
        public Checkbox createCheckbox() {
            return new DarkCheckbox();
        }
    }
    
    // Usage example
    class Main {
        public static void main(String[] args) {
            // Create a light theme UI
            UIFactory lightFactory = new LightThemeFactory();
            Button lightButton = lightFactory.createButton();
            Checkbox lightCheckbox = lightFactory.createCheckbox();
            
            lightButton.render();    // Output: Rendering light theme button
            lightCheckbox.toggle();  // Output: Toggling light theme checkbox
            
            // Create a dark theme UI
            UIFactory darkFactory = new DarkThemeFactory();
            Button darkButton = darkFactory.createButton();
            Checkbox darkCheckbox = darkFactory.createCheckbox();
            
            darkButton.render();     // Output: Rendering dark theme button
            darkCheckbox.toggle();   // Output: Toggling dark theme checkbox
        }
    }
    ```
