# Supply Chain Visibility and Tracking Platform

A Java-based desktop application designed to centralize supply chain operations, monitor shipment progress, manage inventory, maintain supplier and warehouse records, track vehicles, and identify operational exceptions through a unified graphical interface.

## Overview

Supply chains involve multiple interconnected activities, including procurement, inventory management, warehousing, transportation, and shipment delivery. When information about these activities is maintained separately, monitoring operational status and identifying exceptions becomes difficult.

The **Supply Chain Visibility and Tracking Platform** aims to address this challenge through a centralized application that brings essential supply chain operations into one interface.

The application is developed using Java Swing and demonstrates core supply chain workflows through interactive forms, data tables, shipment status updates, tracking events, inventory checks, warehouse utilization calculations, alerts, and operational reports.

The current version is a functional GUI prototype that stores demonstration data in memory. MySQL database integration through JDBC is part of the planned development toward a persistent, layered application.

## Project Objectives

- Centralize shipment, inventory, supplier, warehouse, and vehicle information.
- Provide a dashboard for viewing important operational statistics.
- Support shipment creation, status updates, deletion, and searching.
- Maintain shipment event history for traceability.
- Identify inventory items below their reorder levels.
- Calculate warehouse capacity utilization.
- Manage vehicle information and operational status.
- Display operational alerts and support alert resolution.
- Generate summary reports for supply chain operations.
- Establish a foundation for database-backed persistence and a layered software architecture.

## Key Features

### 1. Authentication

A login screen provides access to the application using demonstration credentials.

- Username and password fields.
- Basic credential validation.
- Navigation to the main application after successful login.
- Error message for invalid credentials.

**Current implementation:** The credentials are hardcoded for demonstration. This is not production-grade authentication.

### 2. Dashboard

The dashboard presents an overview of supply chain activity.

- Total number of shipments.
- Shipments currently in transit.
- Delivered and delayed shipments.
- Inventory items below their reorder levels.
- Vehicle count.
- Recent shipment information in a tabular view.

The statistics are calculated from the application's in-memory collections.

### 3. Shipment Management

The shipment management module provides basic operations for maintaining shipment records.

- Add a new shipment.
- View shipment details.
- Update a shipment's status.
- Delete a shipment.
- Search shipments using identifying or descriptive information.

Each shipment contains attributes such as shipment ID, origin, destination, product, vehicle, expected delivery date, and status.

Supported shipment statuses:

- `CREATED`
- `PACKED`
- `DISPATCHED`
- `IN_TRANSIT`
- `ARRIVED`
- `DELIVERED`
- `DELAYED`
- `CANCELLED`

### 4. Shipment Tracking

The tracking module provides shipment-level visibility into recorded events.

- Search for a shipment by ID.
- Retrieve the shipment's current status.
- Display recorded tracking events.
- Show event timestamps, locations, and statuses.
- Record a new event when the shipment status is updated through the shipment management module.

A shipment may have multiple tracking events, allowing the application to represent its operational history rather than only its current state.

### 5. Inventory Management

The inventory module supports basic inventory monitoring.

- Add inventory records.
- View product and warehouse information.
- Update available quantities.
- Delete inventory records.
- Display reorder levels.
- Identify low-stock items.

**Low-stock rule:**

An inventory item is classified as low stock when:

`Available Quantity < Reorder Level`

This rule is used to determine the stock status displayed in the inventory table and the low-stock count shown on the dashboard.

### 6. Supplier Management

The supplier module maintains basic supplier information.

- Add supplier records.
- View supplier details.
- Delete supplier records.

Supplier records include supplier ID, name, contact information, email, and associated product information.

### 7. Warehouse Management

The warehouse module maintains warehouse information and provides a capacity utilization calculation.

- Add warehouse records.
- View warehouse ID, name, and location.
- Record total warehouse capacity.
- Record occupied capacity.
- Calculate utilization as a percentage.

**Warehouse utilization formula:**

`Utilization (%) = (Occupied Capacity / Total Capacity) × 100`

The calculation returns zero when the total capacity is zero.

