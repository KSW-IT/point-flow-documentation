# Role Control API Documentation
# ロール管理API ドキュメント

---

# 1. Save Role
# ロール作成

## English

Admin creates a new role by providing a privilege code, name, and the user performing the action.

## Japanese

管理者は権限コード、名前、操作ユーザーを指定して新しいロールを作成します。

---

## Notes
## 注意事項

- Only Admin can create roles
  ロールの作成は管理者のみが行えます

- `privilegeCode` must be unique
  `privilegeCode` は一意である必要があります

---

## Endpoint

```text
POST /role/save
(ADMIN)
```

---

## Request

```json
{
  "privilegeCode": "AT",
  "nameOfPrivilege": "Accountent",
  "enteredByUser": "Admin"
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
    --> B[Call /role/save API]

    B --> C[Validate Request]

    C --> D[Insert Into role TABLE]

    D --> E[Success Response]

    %% Styles
    style A fill:#1E3A8A,color:#ffffff,stroke:#0F172A,stroke-width:2px
    style B fill:#2563EB,color:#ffffff,stroke:#1E40AF,stroke-width:2px
    style C fill:#D97706,color:#ffffff,stroke:#92400E,stroke-width:2px
    style D fill:#7C3AED,color:#ffffff,stroke:#4C1D95,stroke-width:2px
    style E fill:#059669,color:#ffffff,stroke:#065F46,stroke-width:2px
```

---

## Database Update

| Table  | Action        |
|--------|---------------|
| role   | Insert Record |

---

# 2. Update Role
# ロール更新

## English

Admin updates an existing role's name using its privilege code.

## Japanese

管理者は権限コードを使用して既存のロール名を更新します。

---

## Notes
## 注意事項

- `privilegeCode` must already exist
  `privilegeCode` は既に存在している必要があります

- Only the role name can be updated
  更新できるのはロール名のみです

---

## Endpoint

```text
PUT /role/update
(ADMIN)
```

---

## Request

```json
{
  "privilegeCode": "AC",
  "nameOfPrivilege": "Acct",
  "enteredByUser": "Admin"
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
    --> B[Call /role/update API]

    B --> C{Role Exists?}

    C -->|Yes| D[Update role TABLE]

    D --> E[Success Response]

    C -->|No| F[Role Not Found Error]

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

| Table  | Action        |
|--------|---------------|
| role   | Update Record |

---

# 3. Deactivate Role
# ロール無効化

## English

Admin deactivates an existing role. The role is not permanently deleted but marked inactive.

## Japanese

管理者は既存のロールを無効化します。ロールは完全に削除されず、非アクティブとしてマークされます。

---

## Notes
## 注意事項

- Role is deactivated, not deleted
  ロールは削除ではなく非アクティブ化されます

- Only Admin can deactivate roles
  ロールの無効化は管理者のみが行えます

---

## Endpoint

```text
DELETE /user/deactivate
(ADMIN)
```

---

## Request

```json
{
  "privilegeCode": "AC",
  "nameOfPrivilege": "Accountent",
  "enteredByUser": "Admin"
}
```

---

## Flow Diagram

```mermaid
flowchart TD

    A[Admin]
    --> B[Call /user/deactivate API]

    B --> C{Role Exists?}

    C -->|Yes| D[Mark Role as Inactive]

    D --> E[Update role TABLE]

    E --> F[Success Response]

    C -->|No| G[Role Not Found Error]

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

| Table  | Action               |
|--------|----------------------|
| role   | Update Status to Inactive |

---

# 4. Get Active Roles
# アクティブロール取得

## English

Admin retrieves all currently active roles.

## Japanese

管理者は現在アクティブなすべてのロールを取得します。

---

## Endpoint

```text
GET /role/getActive
(ADMIN)
```

---

## Flow Diagram

```mermaid
flowchart TD

    A[Admin]
    --> B[Call /role/getActive API]

    B --> C[Query role TABLE WHERE status = active]

    C --> D[Return Active Roles List]

    %% Styles
    style A fill:#1E3A8A,color:#ffffff,stroke:#0F172A,stroke-width:2px
    style B fill:#2563EB,color:#ffffff,stroke:#1E40AF,stroke-width:2px
    style C fill:#7C3AED,color:#ffffff,stroke:#4C1D95,stroke-width:2px
    style D fill:#059669,color:#ffffff,stroke:#065F46,stroke-width:2px
```

---

# 5. Get All Roles
# 全ロール取得

## English

Admin retrieves all roles, including inactive ones.

## Japanese

管理者は非アクティブなロールを含む、すべてのロールを取得します。

---

## Endpoint

```text
GET /role/getAll
(ADMIN)
```

---

## Flow Diagram

```mermaid
flowchart TD

    A[Admin]
    --> B[Call /role/getAll API]

    B --> C[Query role TABLE - All Records]

    C --> D[Return All Roles List]

    %% Styles
    style A fill:#1E3A8A,color:#ffffff,stroke:#0F172A,stroke-width:2px
    style B fill:#2563EB,color:#ffffff,stroke:#1E40AF,stroke-width:2px
    style C fill:#7C3AED,color:#ffffff,stroke:#4C1D95,stroke-width:2px
    style D fill:#059669,color:#ffffff,stroke:#065F46,stroke-width:2px
```

---

# API Summary
# API サマリー

| Endpoint            | Method | Description (EN)        | Description (JP)       |
|---------------------|--------|-------------------------|------------------------|
| /role/save          | POST   | Create new role         | 新しいロールを作成     |
| /role/update        | PUT    | Update existing role    | 既存ロールを更新       |
| /user/deactivate    | DELETE | Deactivate a role       | ロールを無効化         |
| /role/getActive     | GET    | Fetch all active roles  | アクティブロールを取得 |
| /role/getAll        | GET    | Fetch all roles         | 全ロールを取得         |

---

# Complete Sequence Diagram
# 全体シーケンス図

```mermaid
sequenceDiagram

    participant Admin
    participant System
    participant Database

    Admin->>System: POST /role/save
    activate System

    System->>Database: INSERT INTO role
    activate Database
    Database-->>System: Success
    deactivate Database

    System-->>Admin: Success Response
    deactivate System

    Admin->>System: PUT /role/update
    activate System

    System->>Database: Check if role exists
    activate Database

    alt Role Found

        Database-->>System: Role OK

        System->>Database: UPDATE role SET nameOfPrivilege

        Database-->>System: Updated

        System-->>Admin: Success Response

    else Role Not Found

        Database-->>System: Not Found

        System-->>Admin: Error Response

    end

    deactivate Database
    deactivate System

    Admin->>System: DELETE /user/deactivate
    activate System

    System->>Database: UPDATE role SET status = inactive
    activate Database

    Database-->>System: Deactivated

    System-->>Admin: Success Response

    deactivate Database
    deactivate System

    Admin->>System: GET /role/getActive
    activate System

    System->>Database: SELECT * FROM role WHERE active
    activate Database

    Database-->>System: Active Roles

    System-->>Admin: Active Roles List

    deactivate Database
    deactivate System

    Admin->>System: GET /role/getAll
    activate System

    System->>Database: SELECT * FROM role
    activate Database

    Database-->>System: All Roles

    System-->>Admin: Full Roles List

    deactivate Database
    deactivate System
```