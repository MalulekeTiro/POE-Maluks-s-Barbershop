package com.mycompany.chat_app;

/**
 * Rescue case for an endangered species.
 */
public class EndangeredSpeciesRescue extends RescueCase {

    private final String conservationClassification;
    private final double securityCost;
    private final boolean specialistTeamRequired;

    public EndangeredSpeciesRescue(
            String rescueCaseId,
            String animalName,
            String species,
            String rescueLocation,
            String assignedRanger,
            int numberOfRescueDays,
            double dailyCareCost,
            String conservationClassification,
            double securityCost,
            boolean specialistTeamRequired) {

        super(
                rescueCaseId,
                animalName,
                species,
                rescueLocation,
                assignedRanger,
                numberOfRescueDays,
                dailyCareCost
        );

        this.conservationClassification = conservationClassification;
        this.securityCost = securityCost;
        this.specialistTeamRequired = specialistTeamRequired;
    }

    public String getConservationClassification() {
        return conservationClassification;
    }

    public double getSecurityCost() {
        return securityCost;
    }

    public boolean isSpecialistTeamRequired() {
        return specialistTeamRequired;
    }

    @Override
    public String getRescueType() {
        return "Endangered Species Rescue";
    }

    @Override
    public double calculateTotalRescueCost() {

        double total = calculateBaseCareCost()
                + securityCost;

        if (specialistTeamRequired) {
            total += 8000.00;
        }

        return total;
    }

    @Override
    public String determineRescuePriority() {

        if (specialistTeamRequired
                || conservationClassification.equalsIgnoreCase("Critically Endangered")) {

            return "Critical";
        }

        if (conservationClassification.equalsIgnoreCase("Endangered")) {
            return "High";
        }

        return "Medium";
    }

    @Override
    protected String getTypeSpecificInformation() {

        return String.format(
                "Classification    : %s%n"
                + "Security Cost      : R%,.2f%n"
                + "Specialist Team    : %s",

                conservationClassification,
                securityCost,
                specialistTeamRequired ? "Yes" : "No"
        );
    }
}
