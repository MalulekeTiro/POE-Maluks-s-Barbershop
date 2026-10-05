package com.mycompany.chat_app;

import java.util.Scanner;

/**
 * Main class for the Wildlife Rescue Operations System.
 */
public class Chat_app {

    private static final Scanner scanner =
            new Scanner(System.in);

    private static final RescueCaseManager manager =
            new RescueCaseManager();

    public static void main(String[] args) {

        runApplication();
    }

    public static void runApplication() {

        boolean running = true;

        while (running) {

            displayMenu();

            int option =
                    readInt("Select an option: ");

            switch (option) {

                case 1:
                    createRescueCase();
                    break;

                case 2:
                    searchRescueCase();
                    break;

                case 3:
                    updateRescueStatus();
                    break;

                case 4:
                    displayAllRescueCases();
                    break;

                case 5:

                    System.out.println();
                    System.out.println(
                            manager.generateReport()
                    );

                    break;

                case 6:

                    running = false;

                    System.out.println(
                            "Thank you for using the "
                            + "Wildlife Rescue Operations System."
                    );

                    break;

                default:

                    System.out.println(
                            "Invalid menu selection. "
                            + "Please choose an option from 1 to 6."
                    );
            }
        }
    }

    private static void displayMenu() {

        System.out.println();
        System.out.println(
                "=============================================="
        );

        System.out.println(
                "      WILDLIFE RESCUE OPERATIONS SYSTEM"
        );

        System.out.println(
                "=============================================="
        );

        System.out.println(
                "1. Create Rescue Case"
        );

        System.out.println(
                "2. Search Rescue Case"
        );

        System.out.println(
                "3. Update Rescue Status"
        );

        System.out.println(
                "4. Display All Rescue Cases"
        );

        System.out.println(
                "5. Rescue Report"
        );

        System.out.println(
                "6. Exit"
        );

        System.out.println(
                "=============================================="
        );
    }

    private static void createRescueCase() {

        System.out.println(
                "\n--- CREATE RESCUE CASE ---"
        );

        String caseId =
                readRequiredString(
                        "Enter Rescue Case ID: "
                );

        if (manager.findRescueCase(caseId) != null) {

            System.out.println(
                    "Error: Rescue Case ID already exists."
            );

            return;
        }

        String animalName =
                readRequiredString(
                        "Enter Animal Name: "
                );

        String species =
                readRequiredString(
                        "Enter Species: "
                );

        String location =
                readRequiredString(
                        "Enter Rescue Location: "
                );

        String ranger =
                readRequiredString(
                        "Enter Assigned Ranger: "
                );

        int rescueDays =
                readPositiveInt(
                        "Enter Number of Rescue Days: "
                );

        double dailyCareCost =
                readPositiveDouble(
                        "Enter Daily Care Cost: "
                );

        System.out.println(
                "\nSelect Rescue Case Type:"
        );

        System.out.println(
                "1. Injured Animal Rescue"
        );

        System.out.println(
                "2. Orphaned Animal Rescue"
        );

        System.out.println(
                "3. Endangered Species Rescue"
        );

        int type =
                readInt(
                        "Select rescue type: "
                );

        RescueCase rescueCase;

        switch (type) {

            case 1:

                rescueCase =
                        createInjuredCase(
                                caseId,
                                animalName,
                                species,
                                location,
                                ranger,
                                rescueDays,
                                dailyCareCost
                        );

                break;

            case 2:

                rescueCase =
                        createOrphanedCase(
                                caseId,
                                animalName,
                                species,
                                location,
                                ranger,
                                rescueDays,
                                dailyCareCost
                        );

                break;

            case 3:

                rescueCase =
                        createEndangeredCase(
                                caseId,
                                animalName,
                                species,
                                location,
                                ranger,
                                rescueDays,
                                dailyCareCost
                        );

                break;

            default:

                System.out.println(
                        "Invalid rescue type. "
                        + "Rescue case was not created."
                );

                return;
        }

        if (manager.addRescueCase(rescueCase)) {

            System.out.println(
                    "\nRescue case successfully created."
            );

            System.out.println(
                    "Rescue Priority: "
                    + rescueCase.determineRescuePriority()
            );

            System.out.printf(
                    "Total Rescue Cost: R%,.2f%n",
                    rescueCase.calculateTotalRescueCost()
            );

        } else {

            System.out.println(
                    "Unable to create rescue case."
            );
        }
    }

