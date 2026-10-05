package com.mycompany.chat_app;

/**
 * Rescue case for an orphaned animal.
 */
public class OrphanedAnimalRescue extends RescueCase {

    private final int estimatedAgeMonths;
    private final double feedingCost;
    private final boolean fosterCareRequired;

    public OrphanedAnimalRescue(
            String rescueCaseId,
            String animalName,
            String species,
            String rescueLocation,
            String assignedRanger,
            int numberOfRescueDays,
            double dailyCareCost,
            int estimatedAgeMonths,
            double feedingCost,
            boolean fosterCareRequired) {

        super(
                rescueCaseId,
                animalName,
                species,
                rescueLocation,
                assignedRanger,
                numberOfRescueDays,
                dailyCareCost
        );

        this.estimatedAgeMonths = estimatedAgeMonths;
        this.feedingCost = feedingCost;
        this.fosterCareRequired = fosterCareRequired;
    }

    public int getEstimatedAgeMonths() {
        return estimatedAgeMonths;
    }

    public double getFeedingCost() {
        return feedingCost;
    }

    public boolean isFosterCareRequired() {
        return fosterCareRequired;
    }

    @Override
    public String getRescueType() {
        return "Orphaned Animal Rescue";
    }

    @Override
    public double calculateTotalRescueCost() {

        double total = calculateBaseCareCost()
                + feedingCost;

        if (fosterCareRequired) {
            total += 2500.00;
        }

        return total;
    }

    @Override
    public String determineRescuePriority() {

        if (estimatedAgeMonths <= 3 || fosterCareRequired) {
            return "High";
        }

        if (estimatedAgeMonths <= 12) {
            return "Medium";
        }

        return "Low";
    }

    @Override
    protected String getTypeSpecificInformation() {

        return String.format(
                "Estimated Age     : %d months%n"
                + "Feeding Cost      : R%,.2f%n"
                + "Foster Care       : %s",

                estimatedAgeMonths,
                feedingCost,
                fosterCareRequired ? "Yes" : "No"
        );
    }
}
