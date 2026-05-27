# Branch Control API Documentation
# ブランチ管理API ドキュメント

---

# 1. Save Branch
# ブランチ作成

## English

Admin creates a new branch by providing a branch office code, name, and lock code.

## Japanese

管理者はブランチコード、名前、ロックコードを指定して新しいブランチを作成します。

---

## Notes
## 注意事項

- Only Admin can create branches
  ブランチの作成は管理者のみが行えます

- `branchOfficeCode` must be unique
  `branchOfficeCode` は一意である必要があります

- Record is inserted into both `branch_office` and `branch_info` tables
  レコードは `branch_office` と `branch_info` の両テーブルに挿入されます

---

## Endpoint

```text
POST /branch/save
(ADMIN)
```

---

## Request

```json
{
  "branchOfficeCode": "HO123",
  "branchOfficeName": "Head Office",
  "lockCode": "12345"
}
```

## Response

```json
Success
```

---

## Flow Diagram

```mermaid
flowchart TD

    A[Admin]
    --> B[Call /branch/save API]

    B --> C[Validate Request]

    C --> D[Insert Into branch_office TABLE]

    D --> E[Insert Into branch_info TABLE]

    E --> F[Success Response]

    %% Styles
    style A fill:#1E3A8A,color:#ffffff,stroke:#0F172A,stroke-width:2px
    style B fill:#2563EB,color:#ffffff,stroke:#1E40AF,stroke-width:2px
    style C fill:#D97706,color:#ffffff,stroke:#92400E,stroke-width:2px
    style D fill:#7C3AED,color:#ffffff,stroke:#4C1D95,stroke-width:2px
    style E fill:#9333EA,color:#ffffff,stroke:#581C87,stroke-width:2px
    style F fill:#059669,color:#ffffff,stroke:#065F46,stroke-width:2px
```

---

## Database Update

| Table         | Action        |
|---------------|---------------|
| branch_office | Insert Record |
| branch_info   | Insert Record |

---

# 2. Update Branch
# ブランチ更新

## English

Admin updates an existing branch's name and lock code using its branch office code.

## Japanese

管理者はブランチコードを使用して既存のブランチ名とロックコードを更新します。

---

## Notes
## 注意事項

- `branchOfficeCode` must already exist
  `branchOfficeCode` は既に存在している必要があります

- Only `branchOfficeName` and `lockCode` can be updated
  更新できるのはブランチ名とロックコードのみです

---

## Endpoint

```text
PUT /branch/save
(ADMIN)
```

---

## Request

```json
{
  "branchOfficeCode": "B3",
  "branchOfficeName": "Branch Three",
  "lockCode": "A527"
}
```

## Response

```json
Success
```

---

## Flow Diagram

```mermaid
flowchart TD

    A[Admin]
    --> B[Call /branch/save API - PUT]

    B --> C{Branch Exists?}

    C -->|Yes| D[Update branch_office TABLE]

    D --> E[Success Response]

    C -->|No| F[Branch Not Found Error]

    %% Styles
    style A fill:#1E3A8A,color:#ffffff,stroke:#0F172A,stroke-width:2px
    style B fill:#2563EB,color:#ffffff,stroke:#1E40AF,stroke-width:2px
    style C fill:#D97706,color:#ffffff,stroke:#92400E,stroke-width:2px
    style D fill:#7C3AED,color:#ffffff,stroke:#4C1D95,stroke-width:2px
    style E fill:#059669,color:#ffffff,stroke:#065F46,stroke-width:2px
    style F fill:#B91C1C,color:#ffffff,stroke:#7F1D1D,stroke-width:2px
```

---

## Database Update

| Table         | Action        |
|---------------|---------------|
| branch_office | Update Record |

---

# 3. Deactivate Branch
# ブランチ無効化

## English

Admin deactivates an existing branch. The branch is not permanently deleted but marked inactive.

## Japanese

管理者は既存のブランチを無効化します。ブランチは完全に削除されず、非アクティブとしてマークされます。

---

## Notes
## 注意事項

- Branch is deactivated, not deleted
  ブランチは削除ではなく非アクティブ化されます

- Only Admin can deactivate branches
  ブランチの無効化は管理者のみが行えます

---

## Endpoint

```text
DELETE /branch/deactivate
(ADMIN)
```

---

## Request

```json
{
  "branchCode": "B2"
}
```

---

## Flow Diagram

```mermaid
flowchart TD

    A[Admin]
    --> B[Call /branch/deactivate API]

    B --> C{Branch Exists?}

    C -->|Yes| D[Mark Branch as Inactive]

    D --> E[Update branch_office TABLE]

    E --> F[Success Response]

    C -->|No| G[Branch Not Found Error]

    %% Styles
    style A fill:#1E3A8A,color:#ffffff,stroke:#0F172A,stroke-width:2px
    style B fill:#2563EB,color:#ffffff,stroke:#1E40AF,stroke-width:2px
    style C fill:#D97706,color:#ffffff,stroke:#92400E,stroke-width:2px
    style D fill:#DC2626,color:#ffffff,stroke:#7F1D1D,stroke-width:2px
    style E fill:#7C3AED,color:#ffffff,stroke:#4C1D95,stroke-width:2px
    style F fill:#059669,color:#ffffff,stroke:#065F46,stroke-width:2px
    style G fill:#B91C1C,color:#ffffff,stroke:#7F1D1D,stroke-width:2px
```

