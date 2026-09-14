import java.util.Scanner;

public class Main {
    private static final Scanner scanner = new Scanner(System.in);
    private static final HospitalSystem hospital = new HospitalSystem();

    public static void main(String[] args) {
        boolean running = true;

        while (running) {
            displayMenu();

            try {
                int choice = Integer.parseInt(scanner.nextLine());

                switch (choice) {
                    case 1:
                        registerPatient();
                        break;
                    case 2:
                        searchPatient();
                        break;
                    case 3:
                        updatePatient();
                        break;
                    case 4:
                        deletePatient();
                        break;
                    case 5:
                        hospital.displayAllPatients();
                        break;
                    case 6:
                        allocateBed();
                        break;
                    case 7:
                        releaseBed();
                        break;
                    case 8:
                        hospital.displayWardLayout();
                        break;
                    case 9:
                        hospital.displayAvailableBeds();
                        break;
                    case 10:
                        hospital.displayOccupiedBeds();
                        break;
                    case 11:
                        displayReports();
                        break;
                    case 12:
                        hospital.sortPatientsById();
                        System.out.println("Patients sorted by Patient ID.");
                        break;
                    case 0:
                        running = false;
                        System.out.println("Thank you for using the Hospital Patient Admission System.");
                        break;
                    default:
                        System.out.println("Invalid option.");
                }

            } catch (NumberFormatException e) {
                System.out.println("Please enter a valid number.");
            } catch (HospitalException e) {
                System.out.println("Error: " + e.getMessage());
            }

            System.out.println();
        }
    }

    private static void displayMenu() {
        System.out.println("======================================");
        System.out.println(" MEDICARE HOSPITAL PATIENT SYSTEM");
        System.out.println("======================================");
        System.out.println("1. Register Patient");
        System.out.println("2. Search Patient");
        System.out.println("3. Update Patient");
        System.out.println("4. Delete Patient");
        System.out.println("5. Display All Patients");
        System.out.println("6. Allocate Bed");
        System.out.println("7. Release Bed");
        System.out.println("8. Display Ward Layout");
        System.out.println("9. Display Available Beds");
        System.out.println("10. Display Occupied Beds");
        System.out.println("11. Generate Reports");
        System.out.println("12. Sort Patients by Patient ID");
        System.out.println("0. Exit");
        System.out.print("Enter option: ");
    }

    private static void registerPatient() throws HospitalException {
        System.out.println();
        System.out.println("REGISTER PATIENT");

        System.out.print("Patient ID: ");
        String id = scanner.nextLine();

        System.out.print("First Name: ");
        String firstName = scanner.nextLine();

        System.out.print("Last Name: ");
        String lastName = scanner.nextLine();

        System.out.print("Age: ");
        int age = Integer.parseInt(scanner.nextLine());

        System.out.print("Gender: ");
        String gender = scanner.nextLine();

        System.out.print("Medical Condition: ");
        String condition = scanner.nextLine();

        System.out.println("1. Inpatient");
        System.out.println("2. Outpatient");
        System.out.println("3. Emergency");
        System.out.print("Select category: ");

        int categoryChoice = Integer.parseInt(scanner.nextLine());

        Patient patient;

        switch (categoryChoice) {
            case 1:
                patient = new Inpatient(
                        id,
                        firstName,
                        lastName,
                        age,
                        gender,
                        condition,
                        1,
                        "Not Allocated"
                );
                break;

            case 2:
                patient = new Patient(
                        id,
                        firstName,
                        lastName,
                        age,
                        gender,
                        condition,
                        PatientCategory.OUTPATIENT
                );
                break;

            case 3:
                patient = new Patient(
                        id,
                        firstName,
                        lastName,
                        age,
                        gender,
                        condition,
                        PatientCategory.EMERGENCY
                );
                break;

            default:
                throw new HospitalException("Invalid patient category.");
        }

        hospital.registerPatient(patient);

        System.out.println("Patient registered successfully.");
    }

    private static void searchPatient() throws HospitalException {
        System.out.print("Enter Patient ID: ");
        String id = scanner.nextLine();

        Patient patient = hospital.findPatient(id);

        if (patient == null) {
            throw new HospitalException("Patient not found.");
        }

        System.out.println();
        patient.displayDetails();
    }

    private static void updatePatient() throws HospitalException {
        System.out.print("Enter Patient ID: ");
        String id = scanner.nextLine();

        Patient patient = hospital.findPatient(id);

        if (patient == null) {
            throw new HospitalException("Patient not found.");
        }

        System.out.print("First Name: ");
        String firstName = scanner.nextLine();

        System.out.print("Last Name: ");
        String lastName = scanner.nextLine();

        System.out.print("Age: ");
        int age = Integer.parseInt(scanner.nextLine());

        System.out.print("Gender: ");
        String gender = scanner.nextLine();

        System.out.print("Medical Condition: ");
        String condition = scanner.nextLine();

        hospital.updatePatient(
                id,
                firstName,
                lastName,
                age,
                gender,
                condition
        );

        System.out.println("Patient updated successfully.");
    }

    private static void deletePatient() throws HospitalException {
        System.out.print("Enter Patient ID: ");
        String id = scanner.nextLine();

        hospital.deletePatient(id);

        System.out.println("Patient deleted successfully.");
    }

    private static void allocateBed() throws HospitalException {
        System.out.print("Enter Inpatient ID: ");
        String id = scanner.nextLine();

        hospital.allocateBed(id);
    }

    private static void releaseBed() throws HospitalException {
        System.out.print("Enter Bed Number: ");
        String bedNumber = scanner.nextLine();

        hospital.releaseBed(bedNumber);
    }

    private static void displayReports() {
        System.out.println();
        System.out.println("======================================");
        System.out.println("           WARD REPORT");
        System.out.println("======================================");

        System.out.println("Total Registered Patients: "
                + hospital.getTotalPatients());

        System.out.println("Total Occupied Beds: "
                + hospital.getOccupiedBeds());

        System.out.println("Total Available Beds: "
                + hospital.getAvailableBeds());

        System.out.printf(
                "Ward Occupancy: %.2f%%%n",
                hospital.getOccupancyPercentage()
        );

        System.out.println();
        hospital.displayAvailableBeds();

        System.out.println();
        hospital.displayOccupiedBeds();

        System.out.println();
        hospital.displayAllPatients();
    }
}