### 8. Vehicle and Transport Management

The vehicle module provides basic vehicle record management.

- Add vehicle information.
- View vehicle number, type, driver, and status.
- Update the vehicle's operational status.

The demonstration supports statuses such as Available, In Transit, Maintenance, and Delayed.

### 9. Alerts and Exceptions

The alerts module displays predefined operational alerts and allows them to be marked as resolved.

Examples include:

- Shipment delays.
- Inventory below reorder level.
- Warehouses approaching capacity.
- Vehicles requiring maintenance.

Each alert contains an identifier, type, description, severity, and resolution state.

**Current implementation:** Alerts are initialized as demonstration data. The application supports marking alerts as resolved, but a complete automated alert-generation engine is not yet implemented.

### 10. Reports

The reports module generates a text-based summary of operational data.

The report includes:

- Total shipments.
- In-transit shipments.
- Delivered shipments.
- Delayed shipments.
- Total inventory records.
- Low-stock inventory count.
- Number of warehouses and their utilization.
- Total vehicles.
- Number of unresolved alerts.

The report is generated from the current in-memory data.

---

## Technology Stack

| Technology | Purpose |
|---|---|
| Java | Core programming language and application logic |
| Java Swing | Desktop graphical user interface |
| AWT | Layout managers, fonts, colors, and GUI support |
| Java Collections Framework | In-memory storage using lists and `ArrayList` |
| JTable | Displaying operational data in tables |
| DefaultTableModel | Managing table data displayed by Swing |
| JOptionPane | Login feedback, forms, and dialog interactions |
| IntelliJ IDEA | Development environment |
| Maven | Optional/project build management, if configured in the repository |
| JDBC | Planned database connectivity |
| MySQL | Planned persistent relational database |

The current prototype primarily uses Java's standard libraries and does not require an external database to run.

## System Architecture

### Current Prototype Architecture

The current implementation uses a simplified structure:

```text
                 USER
                   |
                   v
             Java Swing GUI
                   |
                   v
           Application Methods
                   |
                   v
          Java Domain Objects
                   |
                   v
         ArrayList Collections
             (In Memory)
```

The GUI invokes application methods through event listeners. These methods manipulate Java objects and update the corresponding tables.

Since data is stored in memory, changes are lost when the application terminates.

### Planned Layered Architecture

The intended database-backed architecture is:

```text
          Presentation Layer
             Java Swing
                  |
                  v
             Service Layer
       Validation and Business Rules
                  |
                  v
               DAO Layer
         Database Access Operations
                  |
                  v
                 JDBC
                  |
                  v
             MySQL Database
```

The responsibilities of the proposed layers are:

**Presentation Layer**
- Displays screens, forms, tables, and dialogs.
- Collects user input.
- Presents operation results and validation messages.

**Service Layer**
- Implements business rules.
- Validates operations before they are executed.
- Coordinates interactions between the GUI and data access layer.

**DAO Layer**
- Encapsulates database operations.
- Executes CRUD queries.
- Maps database records to Java domain objects.

**JDBC**
- Establishes connections to MySQL.
- Executes SQL statements.
- Retrieves query results.

**MySQL**
- Stores application records persistently.
- Maintains relationships between shipments, events, inventory, suppliers, warehouses, vehicles, and alerts.

The layered architecture represents the intended target design; it should not be interpreted as fully implemented in the current prototype.

---

## Data Model

The current application uses Java classes to represent the main entities.

### Shipment

Represents a shipment moving between an origin and destination.

Typical attributes:
- Shipment ID
- Origin
- Destination
- Product
- Vehicle
- Expected delivery date
- Shipment status

### Event

Represents an event recorded for a shipment.

Typical attributes:
- Shipment ID
- Event date and time
- Location
- Status

**Relationship:** One shipment can have multiple tracking events.

### Inventory

Represents a product's stock information at a warehouse.

Typical attributes:
- Product ID
- Product name
- Warehouse
- Available quantity
- Reorder level

### Supplier

Represents a supplier associated with products or materials.

### Warehouse

Represents a storage facility and its capacity information.

### Vehicle

