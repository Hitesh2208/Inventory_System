# Inventory Management System

A desktop **Inventory Management System** built in **Java** with a **JavaFX** GUI and a **MySQL** backend. It lets users manage products, manufacturers, subcategories and customer transactions through a clean graphical interface, with login authentication and role-based access control (RBAC).

---

## Features

- **Product management** – add, delete and view products (UPC, name, quantity, retail price, manufacturer, subcategory).
- **Transaction management** – record and view customer orders with order quantity, total paid, date/time and customer details.
- **Authentication** – user login validated against an XML credentials file.
- **Role-Based Access Control (RBAC)** – `admin` and `user` roles with distinct privileges, defined in a JSON configuration file.
- **MySQL persistence** – CRUD operations performed through parameterized SQL statements (protects against SQL injection).
- **Layered architecture** – clear separation of models, services and controllers.
- **Unit tests** – JUnit test suites for the model layer.
- **Dependency vulnerability scanning** – OWASP Dependency-Check integrated into the Maven build.

---

## Tech Stack

| Layer            | Technology                          |
|------------------|-------------------------------------|
| Language         | Java 14                             |
| UI               | JavaFX 15 (FXML views)              |
| Database         | MySQL (`mysql-connector-java` 8.0.23) |
| Build tool       | Maven                               |
| JSON parsing     | `org.json`                          |
| Testing          | JUnit 4                             |
| Security scan    | OWASP Dependency-Check Maven plugin |

---

## Project Structure

```
Inventory_System/
├── pom.xml
└── src/
    ├── main/
    │   ├── java/inventory/
    │   │   ├── Main.java                 # Application entry point (loads Login view)
    │   │   ├── controllers/              # JavaFX controllers for each view
    │   │   │   ├── LoginController.java
    │   │   │   ├── HomeController.java
    │   │   │   ├── ProductsViewController.java
    │   │   │   ├── AddDeleteProductController.java
    │   │   │   ├── TransactionsController.java
    │   │   │   ├── AddDeleteTransactionsController.java
    │   │   │   └── FXML.java
    │   │   ├── models/                   # Domain models
    │   │   │   ├── Product.java
    │   │   │   ├── Transaction.java
    │   │   │   ├── Customer.java
    │   │   │   ├── Manufacturer.java
    │   │   │   ├── Subcategory.java
    │   │   │   └── User.java
    │   │   ├── services/                 # Business logic & data access
    │   │   │   ├── DBConnection.java     # MySQL connection settings
    │   │   │   ├── DBHandler.java        # CRUD operations
    │   │   │   ├── Authenticator.java    # XML-based login
    │   │   │   ├── Authorizer.java       # RBAC logic
    │   │   │   ├── JsonParser.java
    │   │   │   ├── XmlParser.java
    │   │   │   ├── ParseNumbers.java
    │   │   │   ├── Row.java
    │   │   │   ├── RBAC.json             # Roles & privileges config
    │   │   │   └── creds.xml             # User credentials (template)
    │   │   └── views/                    # FXML UI layouts
    │   └── resources/gui_images/         # Icons & logo
    └── test/java/                        # JUnit tests
        ├── models/
        │   ├── TestProduct.java
        │   └── TestTransaction.java
        └── suites/
            ├── AllTests.java
            └── ModelSuite.java
```

---

## Getting Started

### Prerequisites

- **JDK 14** or higher
- **Maven 3.6+**
- **MySQL Server** running locally
- A JavaFX-compatible runtime (handled via Maven dependencies)

### 1. Clone the repository

```bash
git clone https://github.com/Hitesh2208/Inventory_System.git
cd Inventory_System/Inventory_System
```

### 2. Set up the database

Create a MySQL database and the required tables (e.g. `product`, `transactions`). At minimum the `product` table maps to the following columns:

```sql
CREATE TABLE product (
    productID    INT AUTO_INCREMENT PRIMARY KEY,
    upc          VARCHAR(50),
    productName  VARCHAR(255),
    quantity     INT,
    retailPrice  DOUBLE,
    manufacturer INT,
    subcategory  INT
);
```

> Adjust column definitions to match the SQL statements in `DBHandler.java`.

### 3. Configure the database connection

Edit `src/main/java/inventory/services/DBConnection.java`:

```java
private static final String URL = "jdbc:mysql://localhost:3306/your_database_name";
private static final String USERNAME = "your_mysql_username";
private static final String PASSWORD = "your_mysql_password";
```

### 4. Configure login credentials

Edit `src/main/java/inventory/services/creds.xml` with your own username and password:

```xml
<users>
    <user>
        <username>your_username</username>
        <pwd>your_password</pwd>
    </user>
</users>
```

> **Note:** `Authenticator.java` and `Authorizer.java` currently reference absolute file paths for `creds.xml` and `RBAC.json`. Update these paths to match your local environment (ideally make them relative/classpath-based).

### 5. Configure roles (optional)

User roles and privileges are defined in `src/main/java/inventory/services/RBAC.json`. The default roles are:

- **admin** – `create_user`, `delete_user`, `update_inventory`
- **user** – `view_inventory`, `place_order`

### 6. Build and run

```bash
mvn clean install
mvn javafx:run
```

The application launches with the **Login** screen.

---

## Running Tests

```bash
mvn test
```

JUnit suites under `src/test/java` cover the `Product` and `Transaction` models.

---

## Security Notes

This project includes a couple of areas worth hardening before production use:

- **Credentials are stored in plaintext** in `creds.xml` (there is a `FIXME` to add encryption) and database credentials are hardcoded in `DBConnection.java`. Consider environment variables or an encrypted secrets store.
- **Absolute hardcoded file paths** in the service layer should be made portable.
- The build runs **OWASP Dependency-Check** to flag vulnerable dependencies.

---

## License

No license file is currently included. Add a `LICENSE` file if you intend to make the project open source.

---

## Author

**Hitesh** ([@Hitesh2208](https://github.com/Hitesh2208))
