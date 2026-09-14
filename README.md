package com.mycompany.assignment_1;

public class Main {

    public static void main(String[] args) {

        String[] manufacturers = {
            "CANON",
            "SONY",
            "NIKON"
        };

        int[][] prices = {
            {10500, 8500},
            {9500, 7200},
            {12000, 8000}
        };

        int greatestDifference = 0;
        String greatestManufacturer = "";

        System.out.println("==============================================");
        System.out.println("        CAMERA PRICE COMPARISON");
        System.out.println("==============================================");
        System.out.printf("%-15s %-15s %-15s %-15s%n",
                "Manufacturer", "Mirrorless", "DSLR", "Difference");
        System.out.println("--------------------------------------------------------------");

        for (int i = 0; i < manufacturers.length; i++) {

            int mirrorlessPrice = prices[i][0];
            int dslrPrice = prices[i][1];

            int difference = mirrorlessPrice - dslrPrice;

            String stars = "";

            if (difference >= 2500) {
                stars = "***";
            }

            System.out.printf("%-15s R%-14d R%-14d R%d %s%n",
                    manufacturers[i],
                    mirrorlessPrice,
                    dslrPrice,
                    difference,
                    stars);

            if (difference > greatestDifference) {
                greatestDifference = difference;
                greatestManufacturer = manufacturers[i];
            }
        }

        System.out.println("--------------------------------------------------------------");
        System.out.println("Manufacturer with the greatest price difference: "
                + greatestManufacturer);
        System.out.println("Greatest price difference: R" + greatestDifference);
    }
}