    private static InjuredAnimalRescue createInjuredCase(
            String caseId,
            String animalName,
            String species,
            String location,
            String ranger,
            int days,
            double dailyCost) {

        System.out.println(
                "\n--- INJURED ANIMAL INFORMATION ---"
        );

        String injury =
                readRequiredString(
                        "Enter Injury Description: "
                );

        double veterinaryCost =
                readNonNegativeDouble(
                        "Enter Veterinary Treatment Cost: "
                );

        boolean surgery =
                readYesNo(
                        "Is Surgery Required? (Y/N): "
                );

        return new InjuredAnimalRescue(
                caseId,
                animalName,
                species,
                location,
                ranger,
                days,
                dailyCost,
                injury,
                veterinaryCost,
                surgery
        );
    }

    private static OrphanedAnimalRescue createOrphanedCase(
            String caseId,
            String animalName,
            String species,
            String location,
            String ranger,
            int days,
            double dailyCost) {

        System.out.println(
                "\n--- ORPHANED ANIMAL INFORMATION ---"
        );

        int age =
                readNonNegativeInt(
                        "Enter Estimated Age (Months): "
                );

        double feedingCost =
                readNonNegativeDouble(
                        "Enter Feeding Cost: "
                );

        boolean foster =
                readYesNo(
                        "Is Foster Care Required? (Y/N): "
                );

        return new OrphanedAnimalRescue(
                caseId,
                animalName,
                species,
                location,
                ranger,
                days,
                dailyCost,
                age,
                feedingCost,
                foster
        );
    }

    private static EndangeredSpeciesRescue createEndangeredCase(
            String caseId,
            String animalName,
            String species,
            String location,
            String ranger,
            int days,
            double dailyCost) {

        System.out.println(
                "\n--- ENDANGERED SPECIES INFORMATION ---"
        );

        String classification =
                readRequiredString(
                        "Enter Conservation Classification: "
                );

        double securityCost =
                readNonNegativeDouble(
                        "Enter Security Cost: "
                );

        boolean specialist =
                readYesNo(
                        "Is a Specialist Team Required? (Y/N): "
                );

        return new EndangeredSpeciesRescue(
                caseId,
                animalName,
                species,
                location,
                ranger,
                days,
                dailyCost,
                classification,
                securityCost,
                specialist
        );
    }

    private static void searchRescueCase() {

        System.out.println(
                "\n--- SEARCH RESCUE CASE ---"
        );

        String caseId =
                readRequiredString(
                        "Enter Rescue Case ID: "
                );

        RescueCase rescueCase =
                manager.findRescueCase(caseId);

        if (rescueCase == null) {

            System.out.println(
                    "No rescue case found with ID: "
                    + caseId
            );

        } else {

            System.out.println();

            System.out.println(
                    rescueCase.generateRescueSummary()
            );

            System.out.println();

            System.out.println(
                    rescueCase.getFullReportEntry()
            );
        }
    }

