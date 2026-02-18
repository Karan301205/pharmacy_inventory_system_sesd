# ER Diagram — PharmaLog

## Overview

This Entity-Relationship diagram represents the database schema for **PharmaLog**, an automated pharmacy inventory and prescription management system.  
All entities, attributes, data types, and relationships are defined to model professional healthcare workflows, including batch-level tracking, expiry management, and secure dispensing logs for audit compliance.

---

```mermaid
erDiagram

    USERS {
        uuid id PK
        varchar email UK
        varchar password_hash
        varchar full_name
        enum role "ADMIN | PHARMACIST"
        boolean is_active
        timestamp created_at
        timestamp updated_at
    }

    CATEGORIES {
        uuid id PK
        varchar name UK
        text description
    }

    MEDICINES {
        uuid id PK
        uuid category_id FK
        varchar name
        varchar manufacturer
        varchar salt_composition
        text instructions
        timestamp created_at
    }

    BATCHES {
        uuid id PK
        uuid medicine_id FK
        varchar batch_number UK
        date expiry_date
        integer initial_quantity
        integer current_stock
        decimal unit_price
        timestamp created_at
    }

    DISPENSE_LOGS {
        uuid id PK
        uuid pharmacist_id FK
        uuid batch_id FK
        varchar patient_name
        varchar doctor_name
        integer quantity_dispensed
        decimal total_price
        timestamp dispensed_at
    }

    NOTIFICATIONS {
        uuid id PK
        enum type "LOW_STOCK | EXPIRY_WARNING | SYSTEM"
        varchar title
        text message
        boolean is_read
        timestamp created_at
    }

    AUDIT_LOGS {
        uuid id PK
        uuid user_id FK
        varchar action
        varchar entity_type
        uuid entity_id
        json details
        timestamp created_at
    }

    %% ===== RELATIONSHIPS =====

    USERS ||--o{ DISPENSE_LOGS : "records"
    USERS ||--o{ AUDIT_LOGS : "generates"

    CATEGORIES ||--o{ MEDICINES : "classifies"

    MEDICINES ||--|{ BATCHES : "contains"
    MEDICINES ||--o{ DISPENSE_LOGS : "referenced in"

    BATCHES ||--o{ DISPENSE_LOGS : "used_in"

    NOTIFICATIONS }o--|| USERS : "sent_to"
