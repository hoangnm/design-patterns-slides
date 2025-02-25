```java
    // Value Object
    class Money {
        private final BigDecimal amount;
        private final Currency currency;

        public Money(BigDecimal amount, Currency currency) {
            this.amount = amount;
            this.currency = currency;
        }

        public Money add(Money other) {
            if (!this.currency.equals(other.currency)) {
                throw new IllegalArgumentException("Cannot add different currencies");
            }
            return new Money(this.amount.add(other.amount), this.currency);
        }
    }

    // Entity
    class Order {
        private OrderId id;
        private CustomerId customerId;
        private List<OrderLine> orderLines;
        private OrderStatus status;

        public void addProduct(Product product, int quantity) {
            validateOrder();
            OrderLine line = new OrderLine(product, quantity);
            orderLines.add(line);
            publishEvent(new OrderLineAddedEvent(this, line));
        }

        public Money calculateTotal() {
            return orderLines.stream()
                           .map(OrderLine::getTotal)
                           .reduce(Money.ZERO, Money::add);
        }

        private void validateOrder() {
            if (status != OrderStatus.DRAFT) {
                throw new OrderAlreadyConfirmedException();
            }
        }
    }

    // Aggregate Root
    class Customer {
        private CustomerId id;
        private String name;
        private List<Order> orders;
        private CustomerStatus status;

        public void placeOrder(Order order) {
            validateCustomerCanOrder();
            orders.add(order);
            publishEvent(new OrderPlacedEvent(this, order));
        }

        private void validateCustomerCanOrder() {
            if (status != CustomerStatus.ACTIVE) {
                throw new CustomerNotActiveException();
            }
        }
    }
    ```