```mermaid
---
config:
  layout: elk
---
erDiagram
    user {
        INT user_id PK
        NVARCHAR user_full_name
        VARCHAR username
        VARCHAR password_hash
        INT role_id FK
        BIT is_active
        BIT has_seen_guide
    }
    role {
        INT role_id PK
        VARCHAR role_name
        NVARCHAR description
    }
    category {
        INT category_id PK
        NVARCHAR category_name
        NVARCHAR description
    }
    product {
        INT product_id PK
        NVARCHAR product_name
        INT category_id FK
        NVARCHAR brand_name
        FLOAT cost_price
        FLOAT sell_price
        INT stock_quantity
        NVARCHAR description
        BIT is_draft
        DATETIME created_at
        DATETIME updated_at
    }
    customer {
        INT customer_id PK
        NVARCHAR customer_full_name
        VARCHAR phone
        VARCHAR email
        NVARCHAR address
        DATETIME created_at
    }
    orders {
        BIGINT order_id PK
        INT customer_id FK
        INT user_id FK
        DATETIME order_date
        FLOAT total_amount
        VARCHAR status
        BIT is_draft
        NVARCHAR note
    }
    order_detail {
        INT order_detail_id PK
        BIGINT order_id FK
        INT product_id FK
        INT quantity
        FLOAT unit_price
        FLOAT sub_total
    }
    payment {
        INT payment_id PK
        BIGINT order_id FK
        VARCHAR payment_method
        FLOAT amount
        DATETIME payment_date
    }
    app_setting {
        VARCHAR key PK
        NVARCHAR value
        NVARCHAR description
    }
    commission {
        INT commission_id PK
        INT user_id FK
        INT month
        INT year
        FLOAT total_sales
        FLOAT commission_rate
        FLOAT commission_amount
    }

    %% Relationships
    user }o--|| role : belongs_to
    product }o--|| category : belongs_to
    orders }o--|| customer : placed_by
    orders }o--|| user : created_by
    order_detail }o--|| orders : belongs_to
    order_detail }o--|| product : contains
    payment ||--o{ orders : paid_via
    commission }o--|| user : belongs_to
