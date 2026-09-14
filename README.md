import java.util.ArrayList;
import java.util.Comparator;

public class HospitalSystem {
    private final ArrayList<Patient> patients;
    private final Patient[][] beds;

    public HospitalSystem() {
        patients = new ArrayList<>();
        beds = new Patient[4][5];
    }

    public void registerPatient(Patient patient) throws HospitalException {
        if (findPatient(patient.getPatientId()) != null) {
            throw new HospitalException("Patient ID already exists.");
        }

        if (patient.getAge() < 0 || patient.getAge() > 120) {
            throw new HospitalException("Invalid patient age.");
        }

        patients.add(patient);
    }

    public Patient findPatient(String patientId) {
        for (Patient patient : patients) {
            if (patient.getPatientId().equalsIgnoreCase(patientId)) {
                return patient;
            }
        }
        return null;
    }

    public void updatePatient(String patientId, String firstName, String lastName,
                              int age, String gender, String condition)
            throws HospitalException {

        Patient patient = findPatient(patientId);

        if (patient == null) {
            throw new HospitalException("Patient not found.");
        }

        if (age < 0 || age > 120) {
            throw new HospitalException("Invalid patient age.");
        }

        patient.setFirstName(firstName);
        patient.setLastName(lastName);
        patient.setAge(age);
        patient.setGender(gender);
        patient.setMedicalCondition(condition);
    }

    public void deletePatient(String patientId) throws HospitalException {
        Patient patient = findPatient(patientId);

        if (patient == null) {
            throw new HospitalException("Patient not found.");
        }

        if (patient instanceof Inpatient) {
            Inpatient inpatient = (Inpatient) patient;

            if (!inpatient.getBedNumber().equals("Not Allocated")) {
                releaseBed(inpatient.getBedNumber());
            }
        }

        patients.remove(patient);
    }

    public void displayAllPatients() {
        ArrayList<Patient> sortedPatients = new ArrayList<>(patients);

        sortedPatients.sort(
                Comparator.comparing(Patient::getLastName)
                        .thenComparing(Patient::getFirstName)
        );

        if (sortedPatients.isEmpty()) {
            System.out.println("No registered patients.");
            return;
        }

        for (Patient patient : sortedPatients) {
            System.out.println("------------------------------");
            patient.displayDetails();
        }
    }

    public void sortPatientsById() {
        patients.sort(Comparator.comparing(Patient::getPatientId));
    }

    public void allocateBed(String patientId) throws HospitalException {
        Patient patient = findPatient(patientId);

        if (patient == null) {
            throw new HospitalException("Patient not found.");
        }

        if (!(patient instanceof Inpatient)) {
            throw new HospitalException("Only inpatients may be allocated a bed.");
        }

        Inpatient inpatient = (Inpatient) patient;

        if (!inpatient.getBedNumber().equals("Not Allocated")) {
            throw new HospitalException("Patient already has a bed.");
        }

        for (int row = 0; row < beds.length; row++) {
            for (int column = 0; column < beds[row].length; column++) {
                if (beds[row][column] == null) {
                    String bedNumber = getBedNumber(row, column);

                    beds[row][column] = inpatient;
                    inpatient.setWardNumber(1);
                    inpatient.setBedNumber(bedNumber);

                    System.out.println("Bed " + bedNumber + " allocated successfully.");
                    return;
                }
            }
        }

        throw new HospitalException("No beds are available.");
    }

    public void releaseBed(String bedNumber) throws HospitalException {
        for (int row = 0; row < beds.length; row++) {
            for (int column = 0; column < beds[row].length; column++) {
                if (getBedNumber(row, column).equalsIgnoreCase(bedNumber)) {

                    if (beds[row][column] == null) {
                        throw new HospitalException("Bed is already available.");
                    }

                    Patient patient = beds[row][column];

                    if (patient instanceof Inpatient) {
                        Inpatient inpatient = (Inpatient) patient;
                        inpatient.setBedNumber("Not Allocated");
                        inpatient.setWardNumber(1);
                    }

                    beds[row][column] = null;

                    System.out.println("Bed " + bedNumber + " released successfully.");
                    return;
                }
            }
        }

        throw new HospitalException("Invalid bed number.");
    }

