/*
 * Click nbfs://nbhost/SystemFileSystem/Templates/Licenses/license-default.txt to change this license
 * Click nbfs://nbhost/SystemFileSystem/Templates/UnitTests/JUnit5TestClass.java to edit this template
 */

import com.mycompany.assignment_1.HospitalException;
import com.mycompany.assignment_1.HospitalSystem;
import com.mycompany.assignment_1.Inpatient;
import com.mycompany.assignment_1.Patient;
import com.mycompany.assignment_1.PatientCategory;
import org.junit.jupiter.api.AfterEach;
import org.junit.jupiter.api.AfterAll;
import org.junit.jupiter.api.Assertions;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.BeforeAll;
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

public class HospitalSystemTest {

    public HospitalSystemTest() {
    }

    @Test
    public void registerPatient_valid() throws HospitalException {
        HospitalSystem hospital = new HospitalSystem();

        Patient patient = new Patient(
                "P001",
                "John",
                "Smith",
                30,
                "Male",
                "Flu",
                PatientCategory.OUTPATIENT
        );

        hospital.registerPatient(patient);

        int expectedValue = 1;
        int actualValue = hospital.getTotalPatients();

        Assertions.assertEquals(expectedValue, actualValue);
    }

    @Test
    public void searchPatient_valid() throws HospitalException {
        HospitalSystem hospital = new HospitalSystem();

        Patient patient = new Patient(
                "P002",
                "Mary",
                "Jones",
                25,
                "Female",
                "Fever",
                PatientCategory.EMERGENCY
        );

        hospital.registerPatient(patient);

        Patient actualValue = hospital.findPatient("P002");

        Assertions.assertNotNull(actualValue);
        Assertions.assertEquals("Mary", actualValue.getFirstName());
    }

    @Test
    public void searchPatient_invalid() throws HospitalException {
        HospitalSystem hospital = new HospitalSystem();

        Patient actualValue = hospital.findPatient("P999");

        Assertions.assertNull(actualValue);
    }

    @Test
    public void updatePatient_valid() throws HospitalException {
        HospitalSystem hospital = new HospitalSystem();

        Patient patient = new Patient(
                "P003",
                "Peter",
                "Brown",
                40,
                "Male",
                "Cold",
                PatientCategory.OUTPATIENT
        );

        hospital.registerPatient(patient);

        hospital.updatePatient(
                "P003",
                "Peter",
                "Williams",
                41,
                "Male",
                "Flu"
        );

        Patient updatedPatient = hospital.findPatient("P003");

        Assertions.assertEquals("Williams", updatedPatient.getLastName());
        Assertions.assertEquals(41, updatedPatient.getAge());
        Assertions.assertEquals("Flu", updatedPatient.getMedicalCondition());
    }

    @Test
    public void deletePatient_valid() throws HospitalException {
        HospitalSystem hospital = new HospitalSystem();

        Patient patient = new Patient(
                "P004",
                "Sarah",
                "Miller",
                35,
                "Female",
                "Headache",
                PatientCategory.OUTPATIENT
        );

        hospital.registerPatient(patient);
        hospital.deletePatient("P004");

        Assertions.assertNull(hospital.findPatient("P004"));
        Assertions.assertEquals(0, hospital.getTotalPatients());
    }

    @Test
    public void allocateBed_valid() throws HospitalException {
        HospitalSystem hospital = new HospitalSystem();

        Inpatient patient = new Inpatient(
                "P005",
                "David",
                "Wilson",
                50,
                "Male",
                "Pneumonia",
                1,
                "Not Allocated"
        );

        hospital.registerPatient(patient);
        hospital.allocateBed("P005");

        Assertions.assertEquals(1, hospital.getOccupiedBeds());
        Assertions.assertNotEquals("Not Allocated", patient.getBedNumber());
    }

