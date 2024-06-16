### Snail Mailbox Status Project Overview

#### Introduction

The Snail Mailbox Status project is designed to monitor and manage the state of a physical mailbox using an ESP32 microcontroller and various AWS services. This project ensures that mailbox events (such as opening or closing) are reliably tracked and notified, adding layers of fault tolerance and alerting to maintain system resilience.

#### Components

1. **ESP32 Microcontroller**:
   - **Hardware**: Utilizes a button sensor to monitor the mailbox door state and an LED to indicate the mailbox state (open or closed).
   - **Wi-Fi Connectivity**: Connects to a Wi-Fi network to send HTTP POST requests indicating the mailbox state (open/closed) to a specified API endpoint.
   - **Code**: Manages Wi-Fi connections, state detection, and HTTP request sending.

2. **AWS Lambda Functions**:
   - **State Management Lambda**: Handles HTTP requests indicating mailbox events (open/closed) and updates the mailbox state using a state machine.
   - **Health Check Lambda**: Verifies if the mailbox state has been updated within a specified threshold and sends SNS notifications for potential issues.

3. **Mailbox State Machine**:
   - **State Management**: Manages mailbox states ('OPEN', 'CLOSED', 'AJAR') based on received events.
   - **AWS Integration**: Uses DynamoDB for event count and timestamp storage, and SNS for sending notifications based on state transitions and conditions.

4. **AWS Services**:
   - **DynamoDB**: Stores the count of 'open' events and timestamps to monitor state transitions.
   - **SNS (Simple Notification Service)**: Sends notifications when the mailbox state changes or when the mailbox remains in the 'AJAR' state for too long.

#### Functionality

1. **ESP32 Monitoring**:
   - **State Detection**: Detects if the mailbox door is open or closed using a button sensor.
   - **State Notification**: Sends HTTP POST requests with the mailbox state to a specified API endpoint.

2. **Lambda State Management**:
   - **HTTP Handling**: Receives state notifications from the ESP32 and updates the state machine.
   - **State Transitions**: Manages transitions between 'OPEN', 'CLOSED', and 'AJAR' states, updating DynamoDB with the current state and event count.
   - **Notification Sending**: Sends SNS notifications based on state changes or prolonged 'AJAR' state.

3. **Health Check**:
   - **Timestamp Verification**: Periodically checks the last event update timestamp in DynamoDB.
   - **Alerting**: Sends SNS notifications if the mailbox state has not been updated within a specified threshold, indicating potential monitoring issues.

#### Summary

The Snail Mailbox Status project integrates an ESP32 microcontroller with AWS services to provide a robust system for monitoring and managing the state of a physical mailbox. The system tracks mailbox events, manages state transitions, and ensures notifications are sent for significant state changes or issues, maintaining high reliability and responsiveness.
