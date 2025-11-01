```mermaid
---
config:
  layout: elk
---
erDiagram
    app_setting {
        int app_setting_id PK
        string key
        string value
        int value_type
        string category
        string description
        bool is_system
        datetime updated_at
    }

    role {
        int role_id PK
        string name
        string description
    }

    user {
        int user_id PK
        string user_full_name
        string user_name
        string password_hash
        int role_id FK
        bool is_active
        bool has_seen_guide
    }

    category {
        int category_id PK
        string category_name
        string description
    }

    product {
        int product_id PK
        string product_name
        int category_id FK
        string brand_name
        string color
        int storage_capacity
        string processor
        decimal screen_size
        int battery_capacity
        string image_url
        decimal cost_price
        decimal sell_price
        int stock_quantity
        string description
        bool is_draft
        datetime created_at
        datetime updated_at
    }

    customer {
        int customer_id PK
        string customer_name
        string phone_number
        string email
        string address
    }

    order {
        int order_id PK
        int customer_id FK
        int user_id FK
        datetime order_date
        string status
        decimal subtotal
        decimal discount
        decimal tax
        decimal total
        string notes
    }

    order_detail {
        int order_detail_id PK
        int order_id FK
        int product_id FK
        int quantity
        decimal unit_price
        decimal discount
        decimal line_total
    }

    payment {
        int payment_id PK
        int order_id FK
        string payment_method
        decimal amount
        datetime payment_date
    }

    commission {
        int commission_id PK
        int user_id FK
        decimal amount
        datetime created_at
    }

    %% Relationships
    role ||--o{ user : has
    user ||--o{ order : creates
    customer ||--o{ order : places
    category ||--o{ product : categorizes
    product ||--o{ order_detail : appears_in
    order ||--o{ order_detail : contains
    order ||--o{ payment : paid_by
    user ||--o{ commission : earns
```