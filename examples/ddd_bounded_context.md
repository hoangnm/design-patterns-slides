In an e-commerce system:
    ```java
    // Shipping Context
    class Product {
        private Weight weight;
        private Dimensions dimensions;
        private ShippingCategory category;
    }

    // Inventory Context
    class Product {
        private StockLevel currentStock;
        private Location storageLocation;
        private ReorderPoint reorderLevel;
    }

    // Sales Context
    class Product {
        private Price retailPrice;
        private Discount currentDiscount;
        private Category marketingCategory;
    }
    ```