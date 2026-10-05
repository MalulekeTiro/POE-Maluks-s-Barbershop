package com.mycompany.chat_app;

import java.util.ArrayList;

/**
 * Handles the collection of rescue cases.
 */
public class RescueCaseManager {

    private final ArrayList<RescueCase> rescueCases;

    public RescueCaseManager() {
        rescueCases = new ArrayList<>();
    }

    /**
     * Adds a rescue case if the case ID is unique.
     */
    public boolean addRescueCase(RescueCase rescueCase) {

        if (rescueCase == null) {
            return false;
        }

        if (findRescueCase(rescueCase.getRescueCaseId()) != null) {
            return false;
        }

        rescueCases.add(rescueCase);

        return true;
    }

    /**
     * Searches for a rescue case using the Rescue Case ID.
     */
    public RescueCase findRescueCase(String rescueCaseId) {

        if (rescueCaseId == null) {
            return null;
        }

        for (RescueCase rescueCase : rescueCases) {

            if (rescueCase.getRescueCaseId()
                    .equalsIgnoreCase(rescueCaseId.trim())) {

                return rescueCase;
            }
        }

        return null;
    }

    /**
     * Updates the status of an existing rescue case.
     */
    public boolean updateRescueStatus(
            String rescueCaseId,
            String newStatus) {

        RescueCase rescueCase = findRescueCase(rescueCaseId);

        if (rescueCase == null || newStatus == null) {
            return false;
        }

        String status = newStatus.trim().toLowerCase();

        switch (status) {

            case "start":
            case "in progress":
            case "rescue in progress":

                rescueCase.startRescue();
                return true;

            case "complete":
            case "completed":
            case "rescue completed":

                rescueCase.completeRescue();
                return true;

            case "registered":

                rescueCase.setCurrentRescueStatus("Registered");
                return true;

            case "under observation":

                rescueCase.setCurrentRescueStatus(
                        "Under Observation"
                );

                return true;

            default:
                return false;
        }
    }

    /**
     * Returns a copy of the rescue case list.
     */
    public ArrayList<RescueCase> getRescueCases() {
        return new ArrayList<>(rescueCases);
    }

    public int getNumberOfRescueCases() {
        return rescueCases.size();
    }

    /**
     * Calculates the total estimated cost of all cases.
     */
    public double calculateTotalEstimatedRescueCost() {

        double total = 0.0;

        for (RescueCase rescueCase : rescueCases) {
            total += rescueCase.calculateTotalRescueCost();
        }

        return total;
    }

    /**
     * Generates the required rescue report.
     */
    public String generateReport() {

        StringBuilder report = new StringBuilder();

        report.append("====================================================\n");
        report.append("              WILDLIFE RESCUE REPORT\n");
        report.append("====================================================\n\n");

        if (rescueCases.isEmpty()) {

            report.append(
                    "No rescue cases are currently stored.\n"
            );

        } else {

            for (RescueCase rescueCase : rescueCases) {

                report.append(
                        rescueCase.getFullReportEntry()
                );

                report.append(
                        "\n----------------------------------------------------\n\n"
                );
            }
        }

        report.append(
                String.format(
                        "Total Rescue Cases : %d%n",
                        rescueCases.size()
                )
        );

        report.append(
                String.format(
                        "Total Rescue Cost  : R%,.2f%n",
                        calculateTotalEstimatedRescueCost()
                )
        );

        return report.toString();
    }
}