Represents a transport vehicle and its operational status.

### Alert

Represents an operational exception, its severity, and its resolution state.

These are Java domain classes in the current prototype. A corresponding relational schema will be required when persistent database integration is implemented.

---

## Project Structure

The current prototype is organized as a single Java source file containing nested data classes and GUI panels.

```text
SupplyChainVisibility/
├── src/
│   └── main/
│       └── java/
│           └── com/
│               └── supplychain/
│                   └── Main.java
├── pom.xml                 # If Maven is configured
└── README.md
```

The main source file contains the login screen, main application window, navigation, dashboard, operational modules, data models, demonstration data, and helper methods.

A future refactoring can separate these responsibilities into packages:

```text
com.supplychain
├── Main.java
├── model/
├── enums/
├── dao/
├── service/
├── gui/
├── panels/
├── dialogs/
├── components/
├── config/
└── util/
```

This separation would improve maintainability, testability, and integration with the database layer.

---

## Getting Started

### Prerequisites

- Java Development Kit (JDK), preferably JDK 17 or later.
- IntelliJ IDEA or another Java-compatible IDE.
- Git, if cloning the repository.

The current Swing prototype does not require MySQL or a JDBC driver to run.

### 1. Clone the repository

Replace `YOUR_USERNAME` with your GitHub username and `YOUR_REPOSITORY` with your repository name.

```bash
git clone https://github.com/YOUR_USERNAME/YOUR_REPOSITORY.git
```

Navigate into the project directory:

```bash
cd YOUR_REPOSITORY
```

### 2. Open the project

1. Launch IntelliJ IDEA.
2. Select **Open**.
3. Choose the cloned project directory.
4. Allow IntelliJ to index the project.
5. Ensure a compatible JDK is configured.

### 3. Run the application

Open:

```text
src/main/java/com/supplychain/Main.java
```

Run the `main()` method from the IDE.

If compiling manually, navigate to the directory containing `src` and execute:

```bash
javac -d out src/main/java/com/supplychain/Main.java
```

Then run:

```bash
java -cp out com.supplychain.Main
```

These commands assume the source file follows the package structure shown above and a compatible JDK is available on the command line.

### 4. Demo login

Use the following demonstration credentials:

| Field | Value |
|---|---|
| Username | `admin` |
| Password | `admin123` |

These credentials are hardcoded in the prototype and are intended only for demonstration.

---

## How to Use the Application

1. Launch the application.
2. Enter the demo credentials.
3. Review the dashboard and its operational statistics.
4. Open Shipment Management to add, search, update, or delete shipment records.
5. Open Shipment Tracking and search for a shipment ID, such as `SHP-1023`.
6. Open Inventory Management to inspect stock levels and update quantities.
7. Use Supplier Management to maintain supplier records.
8. Open Warehouse Management to review capacity utilization.
9. Use Vehicle Management to add vehicles and update statuses.
10. Open Alerts and Exceptions to inspect and resolve demonstration alerts.
11. Open Reports to generate a summary of the current application data.

### Demonstration Data

The application initializes sample records for shipments, tracking events, inventory, suppliers, warehouses, vehicles, and alerts.

For example, a shipment with ID `SHP-1023` can be used to demonstrate shipment tracking and status updates.

**Note:** These records are in-memory demonstration data. They are reinitialized when the application is restarted.

---

## Testing and Validation

The current prototype can be manually tested using the following scenarios.

| Test scenario | Expected result |
|---|---|
| Valid demo login | Main application opens |
| Invalid login | Error message appears |
| Add shipment | New shipment appears in the table |
| Search shipment | Matching records are displayed |
| Update shipment status | Current status changes |
| Update shipment status | A corresponding tracking event is added |
| Search tracking ID | Matching shipment history is displayed |
| Update inventory quantity | Inventory table reflects the new quantity |
| Low-stock condition | Stock status displays `LOW STOCK` |
| Add warehouse | New warehouse appears with calculated utilization |
| Update vehicle status | Vehicle table displays the selected status |
| Resolve alert | Alert status changes to Resolved |
| Generate report | Summary reflects current in-memory records |

