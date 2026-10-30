# Light Bill Calculator

A complete, beginner-friendly **Electricity Bill Calculator** web application built using **PHP 8**, **MySQL**, and **Pure CSS3** (No Bootstrap or external CSS/JS frameworks). 

This project is designed as a practical laboratory assignment for second-year Computer Engineering students. It includes slab-wise rate calculations, strict HTML5 and server-side validations, secure database storage using prepared statements, and a printable responsive invoice view.

---

## Features

1. **Responsive Input Form:** Modern centered UI with inputs for Customer Name, Address, Mobile Number, billing Month, and Units Consumed.
2. **Slab-Wise Billing Logic:**
   - 0 – 50 Units: **₹3.50** / unit
   - 51 – 100 Units: **₹4.00** / unit
   - 101 – 200 Units: **₹5.20** / unit
   - Above 200 Units: **₹6.50** / unit
3. **Database Integration:** Automatic, secure saving of every transaction using SQL prepared statements to prevent SQL Injection.
4. **Professional Receipt Invoice:** Generates a clean receipt featuring an auto-incremented Bill Number, dynamic local Date & Time, and a print button.
5. **Robust Error Handling:** Checks if MySQL is running, if the database is missing, or if the table is not imported, providing helpful step-by-step resolution interfaces.

---

## Project Structure

```text
LightBill/
│
├── index.php         # Main input form (HTML5 + PHP validation & layout)
├── calculate.php     # Bill calculator & DB storage script
├── db.php            # Database connection configuration & health-checks
├── style.css         # Styling stylesheet (Inter fonts, cards, print-optimized)
├── database.sql      # Database schema creation script
└── README.md         # Setup and configuration documentation
```

---

## Installation & Setup Instructions

Follow these steps to run the project on your local machine using **XAMPP**:

### Step 1: Install XAMPP
1. Download **XAMPP** for Windows from [Apache Friends](https://www.apachefriends.org/).
2. Run the installer and complete the setup. By default, it will be installed in `C:\xampp`.
3. Open the **XAMPP Control Panel** from the start menu.
4. Start both **Apache** and **MySQL** services by clicking the **Start** buttons next to them. (They should turn green).

### Step 2: Copy the Project Files
1. Copy the entire `LightBill` project folder.
2. Paste it into the XAMPP document root folder, which is located at:
   `C:\xampp\htdocs\`
3. Ensure the structure looks like:
   `C:\xampp\htdocs\LightBill\index.php`

### Step 3: Create & Import Database
1. Open your web browser and navigate to:
   `http://localhost/phpmyadmin/`
2. Click on the **New** button in the left sidebar to create a database.
3. Enter `lightbill` as the Database name and click **Create**.
4. Select the newly created `lightbill` database.
5. Click on the **Import** tab at the top menu.
6. Click **Choose File** and select the `database.sql` file located inside your project folder (`C:\xampp\htdocs\LightBill\database.sql`).
7. Scroll down to the bottom and click **Import** (or **Go**).
8. The database tables will be imported successfully.

---

## How to Run the Project

1. Keep the **Apache** and **MySQL** services running in the XAMPP Control Panel.
2. Open your web browser.
3. Access the project using the following URL:
   👉 **[http://localhost/LightBill/](http://localhost/LightBill/)**

---

## Code Logic & Calculations

### 1. Database Connection (`db.php`)
Uses the standard `mysqli_connect` method to connect to the MySQL database. It enables `MYSQLI_REPORT_ERROR | MYSQLI_REPORT_STRICT` exceptions so that if the database is missing or the table is not imported, it catches the error and displays a beautiful troubleshooting UI instead of raw, confusing warnings.

### 2. Bill Calculation (`calculate.php`)
Calculations are processed on the server-side after form validation:
```php
if ($units <= 50) {
    $rate = 3.50;
} elseif ($units <= 100) {
    $rate = 4.00;
} elseif ($units <= 200) {
    $rate = 5.20;
} else {
    $rate = 6.50;
}

$total_bill = $units * $rate;
```

### 3. Prepared Statements (`calculate.php`)
We secure input parameters against SQL injection attacks before inserting into the table:
```php
$insert_query = "INSERT INTO `electricity_bill` (`customer_name`, `address`, `mobile`, `bill_month`, `units`, `rate`, `total_bill`) VALUES (?, ?, ?, ?, ?, ?, ?)";
$stmt = mysqli_prepare($conn, $insert_query);
mysqli_stmt_bind_param($stmt, "ssssidd", $name, $address, $mobile, $month, $units, $rate, $total_bill);
mysqli_stmt_execute($stmt);
```