---

## Database Update

| Table         | Action                    |
|---------------|---------------------------|
| branch_office | Update Status to Inactive |

---

# 4. Get Active Branches
# アクティブブランチ取得

## English

Admin retrieves all currently active branches.

## Japanese

管理者は現在アクティブなすべてのブランチを取得します。

---

## Endpoint

```text
GET /branch/getActive
(ADMIN)
```

---

## Flow Diagram

```mermaid
flowchart TD

    A[Admin]
    --> B[Call /branch/getActive API]

    B --> C[Query branch_office TABLE WHERE status = active]

    C --> D[Return Active Branches List]

    %% Styles
    style A fill:#1E3A8A,color:#ffffff,stroke:#0F172A,stroke-width:2px
    style B fill:#2563EB,color:#ffffff,stroke:#1E40AF,stroke-width:2px
    style C fill:#7C3AED,color:#ffffff,stroke:#4C1D95,stroke-width:2px
    style D fill:#059669,color:#ffffff,stroke:#065F46,stroke-width:2px
```

---

# 5. Get All Branches
# 全ブランチ取得

## English

Admin retrieves all branches, including inactive ones.

## Japanese

管理者は非アクティブなブランチを含む、すべてのブランチを取得します。

---

## Endpoint

```text
GET /branch/getAll
(ADMIN)
```

---

## Flow Diagram

```mermaid
flowchart TD

    A[Admin]
    --> B[Call /branch/getAll API]

    B --> C[Query branch_office TABLE - All Records]

    C --> D[Return All Branches List]

    %% Styles
    style A fill:#1E3A8A,color:#ffffff,stroke:#0F172A,stroke-width:2px
    style B fill:#2563EB,color:#ffffff,stroke:#1E40AF,stroke-width:2px
    style C fill:#7C3AED,color:#ffffff,stroke:#4C1D95,stroke-width:2px
    style D fill:#059669,color:#ffffff,stroke:#065F46,stroke-width:2px
```

---

# API Summary
# API サマリー

| Endpoint             | Method | Description (EN)          | Description (JP)           |
|----------------------|--------|---------------------------|----------------------------|
| /branch/save         | POST   | Create new branch         | 新しいブランチを作成       |
| /branch/save         | PUT    | Update existing branch    | 既存ブランチを更新         |
| /branch/deactivate   | DELETE | Deactivate a branch       | ブランチを無効化           |
| /branch/getActive    | GET    | Fetch all active branches | アクティブブランチを取得   |
| /branch/getAll       | GET    | Fetch all branches        | 全ブランチを取得           |

---

# Complete Sequence Diagram
# 全体シーケンス図

```mermaid
sequenceDiagram

    participant Admin
    participant System
    participant Database

    Admin->>System: POST /branch/save
    activate System

    System->>Database: INSERT INTO branch_office
    activate Database
    Database-->>System: Success
    deactivate Database

    System->>Database: INSERT INTO branch_info
    activate Database
    Database-->>System: Success
    deactivate Database

    System-->>Admin: Success Response
    deactivate System

    Admin->>System: PUT /branch/save
    activate System

    System->>Database: Check if branch exists
    activate Database

    alt Branch Found

        Database-->>System: Branch OK

        System->>Database: UPDATE branch_office SET branchOfficeName, lockCode

        Database-->>System: Updated

        System-->>Admin: Success Response

    else Branch Not Found

        Database-->>System: Not Found

        System-->>Admin: Error Response

    end

    deactivate Database
    deactivate System

    Admin->>System: DELETE /branch/deactivate
    activate System

    System->>Database: Check if branch exists
    activate Database

    alt Branch Found

        Database-->>System: Branch OK

        System->>Database: UPDATE branch_office SET status = inactive

        Database-->>System: Deactivated

        System-->>Admin: Success Response

    else Branch Not Found

        Database-->>System: Not Found

        System-->>Admin: Error Response

    end

    deactivate Database
    deactivate System

    Admin->>System: GET /branch/getActive
    activate System

    System->>Database: SELECT * FROM branch_office WHERE active
    activate Database

    Database-->>System: Active Branches

    System-->>Admin: Active Branches List

    deactivate Database
    deactivate System

    Admin->>System: GET /branch/getAll
    activate System

    System->>Database: SELECT * FROM branch_office
    activate Database

    Database-->>System: All Branches

    System-->>Admin: Full Branches List

    deactivate Database
    deactivate System
```