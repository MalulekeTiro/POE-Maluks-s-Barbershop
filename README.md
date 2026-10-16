
import java.util.Scanner;

// Error Message Class
class OrderMessages {

    public static final String[] MESSAGES = {
        "Item number must be numeric.",
        "Item number is too low. It cannot be less than 0.",
        "Item number is too high. It cannot be greater than 9999.",
        "Quantity must be numeric.",
        "Quantity is too low. It must be at least 1.",
        "Quantity is too high. It cannot be greater than 12.",
        "The item number does not correspond to a valid item."
    };
}

// Custom Exception Class
class OrderException extends Exception {

    public OrderException(String message) {
        super(message);
    }
}

// Main Application Class
public class Main {

    public static void main(String[] args) {

        Scanner input = new Scanner(System.in);

        // Inventory
        int[] itemNumbers = {111, 222, 333, 444};
        double[] itemPrices = {0.89, 1.47, 2.43, 5.99};

        try {
            // Enter item number
            System.out.print("Enter item number: ");
            String itemInput = input.nextLine();

            int itemNumber;

            try {
                itemNumber = Integer.parseInt(itemInput);
            } catch (NumberFormatException e) {
                throw new OrderException(OrderMessages.MESSAGES[0]);
            }

            // Validate item number
            if (itemNumber < 0) {
                throw new OrderException(OrderMessages.MESSAGES[1]);
            }

            if (itemNumber > 9999) {
                throw new OrderException(OrderMessages.MESSAGES[2]);
            }

            // Enter quantity
            System.out.print("Enter quantity: ");
            String quantityInput = input.nextLine();

            int quantity;

            try {
                quantity = Integer.parseInt(quantityInput);
            } catch (NumberFormatException e) {
                throw new OrderException(OrderMessages.MESSAGES[3]);
            }

            // Validate quantity
            if (quantity < 1) {
                throw new OrderException(OrderMessages.MESSAGES[4]);
            }

            if (quantity > 12) {
                throw new OrderException(OrderMessages.MESSAGES[5]);
            }

            // Search for the item
            int itemIndex = -1;

            for (int i = 0; i < itemNumbers.length; i++) {
                if (itemNumbers[i] == itemNumber) {
                    itemIndex = i;
                    break;
                }
            }

            // Check if the item exists
            if (itemIndex == -1) {
                throw new OrderException(OrderMessages.MESSAGES[6]);
            }

            // Calculate total amount
            double total = itemPrices[itemIndex] * quantity;

            // Display order details
            System.out.println("\nOrder successful!");
            System.out.println("Item number: " + itemNumber);
            System.out.println("Quantity: " + quantity);

            System.out.printf("Price per item: $%.2f%n",
                    itemPrices[itemIndex]);

            System.out.printf("Total amount due: $%.2f%n",
                    total);

        } catch (OrderException e) {
            System.out.println("Error: " + e.getMessage());

        } finally {
            input.close();
        }
    }
}
