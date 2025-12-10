# Medical Data Export & Analysis Tool STAT

## Overview

STAT is a specialized engineering tool designed for the management, analysis, and export of Nuclear Medicine imaging data. Developed as a project, it bridges the gap between raw database storage and analytical data formats.

The system integrates a C++/Qt desktop application with a relational PostgreSQL database, meticulously designed to reflect the hierarchical DICOM real-world information model (Patient → Study → Series → Image). It allows researchers and medical staff to filter patient cohorts and export structured datasets to XML with automated statistical summaries and XSD validation.

## Key Features

* Hierarchical Data Management: Implements a complex relational model handling patients, studies, series, and image metadata with cascade integrity[cite: 524, 640].
* Advanced Filtering Engine: GUI-based query builder allowing selection by:
    * Patient demographics (Age, Sex).
    * Study details (Date ranges, Modality types like NM, CT, MRI).
* Automated XML Reporting: Generates comprehensive XML reports containing full hierarchy data and calculated statistics (study counts, total sizes)[cite: 530, 842].
* Data Integrity & Validation: Includes a dedicated CLI tool using `libxml2` to validate exported files against a strict XSD schema, ensuring compliance with data standards[cite: 536].
* Performance: Optimized SQL queries for handling large datasets (tested with >600 patients and >1000 images)[cite: 909, 912].

## Database Architecture

The project uses a normalized database schema designed to mirror the DICOM standard:

1.  Patients: `patient_id` (DICOM Tag), `name`, `age`, `sex`.
2.  Studies: Linked to Patients. Contains `study_uid`, `date`, `description`, `institution`.
3.  Series: Linked to Studies. Contains `modality` (e.g., NM), `series_uid`.
4.  Images: Linked to Series. Contains resolution (`rows`, `cols`), `bits_allocated`, `instance_number`.

## Project Setup

### Prerequisites
* **OS:** Linux (Ubuntu/Debian recommended) or Windows (WSL2).
* **Tools:** CMake, G++, Docker & Docker Compose.
* **Libraries:** Qt6 (`qt6-base-dev`), `libqt6sql6-psql`, `libxml2-dev`.

### 1. Database
The easiest way to run the database is using the included Docker configuration.

```bash
cd docker
docker-compose up -d
```

### 2. Build the Application
```bash
mkdir build && cd build
cmake ../src
make
```

### 3. Run the Application
The application requires database connection arguments passed via command line.

```bash
./sim_app <DB_Name> <User> <Port> <Password>
```
