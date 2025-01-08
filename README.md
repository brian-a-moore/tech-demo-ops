# Order Processing System

This application is designed to showcase event-driven architecture using AWS services. It is a simulated payment processing system.

## Technical Requirements

### Scope

1. Event-Driven Microservices:
   • Implement four services to handle order creation, payment processing, inventory update, and shipment preparation.
   • Each service will be a separate AWS Lambda function.
   • Events will be published and consumed via AWS EventBridge.
2. Event Types:
   • OrderCreated: Published when a customer places an order.
   • PaymentProcessed: Published when the payment is processed successfully or fails.
   • InventoryUpdated: Published when the inventory is updated post-payment.
   • ShipmentPrepared: Published when the order is ready for shipping.
3. Event Flow:
   • Order Service:
   • Receives order input (e.g., items, customer info) and publishes OrderCreated event to EventBridge.
   • Payment Service:
   • Listens for OrderCreated event, processes payment, and publishes PaymentProcessed event.
   • Inventory Service:
   • Listens for PaymentProcessed event, updates inventory, and publishes InventoryUpdated event.
   • Shipping Service:
   • Listens for InventoryUpdated event, prepares the shipment, and publishes ShipmentPrepared event.
4. Data Storage:
   • Use DynamoDB for storing information about:
   • Orders (OrdersTable)
   • Payments (PaymentsTable)
   • Inventory (InventoryTable)
   • Shipments (ShipmentsTable)
   • Ensure that each table captures relevant data (e.g., order details, payment status, inventory count, shipment status).
5. AWS EventBridge:
   • Set up an EventBridge Event Bus to handle routing of events between services.
   • Define event patterns for OrderCreated, PaymentProcessed, InventoryUpdated, and ShipmentPrepared events.
   • Configure EventBridge to route events to the appropriate Lambda functions.
6. Authentication and Authorization:
   • Use AWS IAM to define roles and permissions for each Lambda function.
   • Ensure that Lambda functions have the necessary permissions to interact with EventBridge and DynamoDB.
7. Error Handling:
   • Implement error handling in each Lambda function.
   • Use Dead Letter Queues (DLQs) for failed events that cannot be processed.
   • Configure retries for transient errors (e.g., service unavailability).
8. Infrastructure as Code:
   • Use Terraform to define and provision the following resources:
   • EventBridge Event Bus
   • Lambda functions for each service
   • DynamoDB tables
   • IAM roles and permissions
   • Dead Letter Queues for event failures
9. CI/CD:
   • Set up a GitHub Actions pipeline for:
   • Automated testing of Lambda functions.
   • Deploying the infrastructure using Terraform.
   • Deploying the Lambda functions.
10. Testing:
    • Write unit tests for each Lambda function to ensure proper event handling and data manipulation.
    • Test the full event flow by manually triggering the OrderCreated event and tracking the resulting events through the system.
    • Test failure scenarios (e.g., payment failure or inventory issues) and ensure that events are routed to the DLQ.

### Detailed Steps:

1. Event Creation and Consumption:
   • Implement each Lambda function to handle specific tasks (order creation, payment processing, inventory update, shipment preparation).
   • Each service listens for events, processes them, and generates subsequent events.
2. EventBridge Configuration:
   • Set up an EventBridge event bus to route events.
   • Configure event patterns to match event types (OrderCreated, PaymentProcessed, InventoryUpdated, ShipmentPrepared).
   • Ensure that each Lambda function is subscribed to the appropriate events.
3. DynamoDB Setup:
   • Create tables in DynamoDB for storing orders, payments, inventory, and shipments.
   • Ensure that each table is optimized for the expected access patterns (e.g., primary key, secondary indexes).
4. Error Handling and DLQ Setup:
   • Define Dead Letter Queues (DLQs) for handling failed events.
   • Implement retries for failed event processing based on the configuration of the Lambda function.
5. Infrastructure Provisioning:
   • Use Terraform to automate the provisioning of EventBridge, Lambda functions, DynamoDB, and IAM roles.
   • Define Lambda function triggers, permissions, and environment variables in the Terraform configuration.
6. CI/CD Pipeline:
   • Implement a GitHub Actions pipeline to:
   • Test Lambda functions.
   • Deploy infrastructure with Terraform.
   • Deploy Lambda functions.
7. Testing:
   • Write and run unit tests for each Lambda function.
   • Simulate events and check the flow of events through the system.
   • Manually trigger the OrderCreated event to verify that the entire order processing flow works as expected.
   • Test failure scenarios, such as payment processing failure or inventory stock running out, and ensure they are handled correctly.

### Deliverables:

    •	Working AWS Lambda functions for each microservice.
    •	Configured EventBridge event bus and event patterns.
    •	DynamoDB tables with appropriate schema and data.
    •	Infrastructure provisioned via Terraform.
    •	CI/CD pipeline for testing and deployment.
    •	Test cases and documentation of the event flow and error handling process.
