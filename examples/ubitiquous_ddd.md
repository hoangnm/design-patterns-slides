```java
    // Poor ubiquitous language
    class DataProcessor {
        void process(Data input) {
            if (input.getStatus() == 1) {
                updateRecords(input);
            }
        }
    }

    // Good ubiquitous language
    class OrderProcessor {
        void confirmOrder(Order order) {
            if (order.isPendingConfirmation()) {
                order.confirm();
            }
        }
    }
    ```