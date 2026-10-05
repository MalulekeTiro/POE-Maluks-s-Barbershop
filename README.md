package com.mycompany.chat_app;

/**
 * Utility class containing validation rules required by the assignment.
 */
public final class InputValidator {

    private InputValidator() {
    }

    public static boolean isNotBlank(String value) {

        return value != null
                && !value.trim().isEmpty();
    }

    public static boolean isPositive(int value) {

        return value > 0;
    }

    public static boolean isPositive(double value) {

        return value > 0;
    }

    public static boolean isNonNegative(double value) {

        return value >= 0;
    }
}
