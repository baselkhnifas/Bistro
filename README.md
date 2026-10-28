# Bistro
Restaurant Management System
A desktop restaurant-management application built in Java with JavaFX, MySQL, and a client–server architecture using OCSF.
Project status: Academic/team project. The application has been tested locally on Windows with Java 21, JavaFX 21, and a MySQL database. It is not intended for production use without additional security and deployment work.

Features
- Guests and members: Book tables, view available time slots, cancel reservations, and join or leave the waiting list.
- Members: Sign in, update contact information, view reservation history, and access a digital membership card.
- Self-service terminal: Identify customers, retrieve reservations, handle waiting-list operations, and simulate payments.
- Restaurant staff: View active orders and diners, manage customers, and monitor the waiting list.
- Management: Maintain tables and opening hours, and view operational reports.
- Reservation logic: Check opening hours, booking windows, and capacity; find suitable tables using a best-fit approach and suggest alternative time slots.
- Automated operations: A server-side scheduler handles time-dependent order updates and notifications.
Some functionality, including payments and SMS/email notifications, is simulated rather than connected to real-world providers.
Tech stack
Layer	Technology
Desktop interface	JavaFX, FXML, CSS
Application code	Java
Networking	OCSF client–server framework
Database	MySQL and JDBC
Development	Eclipse IDE


Architecture
Bistro Client (JavaFX)
        |
        | TCP / OCSF — port 5555
        v
Bistro Server (Java)
        |
        | JDBC — localhost:3306
        v
MySQL database (bistro)
The client and server share message types and domain models from BistroCommon. The server processes requests and accesses the database through repository classes.
Repository structure
BistroCommon/             Shared models and protocol messages
OCSF/                     Client–server communication framework
G18_Prototype_Client/     JavaFX client application
G18_Prototype_Server/     Server and database access code
G18_Assignment3/          Assignment snapshot and example SQL dump
G18_Assignment2/          Earlier assignment material
assignment1/              Earlier assignment material
Old_Versions_Backup/      Historical backups
Getting started (Windows + Eclipse)
Requirements
- JDK 21 (the project has been run locally with OpenJDK 21)
- Compatible JavaFX 21 SDK
- Eclipse IDE with Java development tools
- MySQL Server 8.x and optionally MySQL Workbench
- MySQL Connector/J (a JAR is included in the server project's lib directory)
1. Clone or download
git clone https://github.com/baselkhnifas/G18_project.git
Alternatively, use Code → Download ZIP on GitHub and extract the archive.
2. Set up a local test database
Start MySQL and use MySQL Workbench to open:

G18_Assignment3/G18_Assignment3_DB.sql

Execute the SQL script against a disposable local database. It creates the bistro database, tables, and sample records.

Warning: The supplied SQL dump uses DROP TABLE and contains historical/test records that may include personal-looking information. Review and sanitize the data before republishing, sharing, or importing it into any non-test environment.

The server expects a database connection similar to:
jdbc:mysql://localhost:3306/bistro?serverTimezone=Asia/Jerusalem
user: root
The database password is entered through the server's startup screen; do not commit passwords to Git.
3. Import the Java projects into Eclipse
Import the following directories using File → Import → General → Existing Projects into Workspace:
- BistroCommon
- OCSF
- G18_Prototype_Client
- G18_Prototype_Server
If Eclipse does not detect the server as an existing project, create a Java project pointing to its existing directory, without deleting its source files.

Check Properties → Java Build Path:
- The client must reference BistroCommon and OCSF.
- The server must reference BistroCommon and OCSF.
- Both applications need the JavaFX SDK libraries.
- The server needs its MySQL Connector/J JAR on the build path.
- Replace any broken JRE references with the installed JDK.
4. Run the server
Run the main class:

server.ServerUI in G18_Prototype_Server/src/server/ServerUI.java

For a JavaFX 21 SDK installed separately, add VM arguments in Run → Run Configurations → Arguments (adjust the path):
--module-path "C:\path\to\javafx-sdk-21\lib" --add-modules javafx.controls,javafx.fxml
Enter the local MySQL password in the server window and start the service. The server listens on TCP port 5555.
5. Run the client
Run the main class:

client.ClientUI in G18_Prototype_Client/src/client/ClientUI.java

Use corresponding JavaFX VM arguments for this launch configuration. When prompted for the server IP address:
- On the same computer: 127.0.0.1
- On another computer on the same private network: use the server computer's current LAN IP address.
The client connects to TCP port 5555. Keep the server and MySQL running while using the client.
6. Troubleshooting
- JavaFX runtime components are missing: Confirm that JavaFX VM arguments are set on the specific run configuration.
- Module javafx.controls not found: Confirm --module-path points to the SDK's actual lib directory.
- Unbound JRE / classpath errors: Replace outdated JRE entries and check project references.
- Database connection fails: Check that MySQL is running on port 3306, the bistro database exists, and the credentials match.
- Client cannot connect: Start the server first; check the IP address, port 5555, and Windows Firewall rules for the private network.
Packaging the client for another Windows PC
The client can be exported from Eclipse as a Runnable JAR, but a JAR alone is not a standalone Windows application. A portable distribution also needs a compatible Java runtime and JavaFX runtime libraries, plus all JavaFX resource files (FXML, CSS, images) and shared code.

When the server stays on a separate PC, package only the client. The second machine needs LAN access to the running server on TCP port 5555, but it does not need direct access to MySQL or port 3306.
Security and limitations
This repository is suitable for learning and demonstration. Before any real deployment, review authentication, network encryption, permissions, logging, data handling, test fixtures, and any embedded database records. Do not expose the Bistro server port to the public internet without hardening and access controls.
Contributors
Developed as a team project. Add the contributors' names and links here with their permission, and describe individual contributions accurately.
License
No license is specified here. If the repository does not contain a license file, do not assume permission for reuse or redistribution. Agree on a license with the project contributors before adding one.