    private String getBedNumber(int row, int column) {
        int number = row * 5 + column + 1;
        return String.format("B%02d", number);
    }

    public void displayWardLayout() {
        System.out.println();
        System.out.println("WARD 1 - BED LAYOUT");
        System.out.println("------------------------------");

        for (int row = 0; row < beds.length; row++) {
            for (int column = 0; column < beds[row].length; column++) {
                String bedNumber = getBedNumber(row, column);

                if (beds[row][column] == null) {
                    System.out.printf("%-8s", bedNumber);
                } else {
                    System.out.printf("%-8s", bedNumber + "*");
                }
            }

            System.out.println();
        }

        System.out.println("* = Occupied");
    }

    public void displayAvailableBeds() {
        System.out.println("AVAILABLE BEDS:");

        boolean found = false;

        for (int row = 0; row < beds.length; row++) {
            for (int column = 0; column < beds[row].length; column++) {
                if (beds[row][column] == null) {
                    System.out.print(getBedNumber(row, column) + " ");
                    found = true;
                }
            }
        }

        if (!found) {
            System.out.println("No beds available.");
        } else {
            System.out.println();
        }
    }

    public void displayOccupiedBeds() {
        System.out.println("OCCUPIED BEDS:");

        boolean found = false;

        for (int row = 0; row < beds.length; row++) {
            for (int column = 0; column < beds[row].length; column++) {
                if (beds[row][column] != null) {
                    Patient patient = beds[row][column];

                    System.out.println(
                            getBedNumber(row, column) + " - " +
                            patient.getPatientId() + " - " +
                            patient.getFirstName() + " " +
                            patient.getLastName()
                    );

                    found = true;
                }
            }
        }

        if (!found) {
            System.out.println("No occupied beds.");
        }
    }

    public int getTotalPatients() {
        return patients.size();
    }

    public int getOccupiedBeds() {
        int count = 0;

        for (int row = 0; row < beds.length; row++) {
            for (int column = 0; column < beds[row].length; column++) {
                if (beds[row][column] != null) {
                    count++;
                }
            }
        }

        return count;
    }

    public int getAvailableBeds() {
        return 20 - getOccupiedBeds();
    }

    public double getOccupancyPercentage() {
        return (getOccupiedBeds() / 20.0) * 100;
    }
}



PATIENT




public class Patient {
    private String patientId;
    private String firstName;
    private String lastName;
    private int age;
    private String gender;
    private String medicalCondition;
    private PatientCategory category;

    public Patient(String patientId, String firstName, String lastName, int age,
                   String gender, String medicalCondition, PatientCategory category) {
        this.patientId = patientId;
        this.firstName = firstName;
        this.lastName = lastName;
        this.age = age;
        this.gender = gender;
        this.medicalCondition = medicalCondition;
        this.category = category;
    }

    public String getPatientId() {
        return patientId;
    }

    public void setPatientId(String patientId) {
        this.patientId = patientId;
    }

    public String getFirstName() {
        return firstName;
    }

    public void setFirstName(String firstName) {
        this.firstName = firstName;
    }

    public String getLastName() {
        return lastName;
    }

    public void setLastName(String lastName) {
        this.lastName = lastName;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public String getGender() {
        return gender;
    }

    public void setGender(String gender) {
        this.gender = gender;
    }

    public String getMedicalCondition() {
        return medicalCondition;
    }

    public void setMedicalCondition(String medicalCondition) {
        this.medicalCondition = medicalCondition;
    }

    public PatientCategory getCategory() {
        return category;
    }

    public void setCategory(PatientCategory category) {
        this.category = category;
    }

    public void displayDetails() {
        System.out.println("Patient ID: " + patientId);
        System.out.println("Name: " + firstName + " " + lastName);
        System.out.println("Age: " + age);
        System.out.println("Gender: " + gender);
        System.out.println("Medical Condition: " + medicalCondition);
        System.out.println("Category: " + category);
    }
}



