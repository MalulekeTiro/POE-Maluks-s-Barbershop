package com.mycompany.chat_app;

/**
 * Defines the operations that every wildlife rescue case must support.
 */
public interface RescueOperations {

    void startRescue();

    void completeRescue();

    String generateRescueSummary();
}