    private static void updateRescueStatus() {

        System.out.println(
                "\n--- UPDATE RESCUE STATUS ---"
        );

        String caseId =
                readRequiredString(
                        "Enter Rescue Case ID: "
                );

        RescueCase rescueCase =
                manager.findRescueCase(caseId);

        if (rescueCase == null) {

            System.out.println(
                    "No rescue case found with ID: "
                    + caseId
            );

            return;
        }

        System.out.println(
                "Current Status: "
                + rescueCase.getCurrentRescueStatus()
        );

        System.out.println(
                "1. Start Rescue Operation"
        );

        System.out.println(
                "2. Complete Rescue Operation"
        );

        System.out.println(
                "3. Set Under Observation"
        );

        int choice =
                readInt(
                        "Select status option: "
                );

        switch (choice) {

            case 1:

                rescueCase.startRescue();

                System.out.println(
                        "Rescue operation started."
                );

                break;

            case 2:

                rescueCase.completeRescue();

                System.out.println(
                        "Rescue operation completed."
                );

                break;

            case 3:

                manager.updateRescueStatus(
                        caseId,
                        "under observation"
                );

                System.out.println(
                        "Rescue status updated to "
                        + "Under Observation."
                );

                break;

            default:

                System.out.println(
                        "Invalid status option."
                );
        }

        System.out.println(
                "Current Status: "
                + rescueCase.getCurrentRescueStatus()
        );
    }

    private static void displayAllRescueCases() {

        System.out.println(
                "\n--- ALL RESCUE CASES ---"
        );

        if (manager.getNumberOfRescueCases() == 0) {

            System.out.println(
                    "No rescue cases are currently stored."
            );

            return;
        }

        for (RescueCase rescueCase :
                manager.getRescueCases()) {

            System.out.println(
                    "----------------------------------------------"
            );

            System.out.println(
                    rescueCase.generateRescueSummary()
            );
        }

        System.out.println(
                "----------------------------------------------"
        );
    }

    private static String readRequiredString(
            String prompt) {

        while (true) {

            System.out.print(prompt);

            String value =
                    scanner.nextLine().trim();

            if (InputValidator.isNotBlank(value)) {

                return value;
            }

            System.out.println(
                    "Input cannot be blank. "
                    + "Please try again."
            );
        }
    }

    private static int readInt(String prompt) {

        while (true) {

            System.out.print(prompt);

            String input =
                    scanner.nextLine().trim();

            try {

                return Integer.parseInt(input);

            } catch (NumberFormatException e) {

                System.out.println(
                        "Invalid number. "
                        + "Please enter a whole number."
                );
            }
        }
    }

    private static int readPositiveInt(
            String prompt) {

        while (true) {

            int value =
                    readInt(prompt);

            if (InputValidator.isPositive(value)) {

                return value;
            }

            System.out.println(
                    "Value must be greater than zero."
            );
        }
    }

    private static int readNonNegativeInt(
            String prompt) {

        while (true) {

            int value =
                    readInt(prompt);

            if (value >= 0) {

                return value;
            }

            System.out.println(
                    "Value cannot be negative."
            );
        }
    }

    private static double readPositiveDouble(
            String prompt) {

        while (true) {

            double value =
                    readDouble(prompt);

            if (InputValidator.isPositive(value)) {

                return value;
            }

            System.out.println(
                    "Value must be greater than zero."
            );
        }
    }

    private static double readNonNegativeDouble(
            String prompt) {

        while (true) {

            double value =
                    readDouble(prompt);

            if (InputValidator.isNonNegative(value)) {

                return value;
            }

            System.out.println(
                    "Value cannot be negative."
            );
        }
    }

    private static double readDouble(
            String prompt) {

        while (true) {

            System.out.print(prompt);

            String input =
                    scanner.nextLine().trim();

            try {

                double value =
                        Double.parseDouble(input);

                if (!Double.isNaN(value)
                        && !Double.isInfinite(value)) {

                    return value;
                }

            } catch (NumberFormatException ignored) {

                // Error message below keeps
                // the application running.
            }

            System.out.println(
                    "Invalid amount. "
                    + "Please enter a valid number."
            );
        }
    }

    private static boolean readYesNo(
            String prompt) {

        while (true) {

            System.out.print(prompt);

            String input =
                    scanner.nextLine().trim();

            if (input.equalsIgnoreCase("y")
                    || input.equalsIgnoreCase("yes")) {

                return true;
            }

            if (input.equalsIgnoreCase("n")
                    || input.equalsIgnoreCase("no")) {

                return false;
            }

            System.out.println(
                    "Please enter Y or N."
            );
        }
    }
}
