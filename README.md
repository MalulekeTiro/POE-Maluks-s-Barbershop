package com.mycompany.chat_app;

/**
 * Abstract base class containing information common to all rescue cases.
 */
public abstract class RescueCase implements RescueOperations {

    private final String rescueCaseId;
    private final String animalName;
    private final String species;
    private final String rescueLocation;
    private final String assignedRanger;
    private final int numberOfRescueDays;
    private final double dailyCareCost;
    private String currentRescueStatus;

    protected RescueCase(
            String rescueCaseId,
            String animalName,
            String species,
            String rescueLocation,
            String assignedRanger,
            int numberOfRescueDays,
            double dailyCareCost) {

        this.rescueCaseId = rescueCaseId;
        this.animalName = animalName;
        this.species = species;
        this.rescueLocation = rescueLocation;
        this.assignedRanger = assignedRanger;
        this.numberOfRescueDays = numberOfRescueDays;
        this.dailyCareCost = dailyCareCost;

        this.currentRescueStatus = "Registered";
    }

    public String getRescueCaseId() {
        return rescueCaseId;
    }

    public String getAnimalName() {
        return animalName;
    }

    public String getSpecies() {
        return species;
    }

    public String getRescueLocation() {
        return rescueLocation;
    }

    public String getAssignedRanger() {
        return assignedRanger;
    }

    public int getNumberOfRescueDays() {
        return numberOfRescueDays;
    }

    public double getDailyCareCost() {
        return dailyCareCost;
    }

    public String getCurrentRescueStatus() {
        return currentRescueStatus;
    }

    protected void setCurrentRescueStatus(String status) {
        this.currentRescueStatus = status;
    }

    /**
     * Calculates the common daily-care portion of the rescue cost.
     */
    protected double calculateBaseCareCost() {
        return numberOfRescueDays * dailyCareCost;
    }

    public abstract String getRescueType();

    public abstract double calculateTotalRescueCost();

    public abstract String determineRescuePriority();

    /**
     * Returns information specific to each rescue type.
     */
    protected abstract String getTypeSpecificInformation();

    @Override
    public void startRescue() {
        setCurrentRescueStatus("Rescue In Progress");
    }

    @Override
    public void completeRescue() {
        setCurrentRescueStatus("Rescue Completed");
    }

    @Override
    public String generateRescueSummary() {

        return String.format(
                "Case ID: %s%n"
                + "Rescue Type: %s%n"
                + "Species: %s%n"
                + "Assigned Ranger: %s%n"
                + "Rescue Priority: %s%n"
                + "Current Status: %s%n"
                + "Total Rescue Cost: R%,.2f",

                rescueCaseId,
                getRescueType(),
                species,
                assignedRanger,
                determineRescuePriority(),
                currentRescueStatus,
                calculateTotalRescueCost()
        );
    }

    public String getFullReportEntry() {

        return String.format(
                "Case ID       : %s%n"
                + "Animal Name   : %s%n"
                + "Type          : %s%n"
                + "Species       : %s%n"
                + "Location      : %s%n"
                + "Ranger        : %s%n"
                + "Priority      : %s%n"
                + "Status        : %s%n"
                + "Total Cost    : R%,.2f%n"
                + "%s",

                rescueCaseId,
                animalName,
                getRescueType(),
                species,
                rescueLocation,
                assignedRanger,
                determineRescuePriority(),
                currentRescueStatus,
                calculateTotalRescueCost(),
                getTypeSpecificInformation()
        );
    }
}