    @Test
    public void releaseBed_valid() throws HospitalException {
        HospitalSystem hospital = new HospitalSystem();

        Inpatient patient = new Inpatient(
                "P006",
                "James",
                "Taylor",
                45,
                "Male",
                "Infection",
                1,
                "Not Allocated"
        );

        hospital.registerPatient(patient);
        hospital.allocateBed("P006");

        String bedNumber = patient.getBedNumber();

        hospital.releaseBed(bedNumber);

        Assertions.assertEquals(0, hospital.getOccupiedBeds());
        Assertions.assertEquals("Not Allocated", patient.getBedNumber());
    }

    @Test
    public void duplicatePatientId_invalid() throws HospitalException {
        HospitalSystem hospital = new HospitalSystem();

        Patient patient1 = new Patient(
                "P007",
                "John",
                "Smith",
                30,
                "Male",
                "Flu",
                PatientCategory.OUTPATIENT
        );

        Patient patient2 = new Patient(
                "P007",
                "Jane",
                "Smith",
                25,
                "Female",
                "Fever",
                PatientCategory.EMERGENCY
        );

        hospital.registerPatient(patient1);

        Assertions.assertThrows(
                HospitalException.class,
                () -> hospital.registerPatient(patient2)
        );
    }

    @Test
    public void allocateOccupiedBed_invalid() throws HospitalException {
        HospitalSystem hospital = new HospitalSystem();

        Inpatient patient1 = new Inpatient(
                "P008",
                "Alex",
                "Brown",
                30,
                "Male",
                "Flu",
                1,
                "Not Allocated"
        );

        Inpatient patient2 = new Inpatient(
                "P009",
                "Lisa",
                "Brown",
                28,
                "Female",
                "Fever",
                1,
                "Not Allocated"
        );

        hospital.registerPatient(patient1);
        hospital.registerPatient(patient2);

        hospital.allocateBed("P008");

        String occupiedBed = patient1.getBedNumber();

        Assertions.assertThrows(
                HospitalException.class,
                () -> {
                    hospital.releaseBed(occupiedBed + "X");
                }
        );

        Assertions.assertEquals(1, hospital.getOccupiedBeds());
    }

    @Test
    public void outpatientBedAllocation_invalid() throws HospitalException {
        HospitalSystem hospital = new HospitalSystem();

        Patient patient = new Patient(
                "P010",
                "Tom",
                "Green",
                35,
                "Male",
                "Flu",
                PatientCategory.OUTPATIENT
        );

        hospital.registerPatient(patient);

        Assertions.assertThrows(
                HospitalException.class,
                () -> hospital.allocateBed("P010")
        );
    }

    @Test
    public void allBedsOccupied_invalid() throws HospitalException {
        HospitalSystem hospital = new HospitalSystem();

        for (int i = 1; i <= 20; i++) {
            Inpatient patient = new Inpatient(
                    "P" + String.format("%03d", i),
                    "First" + i,
                    "Last" + i,
                    30,
                    "Male",
                    "Condition",
                    1,
                    "Not Allocated"
            );

            hospital.registerPatient(patient);
            hospital.allocateBed(patient.getPatientId());
        }

        Inpatient extraPatient = new Inpatient(
                "P021",
                "Extra",
                "Patient",
                30,
                "Male",
                "Condition",
                1,
                "Not Allocated"
        );

        hospital.registerPatient(extraPatient);

        Assertions.assertThrows(
                HospitalException.class,
                () -> hospital.allocateBed("P021")
        );

        Assertions.assertEquals(20, hospital.getOccupiedBeds());
    }

    @Test
    public void sortPatientsById_valid() throws HospitalException {
        HospitalSystem hospital = new HospitalSystem();

        Patient patient1 = new Patient(
                "P003",
                "John",
                "Smith",
                30,
                "Male",
                "Flu",
                PatientCategory.OUTPATIENT
        );

        Patient patient2 = new Patient(
                "P001",
                "Mary",
                "Jones",
                25,
                "Female",
                "Fever",
                PatientCategory.EMERGENCY
        );

        hospital.registerPatient(patient1);
        hospital.registerPatient(patient2);

        hospital.sortPatientsById();

        Assertions.assertEquals("P001", hospital.findPatient("P001").getPatientId());
    }
}

