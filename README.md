/*
 * Click nbfs://nbhost/SystemFileSystem/Templates/Licenses/license-default.txt to change this license
 * Click nbfs://nbhost/SystemFileSystem/Templates/Classes/Class.java to edit this template
 */
package com.mycompany.assingment_1;

/**
 *
 * @author emeris
 */
public class HospitalException extends Exception {
    public HospitalException(String message) {
        super(message);
    }
}






package com.mycompany.assingment_1;

import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.*;

public class HospitalSystemTest {

    @Test
    public void testRegisterPatient() throws HospitalException {
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

        assertEquals(1, hospital.getTotalPatients());
    }

    @Test
    public void testSearchPatient() throws HospitalException {
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

        Patient result = hospital.findPatient("P002");

        assertNotNull(result);
        assertEquals("Mary", result.getFirstName());
    }

    @Test
    public void testSearchPatientNotFound() throws HospitalException {
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

        assertNull(hospital.findPatient("P999"));
    }

    @Test
    public void testUpdatePatient() throws HospitalException {
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

        Patient updated = hospital.findPatient("P003");

        assertEquals("Williams", updated.getLastName());
        assertEquals(41, updated.getAge());
        assertEquals("Flu", updated.getMedicalCondition());
    }

    @Test
    public void testUpdateNonExistentPatientThrows() {
        HospitalSystem hospital = new HospitalSystem();

        assertThrows(
                HospitalException.class,
                () -> hospital.updatePatient("P999", "Ghost", "Patient", 30, "Male", "None")
        );
    }

    @Test
    public void testDeletePatient() throws HospitalException {
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

        assertEquals(0, hospital.getTotalPatients());
        assertNull(hospital.findPatient("P004"));
    }

    @Test
    public void testDeleteInpatientAlsoReleasesBed() throws HospitalException {
        HospitalSystem hospital = new HospitalSystem();

        Inpatient patient = new Inpatient(
                "P004B",
                "Sarah",
                "Miller",
                35,
                "Female",
                "Headache",
                1,
                "Not Allocated"
        );

        hospital.registerPatient(patient);
        hospital.allocateBed("P004B");

        assertEquals(1, hospital.getOccupiedBeds());

        hospital.deletePatient("P004B");

        assertEquals(0, hospital.getTotalPatients());
        assertEquals(0, hospital.getOccupiedBeds());
    }

    @Test
    public void testAllocateBed() throws HospitalException {
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

        assertEquals(1, hospital.getOccupiedBeds());
        assertNotEquals("Not Allocated", patient.getBedNumber());
    }

    @Test
    public void testReleaseBed() throws HospitalException {
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

        String bed = patient.getBedNumber();

        hospital.releaseBed(bed);

        assertEquals(0, hospital.getOccupiedBeds());
        assertEquals("Not Allocated", patient.getBedNumber());
    }

    @Test
    public void testReleaseInvalidBedThrows() throws HospitalException {
        HospitalSystem hospital = new HospitalSystem();

        assertThrows(
                HospitalException.class,
                () -> hospital.releaseBed("B99")
        );
    }

    @Test
    public void testDuplicatePatientId() throws HospitalException {
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

        assertThrows(
                HospitalException.class,
                () -> hospital.registerPatient(patient2)
        );
    }

    @Test
    public void testPreventAllocatingBedToPatientThatAlreadyHasOne() throws HospitalException {
        HospitalSystem hospital = new HospitalSystem();

        Inpatient patient = new Inpatient(
                "P008",
                "Alex",
                "Brown",
                30,
                "Male",
                "Flu",
                1,
                "Not Allocated"
        );

        hospital.registerPatient(patient);
        hospital.allocateBed("P008");

        assertThrows(
                HospitalException.class,
                () -> hospital.allocateBed("P008")
        );

        assertEquals(1, hospital.getOccupiedBeds());
    }

    @Test
    public void testOnlyInpatientCanGetBed() throws HospitalException {
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

        assertThrows(
                HospitalException.class,
                () -> hospital.allocateBed("P010")
        );
    }

    @Test
    public void testAllBedsOccupied() throws HospitalException {
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

        assertEquals(20, hospital.getOccupiedBeds());

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

        assertThrows(
                HospitalException.class,
                () -> hospital.allocateBed("P021")
        );
    }

    @Test
    public void testSortPatientsById() throws HospitalException {
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

        assertEquals(2, hospital.getTotalPatients());
        assertEquals("P001", hospital.findPatient("P001").getPatientId());
    }
}







<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.mycompany</groupId>
    <artifactId>Assingment_1</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>jar</packaging>
    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.release>26</maven.compiler.release>
        <exec.mainClass>com.mycompany.assingment_1.Assingment_1</exec.mainClass>
        <junit.version>5.10.2</junit.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.junit.jupiter</groupId>
            <artifactId>junit-jupiter</artifactId>
            <version>${junit.version}</version>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-surefire-plugin</artifactId>
                <version>3.2.5</version>
            </plugin>
        </plugins>
    </build>
</project>
