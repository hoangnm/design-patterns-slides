```java
    // Rich Domain Model Example
    public class Order {
        private List<OrderLine> lines;
        private Customer customer;
        private OrderStatus status;
        private Money totalAmount;

        public void addProduct(Product product, int quantity) {
            validateProduct(product);
            validateQuantity(quantity);
            
            OrderLine line = new OrderLine(product, quantity);
            lines.add(line);
            recalculateTotal();
        }

        public void confirm() {
            validateOrderCanBeConfirmed();
            status = OrderStatus.CONFIRMED;
            notifyCustomer();
        }

        private void validateOrderCanBeConfirmed() {
            if (lines.isEmpty()) {
                throw new OrderDomainException("Cannot confirm empty order");
            }
            if (!customer.hasGoodStanding()) {
                throw new OrderDomainException("Customer not in good standing");
            }
        }

        private void recalculateTotal() {
            totalAmount = lines.stream()
                .map(OrderLine::getLineTotal)
                .reduce(Money.ZERO, Money::add);
        }
    }
    ```