These scenarios describe test cases for the current prototype. They are not a claim that automated unit or integration tests have been implemented.

---

## Current Limitations

The current version is a demonstration prototype. Its known limitations include:

1. **In-memory storage:** Application data is lost on restart.
2. **No JDBC integration:** Database connectivity has not yet been implemented in the prototype.
3. **No MySQL persistence:** Records are not saved in a relational database.
4. **Hardcoded authentication:** Login credentials are embedded in the source code.
5. **Limited validation:** Additional checks are needed for duplicate IDs, missing fields, invalid dates, negative quantities, and invalid capacity values.
6. **Predefined alerts:** Alerts are initialized as sample data rather than being generated by a comprehensive automated monitoring engine.
7. **Limited reporting:** Reports are displayed as text and are not exported to PDF, Excel, or other formats.
8. **Single-file structure:** Data models, application logic, and GUI code are currently combined in one file.
9. **Limited exception handling:** Production-grade logging and centralized error handling are not yet implemented.
10. **No live tracking feed:** Shipment locations and events are demonstration records, not live GPS or carrier data.

These limitations define the next development priorities.

---

## Future Enhancements

### Database and Architecture
- Integrate MySQL using JDBC.
- Implement DAO classes for database operations.
- Introduce a service layer for business rules and validation.
- Separate domain models, GUI panels, and database configuration.
- Persist shipment events and operational records across application restarts.

### Security
- Replace hardcoded credentials with database-backed authentication.
- Store passwords using an appropriate salted password-hashing mechanism.
- Introduce role-based access control.
- Use prepared statements for parameterized database queries.

### Operational Intelligence
- Generate alerts automatically when business rules are violated.
- Detect overdue shipments using expected delivery dates.
- Improve inventory replenishment monitoring.
- Identify warehouses approaching their capacity limits.
- Add more comprehensive operational metrics and reporting.

### User Experience
- Improve form validation and error messages.
- Add editing capabilities for supplier and warehouse records.
- Add more advanced search and filtering.
- Support report export.
- Improve the navigation and usability of the desktop interface.

### Advanced Capabilities
- Integrate external shipment or GPS feeds where available.
- Explore ETA estimation and explainable shipment risk scoring.
- Add notification mechanisms for important operational exceptions.

These are potential enhancements and should not be interpreted as current features.

---

## Learning Outcomes

This project provides practical experience with:

- Object-oriented programming in Java.
- Building desktop applications using Java Swing.
- Event-driven programming and action listeners.
- Layout managers and reusable GUI components.
- Collections and domain modeling.
- CRUD workflows.
- Table-based data presentation.
- Basic business-rule implementation.
- Shipment event traceability.
- Inventory and warehouse calculations.
- Software architecture and database integration planning.

---

## Project Status

**Current status:** Functional Java Swing prototype with in-memory demonstration data.

| Component | Status |
|---|---|
| Login interface | Implemented |
| Dashboard | Implemented |
| Shipment management | Implemented |
| Shipment tracking | Implemented |
| Inventory management | Implemented |
| Supplier management | Implemented |
| Warehouse management | Implemented |
| Vehicle management | Implemented |
| Alerts and exceptions interface | Implemented |
| Text-based reports | Implemented |
| Persistent MySQL storage | Planned |
| JDBC integration | Planned |
| DAO and service layers | Planned |
| Automated testing suite | Not yet established |

The status describes the known prototype implementation. Database persistence and the planned layered architecture require additional development.

---

## Contributing

Contributions and suggestions are welcome.

For proposed improvements:

1. Fork the repository.
2. Create a feature branch.
3. Implement and test the change.
4. Submit a pull request describing the changes.

For academic or collaborative development, contributors should coordinate changes to the shared Java source file to reduce merge conflicts.

## License

No license has been specified yet. Unless a license is added to the repository, others should not assume that the code is available for unrestricted reuse or redistribution.

## Acknowledgements

Developed as an academic Java project to explore supply chain visibility, shipment traceability, inventory monitoring, and operational management through a desktop application.
