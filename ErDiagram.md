erDiagram
    USERS ||--o{ DISPENSE_LOGS : records
    MEDICINES ||--|{ BATCHES : contains
    CATEGORIES ||--o{ MEDICINES : classifies
    BATCHES ||--o{ DISPENSE_LOGS : used_in

    USERS {
        int id PK
        string full_name
        string email UK
        string password_hash
        enum role "ADMIN | PHARMACIST"
        timestamp created_at
    }

    CATEGORIES {
        int id PK
        string category_name
        string description
    }

    MEDICINES {
        int id PK
        int category_id FK
        string name
        string manufacturer
        string salt_composition
        timestamp updated_at
    }

    BATCHES {
        int id PK
        int medicine_id FK
        string batch_number UK
        date expiry_date
        int stock_quantity
        decimal unit_price
        timestamp created_at
    }

    DISPENSE_LOGS {
        int id PK
        int user_id FK
        int batch_id FK
        string patient_name
        int quantity_dispensed
        timestamp dispensed_at
    }
