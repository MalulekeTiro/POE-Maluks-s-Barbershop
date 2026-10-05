package com.mycompany.chat_app;

/**
 * Rescue case for an injured animal.
 */
public class InjuredAnimalRescue extends RescueCase {

    private final String injuryDescription;
    private final double veterinaryTreatmentCost;
    private final boolean surgeryRequired;

    public InjuredAnimalRescue(
            String rescueCaseId,
            String animalName,
            String species,
            String rescueLocation,
            String assignedRanger,
            int numberOfRescueDays,
            double dailyCareCost,
            String injuryDescription,
            double veterinaryTreatmentCost,
            boolean surgeryRequired) {

        super(
                rescueCaseId,
                animalName,
                species,
                rescueLocation,
                assignedRanger,
                numberOfRescueDays,
                dailyCareCost
        );

        this.injuryDescription = injuryDescription;
        this.veterinaryTreatmentCost = veterinaryTreatmentCost;
        this.surgeryRequired = surgeryRequired;
    }

    public String getInjuryDescription() {
        return injuryDescription;
    }

    public double getVeterinaryTreatmentCost() {
        return veterinaryTreatmentCost;
    }

    public boolean isSurgeryRequired() {
        return surgeryRequired;
    }

    @Override
    public String getRescueType() {
        return "Injured Animal Rescue";
    }

    @Override
    public double calculateTotalRescueCost() {

        double total = calculateBaseCareCost()
                + veterinaryTreatmentCost;

        if (surgeryRequired) {
            total += 5000.00;
        }

        return total;
    }

    @Override
    public String determineRescuePriority() {

        if (surgeryRequired) {
            return "Critical";
        }

        if (veterinaryTreatmentCost >= 3000) {
            return "High";
        }

        return "Medium";
    }

    @Override
    protected String getTypeSpecificInformation() {

        return String.format(
                "Injury Description: %s%n"
                + "Veterinary Cost   : R%,.2f%n"
                + "Surgery Required  : %s",

                injuryDescription,
                veterinaryTreatmentCost,
                surgeryRequired ? "Yes" : "No"
        );
    }
}
