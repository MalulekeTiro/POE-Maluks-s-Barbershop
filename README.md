package com.mycompany.chat_app;

import static org.junit.jupiter.api.Assertions.*;
import org.junit.jupiter.api.Test;

/**
 * JUnit tests required by the assignment.
 */
public class WildlifeRescueTests {

    @Test
    public void injuredRescueCostWithoutSurgeryIsCorrect() {

        InjuredAnimalRescue rescue =
                new InjuredAnimalRescue(
                        "WR101",
                        "Kito",
                        "African Elephant",
                        "Pretoria",
                        "Ranger A",
                        5,
                        500,
                        "Broken leg",
                        2000,
                        false
                );

        // Base care = 5 x R500 = R2500
        // Total = R2500 + R2000 = R4500

        assertEquals(
                4500.00,
                rescue.calculateTotalRescueCost(),
                0.001
        );
    }

    @Test
    public void injuredRescueCostWithSurgeryIncludesR5000() {

        InjuredAnimalRescue rescue =
                new InjuredAnimalRescue(
                        "WR102",
                        "Temba",
                        "Lion",
                        "Bela-Bela",
                        "Ranger B",
                        4,
                        600,
                        "Severe injury",
                        3000,
                        true
                );

        // Base care = R2400
        // Veterinary = R3000
        // Surgery = R5000
        // Total = R10400

        assertEquals(
                10400.00,
                rescue.calculateTotalRescueCost(),
                0.001
        );
    }

    @Test
    public void orphanedRescueCostWithFosterCareIsCorrect() {

        OrphanedAnimalRescue rescue =
                new OrphanedAnimalRescue(
                        "WR103",
                        "Nala",
                        "Leopard",
                        "Modimolle",
                        "Ranger C",
                        6,
                        400,
                        2,
                        1500,
                        true
                );

        // Base care = R2400
        // Feeding = R1500
        // Foster = R2500
        // Total = R6400

        assertEquals(
                6400.00,
                rescue.calculateTotalRescueCost(),
                0.001
        );
    }

    @Test
    public void endangeredRescueCostWithSpecialistTeamIsCorrect() {

        EndangeredSpeciesRescue rescue =
                new EndangeredSpeciesRescue(
                        "WR104",
                        "Maya",
                        "Black Rhino",
                        "Kruger",
                        "Ranger D",
                        3,
                        700,
                        "Critically Endangered",
                        2000,
                        true
                );

        // Base care = R2100
        // Security = R2000
        // Specialist = R8000
        // Total = R12100

        assertEquals(
                12100.00,
                rescue.calculateTotalRescueCost(),
                0.001
        );
    }

    @Test
    public void rescuePriorityIsCalculatedCorrectly() {

        InjuredAnimalRescue critical =
                new InjuredAnimalRescue(
                        "WR105",
                        "Max",
                        "Elephant",
                        "Pretoria",
                        "Ranger A",
                        2,
                        500,
                        "Emergency injury",
                        1000,
                        true
                );

        OrphanedAnimalRescue high =
                new OrphanedAnimalRescue(
                        "WR106",
                        "Leo",
                        "Lion",
                        "Bela-Bela",
                        "Ranger B",
                        2,
                        500,
                        1,
                        500,
                        false
                );

        EndangeredSpeciesRescue endangered =
                new EndangeredSpeciesRescue(
                        "WR107",
                        "Zuri",
                        "Rhino",
                        "Kruger",
                        "Ranger C",
                        2,
                        500,
                        "Endangered",
                        500,
                        false
                );

        assertEquals(
                "Critical",
                critical.determineRescuePriority()
        );

        assertEquals(
                "High",
                high.determineRescuePriority()
        );

        assertEquals(
                "High",
                endangered.determineRescuePriority()
        );
    }

    @Test
    public void startAndCompleteRescueUpdateStatus() {

        RescueCase rescue =
                new InjuredAnimalRescue(
                        "WR108",
                        "Simba",
                        "Lion",
                        "Polokwane",
                        "Ranger D",
                        2,
                        300,
                        "Minor injury",
                        500,
                        false
                );

        assertEquals(
                "Registered",
                rescue.getCurrentRescueStatus()
        );

        rescue.startRescue();

        assertEquals(
                "Rescue In Progress",
                rescue.getCurrentRescueStatus()
        );

        rescue.completeRescue();

        assertEquals(
                "Rescue Completed",
                rescue.getCurrentRescueStatus()
        );
    }

    @Test
    public void searchFindsExistingRescueCase() {

        RescueCaseManager manager =
                new RescueCaseManager();

        RescueCase rescue =
                new OrphanedAnimalRescue(
                        "WR109",
                        "Luna",
                        "Zebra",
                        "Pretoria",
                        "Ranger E",
                        3,
                        250,
                        4,
                        400,
                        false
                );

        assertTrue(
                manager.addRescueCase(rescue)
        );

        assertNotNull(
                manager.findRescueCase("WR109")
        );

        assertEquals(
                "Luna",
                manager.findRescueCase("WR109")
                        .getAnimalName()
        );
    }

    @Test
    public void duplicateRescueCaseIdsArePrevented() {

        RescueCaseManager manager =
                new RescueCaseManager();

        RescueCase first =
                new InjuredAnimalRescue(
                        "WR110",
                        "A",
                        "Lion",
                        "Pretoria",
                        "Ranger A",
                        1,
                        100,
                        "Cut",
                        100,
                        false
                );

        RescueCase second =
                new OrphanedAnimalRescue(
                        "WR110",
                        "B",
                        "Elephant",
                        "Pretoria",
                        "Ranger B",
                        1,
                        100,
                        2,
                        100,
                        false
                );

        assertTrue(
                manager.addRescueCase(first)
        );

        assertFalse(
                manager.addRescueCase(second)
        );

        assertEquals(
                1,
                manager.getNumberOfRescueCases()
        );
    }

    @Test
    public void reportContainsRequiredInformation() {

        RescueCaseManager manager =
                new RescueCaseManager();

        RescueCase rescue =
                new EndangeredSpeciesRescue(
                        "WR111",
                        "Thabo",
                        "Black Rhino",
                        "Kruger",
                        "Ranger F",
                        2,
                        500,
                        "Critically Endangered",
                        1000,
                        true
                );

        manager.addRescueCase(rescue);

        String report =
                manager.generateReport();

        assertTrue(
                report.contains("WR111")
        );

        assertTrue(
                report.contains(
                        "Endangered Species Rescue"
                )
        );

        assertTrue(
                report.contains(
                        "Black Rhino"
                )
        );

        assertTrue(
                report.contains(
                        "Ranger F"
                )
        );

        assertTrue(
                report.contains(
                        "Critical"
                )
        );

        assertTrue(
                report.contains(
                        "Total Rescue Cases : 1"
                )
        );
    }
}
