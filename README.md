Component Focus: Appointment Management Module
This module encompasses the core Appointment.java business entity and its dedicated verification framework. It ensures all reservation metadata is thoroughly validated before entering the application lifecycle.

1. The Core Domain Class (Appointment.java)
The Appointment.java class encapsulates single reservation records and acts as the system's first line of defense against corrupted data. It implements strict validation rules using standard java.time tracking features:

  • Identity Guard (apptId): Automatically monitors the registration ID. It ensures keys are non-null and strictly restricted to a maximum length of 10 characters.

  • Time-Zone Awareness (apptDate): Utilizes ZonedDateTime variables calibrated against the host machine's execution clock. It explicitly restricts historic, back-dated timelines to prevent setting appointments in the past.

  • Description Limits (apptDescription): Enforces size boundaries on descriptive text blocks, blocking notes that exceed 50 characters or register as empty/null.

2. Object Lifecycle Routing (Constructors)
To handle flexible application state creation, the class processes initialization requests through four distinct setup channels:

  • Default Channel: Assigns a boilerplate tracking ID ("INITIAL"), logs the instant system execution time, and attaches a generic placeholder description string.

  • ID-Only Channel: Sets up an appointment state containing only a verified unique identifier.

  • ID & Date Channel: Configures a forward-dated appointment without descriptive notes, enforcing past-date verification triggers.

  • Full Channel: Instantiates a complete, production-ready appointment by verifying identity parameters, target execution dates, and descriptive texts simultaneously.

3. The Validation Framework (AppointmentTest.java)
The system relies on a comprehensive JUnit 5 unit testing layer to rigorously verify that the domain class behaves correctly. Instead of checking simple paths, the suite maps directly to the entity's input boundaries:

• Happy-Path Construction: Validates successful object instantiation across all four constructor methods (Default, ID-only, ID+Date, and Full) when parameters fall within normal limits.

• Temporal Gatekeeping: Asserts that scheduling an appointment even one day in the past (minusDays(1)) triggers an immediate safety shutdown.

• Data Size Breaches: Verifies that passing an identification string longer than 10 characters, or a descriptive field exceeding 50 characters, securely interrupts execution.

• Exception Integrity: Guarantees that every failed boundary parameter accurately fires a uniform IllegalArgumentException back to the calling service.

------------------------------------------------------------------------------------------------------------------------------------------------

Core System Components & Test Matrix:
To keep the codebase modular, the system is split into three core business domains. Each domain contains a strict data-validation entity and a companion lifecycle service, fully mapped to a dedicated JUnit 5 test suite.

Appointment Management:
This featured example demonstrates the standard validation patterns used across all modules in this repository:
The Domain Model (Appointment.java): Encapsulates single reservation records. It uses ZonedDateTime to manage timezone tracking and features four flexible constructors (Default, ID-only, ID+Date, and Full) to handle multi-stage business data.

The Testing Engine (AppointmentTest.java): A JUnit 5 suite targeting input boundaries. It asserts that happy paths succeed, validates temporal gates (rejects minusDays(1)), and guarantees that breaches immediately trigger a safe IllegalArgumentException.

Comprehensive Component Map: The remaining entities follow the exact same high-integrity architecture shown above and are mapped below:

Domain / Entity        Core Business & Data Rules                                  Companion Test Suites 
---------------------------------------------------------------------------------------------------------------------
AppointmentService -   Coordinates in-memory storage, updates, 
                       and deletion workflows for active appointments.             
                       - AppointmentServiceTest.java
                       
Contact -              Enforces user profile layout rules                          
                       (First Name $\le$ 10, Last Name $\le$ 10, 
                       Phone = 10 digits, Address $\le$ 30). ContactTest.java -              
                       
ContactService -       Manages unique contacts directory, preventing              
                       duplicate keys and ensuring safe record updates.
                       -  ContactServiceTest.java
                       
Task -                 Controls task tracking parameters. Enforces tight           
                       boundaries on task names ($\le$ 20) and descriptions 
                       ($\le$ 50). - TaskTest.java                               
                       
TaskService -          Drives active operations to add, remove, and                
                       safely modify system task properties. -  TaskServiceTest.java  
