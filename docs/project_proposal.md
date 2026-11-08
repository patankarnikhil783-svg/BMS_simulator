# Project Proposal

## 1. Project Information

### 1.1 Project Title

**EV Battery Management System (BMS) Simulator in C**

### 1.2 Engineering Domain

Automotive (Electric Vehicle Battery Management)

### 1.3 Student Details

| Field | Details |
|---|---|
| Student name | Nikhil Shripad Patankar |
| Roll number | 612509029 |
| Division / batch | Instrumentation and Control Engineering |
| Repository name | BMS_simulator |
| GitHub repository link | https://github.com/patankarnikhil783-svg/BMS_simulator |

### 1.4 Individual Project Declaration

I confirm that this project is my individual work. I understand the design,
source code, unit tests, Makefile and documentation included in this
repository, and I am prepared to explain or modify any part during evaluation.

Student acknowledgement: Nikhil Shripad Patankar, 07/07/2026

## 2. Engineering Scenario and Problem Definition

### 2.1 Scenario Description

**Where it occurs.** The scenario takes place in an electric vehicle (EV)
battery development and test laboratory. A battery pack is charged and
discharged on a test bench while a data logger records its measurements.

**Who is involved.** BMS software engineers, who write the battery protection
logic, and test and validation engineers, who review the recorded data to check
that the pack stayed within safe limits.

**Current working method.** The logged data is exported as a CSV file and
reviewed manually in a spreadsheet. Engineers scan the values and compare them
with the battery limits by eye.

**Information that must be managed.** For each time stamp: the voltage of every
cell in the pack, the pack current and the pack temperature. From these the
following are derived: State of Charge (SoC), cell imbalance, fault severity
and the contactor (pack disconnect) state.

**Problems with the current method.**
- Limits are applied by eye, so a borderline reading can be missed.
- Bad data such as blank cells, text or impossible values can pass unnoticed.
- Different people may apply different limits to the same data.
- There is no repeatable record of which faults were found and when.

### 2.2 Problem Statement

In an EV battery test laboratory, BMS and validation engineers need to check
recorded battery data against safety limits. They currently review the data
manually in spreadsheets, which results in missed faults, undetected invalid
readings, inconsistent limits and no repeatable fault record. The proposed
software will validate every recorded sample, check it against defined limits,
estimate the State of Charge, record faults and report the final pack status,
so that engineers can verify protection behaviour quickly and consistently.

### 2.3 Proposed Solution

The BMS Simulator is a console application written in C. It replays recorded
battery measurements through the same decision logic a Battery Management
System would use.

The application will:
- load a CSV file of battery measurements, for a pack of 4 series cells;
- validate each sample and reject malformed or implausible data with a reason;
- check cell voltage, temperature and current against warning and critical limits;
- calculate cell imbalance and estimate SoC by coulomb counting;
- decide the contactor state, which opens on a critical fault;
- keep up to 1000 processed samples in memory;
- provide a menu so the user can load a file, run the simulation, view the
  summary, filter events by severity and check a single reading entered by hand;
- write a fault log file and show a summary report.

The project is a software simulation only. It does not interface with any
hardware.

### 2.4 Target Users

| User | Responsibility | How they use the application |
|---|---|---|
| BMS software engineer | Develops battery protection logic | Replays recorded data to check that limits and decisions behave as intended |
| Test and validation engineer | Reviews battery test results | Loads logged data, reviews faults and the summary, and keeps the fault log |
| Student or evaluator (secondary) | Learns from or assesses the project | Builds, runs and tests the application to see the design in action |

## 3. Project Scope

### 3.1 In-Scope Features

1. Load battery measurements from a CSV file for a pack of 4 series cells.
2. Parse and validate every sample, rejecting malformed or implausible data
   with the line number and the reason.
3. Check each cell voltage, the pack temperature and the pack current against
   warning and critical limits.
4. Calculate cell imbalance (highest minus lowest cell voltage).
5. Estimate State of Charge (SoC) by coulomb counting.
6. Decide the contactor state, which opens on a critical fault and stays open.
7. Store up to 1000 processed samples in memory.
8. Provide a console menu to load a file, run the simulation, show the summary,
   filter events by severity and check a single reading entered by the user.
9. Validate all user input, including menu choices and file names.
10. Write a fault log file and display a summary report.

### 3.2 Out-of-Scope Features

1. Any hardware interface, such as sensors, a microcontroller or CAN communication.
2. Live (real-time) data acquisition.
3. State of Health (SoH) estimation, Kalman filtering and thermal modelling.
4. Active cell balancing.
5. A graphical user interface.
6. Packs with a different number of cells, or support for several packs at once.

### 3.3 Assumptions

1. The battery is a typical NMC lithium-ion pack, so the default limits follow
   that chemistry and are held in one place so they can be changed.
2. Discharge current is positive and charge current is negative.
3. Units are volts, amperes, degrees Celsius, seconds and ampere-hours.
4. Samples in the input file are in increasing time order.
5. The pack capacity is 50 Ah and the initial SoC is 80 %, unless changed.
6. The input file is a plain-text CSV file with a header line.
7. The program is used on a Linux system with gcc, make and Google Test installed.

### 3.4 Constraints

1. The project is an individual project with limited time.
2. The application is written in C (C99) and built with a Makefile.
3. The user interface is a command-line (console) menu.
4. Storage is file-based, with no database.
5. At most 1000 samples are held in memory, using fixed-size arrays and no
   dynamic memory allocation.
6. The pack is fixed at 4 cells.
7. Unit tests are written with Google Test.