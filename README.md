<h1 align="center">CV Builder</h1>
<p align="center">
  A Java desktop application for entering CV details, saving records, and exporting a PDF.
</p>
<p align="center">
  <img src="https://img.shields.io/badge/Language-Java-ED8B00?style=flat-square" alt="Language: Java">
  <img src="https://img.shields.io/badge/GUI-Swing%20%2F%20AWT-0078D4?style=flat-square" alt="GUI: Swing / AWT">
  <img src="https://img.shields.io/badge/Database-SQL%20Server-CC2927?style=flat-square" alt="Database: SQL Server">
  <img src="https://img.shields.io/badge/Export-PDF-6F42C1?style=flat-square" alt="Export: PDF">
  <img src="https://img.shields.io/badge/Status-Educational%20Prototype-F2C94C?style=flat-square" alt="Status: Educational prototype">
</p>
<p align="center">
  <a href="#overview">Overview</a> ·
  <a href="#features">Features</a> ·
  <a href="#quick-start">Quick Start</a> ·
  <a href="#how-to-use">How to Use</a> ·
  <a href="#architecture">Architecture</a> ·
  <a href="#known-issues">Known Issues</a>
</p>

---
# Overview
CV Builder is a **Java Swing desktop project** for the Principles of Database Management course. Users can enter personal details, skills, employment history, education, and an image; save the information to a relational database; look up records by first name; and generate a PDF from the form.
> **Application type:** Despite the earlier title “Online CV Builder”, the supplied source is a local desktop application. There is no web server, browser interface, or user account system.

---

# Features
| | Feature | Implementation |
| :---: | --- | --- |
| 📝 | CV form | Personal information, contact details, skills, employment, and education |
| 🖼️ | Photo attachment | Select an image to include with a CV record |
| 💾 | Save record | Parameterized SQL `INSERT` into the CV table |
| 🔎 | Search record | Parameterized lookup by first name to populate the form |
| 📄 | PDF export | Choose an output file and generate a CV using iText |
| 🧹 | Clear form | Reset entered fields in the desktop interface |

The current source does **not** implement database update or delete actions. “Generate CV” writes a PDF; “Save CV” inserts a database row.

---

# Quick Start
## 1. Prepare your environment
- Install a **JDK** and an IDE that can open a NetBeans Ant project, preferably **NetBeans** for the included `.form` UI file.
- Run a local **SQL Server** instance and know its host, port, database credentials, and connection settings.
- Obtain a **SQL Server JDBC driver JAR** compatible with your JDK and add it to the project libraries. The supplied `dist/lib/` has SQLite, iText, and `rs2xml` JARs, **not** the SQL Server driver required by the active `db.java`.
- Use the included `itextpdf-5.5.4.jar` for PDF generation. The NetBeans project properties contain machine-specific library paths, so relink dependencies in your IDE.

Open the project in the **`CV Generator/`** subfolder. It contains `build.xml`, `nbproject/`, `src/`, and `CVs.sql`.

## 2. Create and align the database
The SQL script starts with `USE CVs;`: create the `CVs` database on your SQL Server instance first. Before running [`CV Generator/CVs.sql`](CV%20Generator/CVs.sql), resolve this table name mismatch:

| File | Name currently used |
| --- | --- |
| `CV Generator/CVs.sql` | `CREATE TABLE CVs` |
| `CV Generator/src/cv/generator/CVs.java` | `INSERT INTO CV` and `SELECT * FROM cv` |

For the smallest setup change, edit the schema line to **`CREATE TABLE CV (`** and then run the SQL script in the `CVs` database. SQL Server's treatment of `CV` versus `cv` depends on collation; if your database is case-sensitive, use the same capitalization in both queries and schema.

The script does not create the database or insert sample data. Keep the SQL file's other column definitions and constraints, including a unique email address.

## 3. Configure the connection
Open `CV Generator/src/cv/generator/db.java` and configure the JDBC URL, username, and password for **your own local SQL Server**. The current file has a hardcoded connection and credential. Remove that credential from code before publishing or sharing the repository; if it is used anywhere else, change it there too.

Also ensure the SQL Server JDBC driver JAR is on the project's **compile and runtime classpaths**. The bundled SQLite database and SQLite JDBC JAR belong to an earlier implementation and are not used by the active connection method.

## 4. Run the GUI
Open `CV Generator/` as a NetBeans project, repair its library references, then run **`cv.generator.CVs`** as the main class.

`cv.generator.CVGenerator` currently has an empty `main` method. The project configuration and packaged `dist/CV_Generator.jar` point to that class, so invoking the included JAR as-is does not launch the form. Update the project's **Run → Main Class** setting to `cv.generator.CVs` and rebuild before using the Ant build output.

<details>
<summary><strong>Troubleshooting</strong></summary>

