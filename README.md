# 👶 Infant Suction Measurement App V2

### *Real-Time Bio-Feedback & Pediatric Analytics System*

This project was built to revolutionize the way infant suction development is monitored. Developed with **Flutter**, this completely overhauled application addresses the core limitations of the first-generation (V1) system by integrating with high-precision hardware sensors, enabling infinite historical data persistence, and offering advanced comparative analytical graphing.

---

## 🚨 Disclaimer
> **This is a Private Repository to protect proprietary source code, organizational data, and intellectual property.** This public-facing documentation is designed to showcase the system's architecture, design patterns, technical challenges, and final outcomes for portfolio purposes.

---

## 🎯 The Problem & Solution

### The Challenge in V1:
*   **Sensor Limitation:** The first-generation system utilized a makeshift force/pressure sensor which measured pressing force rather than true biological suction pressure (vacuum-based), leading to inaccurate clinical readings.
*   **Data Bottleneck:** The legacy application suffered from extreme storage limitations—users could only compare the **last 2 sessions** and view detailed metric breakdowns strictly for the patient's **latest session**. This prevented pediatricians and specialists from tracking long-term clinical progress.

### The V2 Solution:
*   **True Suction Integration:** Re-engineered to process direct suction/vacuum inputs from the upgraded V2 medical-grade hardware sensor, providing highly accurate, raw biological data.
*   **Complete App Overhaul:** Rewrote the entire application from scratch using **Flutter (Cross-platform)** for a high-performance, modern user experience.
*   **Infinite Data Retention:** Implemented an optimized relational database schema capable of storing **unlimited historical measurement sessions** per patient.
*   **Advanced Analytics:** Developed a customized multi-graph analytics engine allowing users to select and overlay any historical sessions on a single timeline to visualize growth trends and clinical development.

---

## 🏗️ High-Level Architecture & Tech Stack

### Tech Stack Badges
![Flutter](https://img.shields.io/badge/Flutter-%2302569B.svg?style=for-the-badge&logo=Flutter&logoColor=white)
![Dart](https://img.shields.io/badge/dart-%230175C2.svg?style=for-the-badge&logo=dart&logoColor=white)
![SQLite](https://img.shields.io/badge/sqlite-%2307405e.svg?style=for-the-badge&logo=sqlite&logoColor=white)
![Bluetooth](https://img.shields.io/badge/bluetooth-%230082FC.svg?style=for-the-badge&logo=bluetooth&logoColor=white)

### System Architecture Diagram
The system is built on **Clean Architecture** principles, strictly decoupling responsibilities into Presentation, Domain, and Data layers to ensure testability and robust maintenance.

```mermaid
flowchart TD
    Start([Start]) --> AddPatient[Add Infant Patient Profile]
    AddPatient --> ConnectBLE[Connect via Bluetooth to Suction Device]
    ConnectBLE --> SetTimer[Set Test Duration Time]
    
    SetTimer --> CheckConnection{Is Connected to\nSuction Device?}
    CheckConnection -- False --> ConnectBLE
    
    CheckConnection -- True --> SendStart[Send 'Start' Command\nto Suction Device]
    SendStart --> StartTimer[Start Timer Clock]
    
    StartTimer --> ReceiveData[Receive Suction Pressure Data\nfrom Suction Device]
    ReceiveData --> DisplayRealtime[/Display Current Suction Value\n& Remaining Time/]
    
    DisplayRealtime --> CacheData[Cache Current Suction Value\n& Corresponding Timestamp]
    CacheData --> CheckTimer{Has the Set Time\nExpired?}
    
    CheckTimer -- False --> ReceiveData
    
    CheckTimer -- True --> SummarizeResult[Summarize All Measurement Data]
    SummarizeResult --> DisplaySummary[/Display Measurement\nSummary Results/]
    
    DisplaySummary --> CheckSave{Save Test Results\nto History?}
    
    CheckSave -- True --> SaveData[Save Test Results\nto Local Database]
    SaveData --> DisplaySaved[/Display All Saved\nHistorical Results/]
    DisplaySaved --> End([End])
    
    CheckSave -- False --> End