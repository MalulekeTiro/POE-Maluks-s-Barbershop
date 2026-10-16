
import java.io.File;
import java.io.FileNotFoundException;
import java.io.PrintWriter;
import java.util.Scanner;

public class Main {

    public static void main(String[] args) {

        // Input and output files
        File inputFile = new File("input.txt");
        File outputFile = new File("output.txt");

        try (
            Scanner fileReader = new Scanner(inputFile);
            PrintWriter fileWriter = new PrintWriter(outputFile)
        ) {

            System.out.println("ACCOUNT NUMBER VALIDATION");
            System.out.println("--------------------------");

            while (fileReader.hasNextLine()) {

                String accountNumber = fileReader.nextLine().trim();

                // Skip empty lines
                if (accountNumber.isEmpty()) {
                    continue;
                }

                // Check that the account number has exactly six digits
                if (!accountNumber.matches("\\d{6}")) {
                    System.out.println(
                        accountNumber + " - INVALID"
                    );
                    continue;
                }

                // Calculate the sum of the first five digits
                int sum = 0;

                for (int i = 0; i < 5; i++) {
                    sum += Character.getNumericValue(
                        accountNumber.charAt(i)
                    );
                }

                // Calculate the remainder
                int remainder = sum % 10;

                // Get the last digit
                int lastDigit = Character.getNumericValue(
                    accountNumber.charAt(5)
                );

                // Validate the account number
                if (remainder == lastDigit) {

                    System.out.println(
                        accountNumber + " - VALID"
                    );

                    // Write only valid account numbers
                    fileWriter.println(accountNumber);

                } else {

                    System.out.println(
                        accountNumber + " - INVALID"
                    );
                }
            }

            System.out.println("--------------------------");
            System.out.println("Validation completed.");
            System.out.println("Valid account numbers saved to output.txt");

        } catch (FileNotFoundException e) {

            System.out.println(
                "Error: input.txt could not be found."
            );

        }
    }
}