| Symptom | Check |
| --- | --- |
| Window does not open from the JAR | Reconfigure the main class to `cv.generator.CVs` and rebuild. |
| SQL Server driver class cannot be found | Add a SQL Server JDBC driver JAR to both classpaths. |
| Cannot connect to the database | Match the JDBC URL, host/port, login, and SQL Server configuration to your local instance. |
| “Invalid object name 'CV'” | Align the table name in `CVs.sql` with the queries in `CVs.java`. |
| PDF classes cannot be found | Relink the included iText JAR in project libraries. |
| Save rejects an email | The schema defines a unique constraint on `email`; use a different address for a new row. |

**Validation:** Instructions were checked against the supplied Java, SQL, project metadata, and bundled JAR names. The application was not built or connected to SQL Server in this review environment.

</details>

---

# How to Use
| Step | Action |
| --- | --- |
| **1** | Start the `CVs` form after configuring the database connection. |
| **2** | Enter personal details, skills, experience, and education. Attach a photo if needed. |
| **3** | Select **Save CV** to insert a record into the database. |
| **4** | Type a first name in the search field to look up a saved record. |
| **5** | Select **Generate CV** and choose a location for the PDF. Use **Clear** to reset the form. |

The lookup searches by **first name**, which may match more than one person; the source does not offer a unique-ID search. Check the generated PDF and saved row before relying on their contents.

---

# Architecture
| File | Responsibility |
| --- | --- |
| `CV Generator/src/cv/generator/CVs.java` | Swing form, handlers, SQL queries, image selection, and PDF generation |
| `CV Generator/src/cv/generator/CVs.form` | NetBeans form layout |
| `CV Generator/src/cv/generator/db.java` | JDBC driver loading and connection creation |
| `CV Generator/src/cv/generator/CVGenerator.java` | Configured entry point with an empty `main` method |
| `CV Generator/CVs.sql` | SQL Server table definition |
| `CV Generator/build.xml`, `nbproject/` | Ant and NetBeans build configuration |

<details>
<summary><strong>Database design and libraries</strong></summary>

| Component | Current behavior |
| --- | --- |
| Primary key | `id INT IDENTITY(1,1)` |
| Personal fields | Name, addresses, postal code, nationality, birth date, telephone, and unique email |
| CV fields | Four skill fields, three companies with work descriptions, university, and two qualifications |
| Image | Stored in an `image VARBINARY(MAX)` column |
| SQL access | JDBC `PreparedStatement` is used for insert and first-name search |
| PDF library | Bundled `itextpdf-5.5.4.jar` |
| Historical files | `cvdatabase.sqlite` and `sqlitejdbc-v056.jar` remain in the archive, but active `db.java` selects SQL Server |

This is a **single-table schema**. The current design stores repeated skill and employment fields as numbered columns. Normalizing these into separate related tables would allow an arbitrary number of skills and jobs.

</details>

---

# Project Files
| Path | Contents |
| --- | --- |
| `CV Generator/src/` | Java source and image assets |
| `CV Generator/CVs.sql` | Database schema script |
| `CV Generator/nbproject/` | NetBeans configuration |
| `CV Generator/dist/` | Previously built application JAR and library copies |
| `CV Generator/cvdatabase.sqlite` | Bundled SQLite file from another implementation |

---

# Known Issues
**The included artifacts reflect different development stages.** Complete the configuration steps above before expecting database operations to work.

<details>
<summary><strong>View source-review findings</strong></summary>

| Area | Finding |
| --- | --- |
| Table name | Script creates `CVs`; Java uses `CV` / `cv`. |
| Driver | Active code loads a SQL Server driver absent from the bundled JARs. |
| Entry point | Ant and the packaged JAR use an empty `CVGenerator.main`; the GUI opens from `CVs.main`. |
| Credentials | Database credentials are hardcoded in `db.java`; replace them and avoid committing new secrets. |
| Outdated documentation | Original README claimed update/delete support and listed PDF export as future work. The code has PDF export and no update/delete actions. |
| Input and search | Date and mandatory fields need clearer validation; first-name search can be ambiguous. |
| Resources | Database and statement cleanup is inconsistent, and some exceptions are swallowed. |
| Build reproducibility | NetBeans project properties point to JAR locations on another computer. |
| Tests | No automated test suite is included. |

</details>

---

# Roadmap
- Unify schema and SQL queries around one table name.
- Supply a reproducible SQL Server driver and build configuration.
- Move the database connection details out of source code.
- Set the GUI as the main class and rebuild the distribution JAR.
- Validate form input and use a unique identifier for searches.
- Add update/delete actions only after implementing and testing them.

---

# Contributing
Open an issue or submit a focused pull request. Include your JDK version, database setup, steps to reproduce, and how you verified the change. Remove credentials and personal CV data from examples and screenshots.

# License
No `LICENSE` file is included in the supplied archive. The original README describes educational use, but that statement does not specify a reusable license for the source or bundled libraries. The maintainers should document applicable terms.

---

<p align="center"><a href="#cv-builder">Back to top ↑</a></p>
