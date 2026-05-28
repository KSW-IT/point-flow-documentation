# DPS System Workflow Documentation
# DPS システム ワークフロー ドキュメント

---

# System Overview
# システム概要

## English

The DPS application is built around three core entities: **Role**, **Branch**, and **User**.
Access to all functions is governed by Role-Based Access Control (RBAC).

## Japanese

DPS アプリケーションは **ロール**、**ブランチ**、**ユーザー** の3つのコアエンティティで構成されています。
すべての機能へのアクセスはロールベースアクセス制御（RBAC）によって管理されます。

---

## Initial System State
## システム初期状態

| Entity  | Initial Value          | Notes                              |
|---------|------------------------|------------------------------------|
| Role    | ADMIN                  | System administrator role          |
| Role    | BRANCH MANAGER         | Branch-level management role       |
| Role    | DRIVER                 | Driver role — mobile login only    |
| User    | Super User / Admin     | Role: ADMIN — No branch assigned   |
| Branch  | None                   | No branches exist at startup       |

---

## Rules
## ルール

- Only Admin can assign permissions to a role
  権限の付与は管理者のみが行えます

- Drivers can only log in via the Mobile App
  ドライバーはモバイルアプリからのみログインできます

---

## Role Responsibilities
## ロールの責務

| Role           | Responsibilities (EN)                                         | Responsibilities (JP)                          |
|----------------|---------------------------------------------------------------|------------------------------------------------|
| ADMIN          | Manages Roles, Creates Branches, Assigns Permissions, Controls System | ロール管理、ブランチ作成、権限付与、システム制御 |
| BRANCH MANAGER | Manages Drivers, Transfers Points, Handles Withdrawals        | ドライバー管理、ポイント転送、出金対応          |
| DRIVER         | Receives Points, Requests Withdrawals                         | ポイント受取、出金申請                          |

---

## User Interface Access
## ユーザーインターフェースアクセス

| Interface  | Accessible By                              | Notes                                  |
|------------|--------------------------------------------|----------------------------------------|
| Web App    | Admin, Branch Manager, and other new roles | Drivers are NOT permitted on Web       |
| Mobile App | Driver only                                | Exclusively for Driver login and usage |

---

# Main Features
# メイン機能

---

# Feature 1 — User and Branch Entity Creation
# 機能1 — ユーザーおよびブランチエンティティ作成

## Step 1 — Login as Admin (Web)
## ステップ1 — 管理者としてログイン（Web）

```
Actor : Admin (Super User)
Interface : Web App
```

## Step 2 — Create a Branch
## ステップ2 — ブランチを作成する

```
Actor    : Admin
Action   : Create Branch
Example  : BRANCH 1
```

## Step 3 — Create a Branch Manager User
## ステップ3 — ブランチマネージャーユーザーを作成する

```
Actor    : Admin
Action   : Create User
Role     : BRANCH MANAGER
Branch   : BRANCH 1
Example  : MANAGER 1
```

## Step 4 — Create a Driver User
## ステップ4 — ドライバーユーザーを作成する

```
Actor    : Admin
Action   : Create User
Role     : DRIVER
Branch   : BRANCH 1
Example  : DRIVER 1
```

---

## Flow Diagram — User and Branch Creation
## フロー図 — ユーザーおよびブランチ作成

```mermaid
flowchart TD

    A[Admin Login - Web]
    --> B[Create Branch - BRANCH 1]

    B --> C[Create User - MANAGER 1\nRole: BRANCH MANAGER\nBranch: BRANCH 1]

    C --> D[Create User - DRIVER 1\nRole: DRIVER\nBranch: BRANCH 1]

    D --> E[Entity Setup Complete]

    %% Styles
    style A fill:#1E3A8A,color:#ffffff,stroke:#0F172A,stroke-width:2px
    style B fill:#0F766E,color:#ffffff,stroke:#134E4A,stroke-width:2px
    style C fill:#2563EB,color:#ffffff,stroke:#1E40AF,stroke-width:2px
    style D fill:#2563EB,color:#ffffff,stroke:#1E40AF,stroke-width:2px
    style E fill:#059669,color:#ffffff,stroke:#065F46,stroke-width:2px
```

---

# Feature 2 — Point Flow
# 機能2 — ポイントフロー

## Step 1 — Admin Credits Points to Branch
## ステップ1 — 管理者がブランチにポイントを付与する

```
Actor    : Admin
Action   : Credit Points to BRANCH 1
Result   : Points credited to BRANCH 1 balance
```

## Step 2 — Branch Manager Transfers Points to Driver
## ステップ2 — ブランチマネージャーがドライバーにポイントを転送する

```
Actor    : MANAGER 1
Action   : Transfer Points to DRIVER 1
Result   : Points deducted from BRANCH 1 and added to DRIVER 1 account
```

## Step 3 — Driver Requests Withdrawal
## ステップ3 — ドライバーが出金申請を行う

```
Actor     : DRIVER 1
Interface : Mobile App
Action    : Submit Withdrawal Request for some points
Result    : Withdrawal request created — pending Branch Manager response
```

## Step 4 — Branch Manager Responds to Withdrawal Request
## ステップ4 — ブランチマネージャーが出金申請に回答する

```
Actor    : MANAGER 1
Action   : Review DRIVER 1 Withdrawal Request
Options  : Fully Approve / Partially Approve / Reject
Result   : Points deducted from DRIVER 1 account upon approval
```

---

## Flow Diagram — Point Flow
## フロー図 — ポイントフロー

```mermaid
flowchart TD

    A[Admin Login - Web]
    --> B[Credit Points to BRANCH 1]

    B --> C[Points Added to Branch Balance]

    C --> D[MANAGER 1 Login - Web]

    D --> E[Transfer Points to DRIVER 1]

    E --> F[Points Deducted from BRANCH 1\nPoints Added to DRIVER 1]

    F --> G[DRIVER 1 Login - Mobile App]

    G --> H[Driver Submits Withdrawal Request]

    H --> I[MANAGER 1 Login - Web]

    I --> J{Branch Manager Decision}

    J -->|Fully Approve| K[Full Amount Approved]
    J -->|Partially Approve| L[Partial Amount Approved]
    J -->|Reject| M[Request Rejected]

    K --> N[Points Deducted from DRIVER 1]
    L --> N
    M --> O[No Point Change]

    %% Styles
    style A fill:#1E3A8A,color:#ffffff,stroke:#0F172A,stroke-width:2px
    style B fill:#2563EB,color:#ffffff,stroke:#1E40AF,stroke-width:2px
    style C fill:#059669,color:#ffffff,stroke:#065F46,stroke-width:2px
    style D fill:#1E3A8A,color:#ffffff,stroke:#0F172A,stroke-width:2px
    style E fill:#2563EB,color:#ffffff,stroke:#1E40AF,stroke-width:2px
    style F fill:#0F766E,color:#ffffff,stroke:#134E4A,stroke-width:2px
    style G fill:#1E3A8A,color:#ffffff,stroke:#0F172A,stroke-width:2px
    style H fill:#2563EB,color:#ffffff,stroke:#1E40AF,stroke-width:2px
    style I fill:#1E3A8A,color:#ffffff,stroke:#0F172A,stroke-width:2px
    style J fill:#D97706,color:#ffffff,stroke:#92400E,stroke-width:2px
    style K fill:#16A34A,color:#ffffff,stroke:#14532D,stroke-width:2px
    style L fill:#F59E0B,color:#000000,stroke:#92400E,stroke-width:2px
    style M fill:#B91C1C,color:#ffffff,stroke:#7F1D1D,stroke-width:2px
    style N fill:#7C3AED,color:#ffffff,stroke:#4C1D95,stroke-width:2px
    style O fill:#6B7280,color:#ffffff,stroke:#374151,stroke-width:2px
```

---

# RBAC Feature
# RBACフィーチャー（ロールベースアクセス制御）

---

# RBAC 1 — Role Entity Creation
# RBAC1 — ロールエンティティ作成

## Step 1 — Login as Admin (Web)
## ステップ1 — 管理者としてログイン（Web）

```
Actor     : Admin
Interface : Web App
```

## Step 2 — Create a New Role
## ステップ2 — 新しいロールを作成する

```
Actor    : Admin
Action   : Create Role
Example  : HR
```

## Step 3 — Create a User with the New Role
## ステップ3 — 新しいロールでユーザーを作成する

```
Actor    : Admin
Action   : Create User
Role     : HR
Example  : HR_1
```

---

## Flow Diagram — Role Creation
## フロー図 — ロール作成

```mermaid
flowchart TD

    A[Admin Login - Web]
    --> B[Create New Role - HR]

    B --> C[Create User - HR_1\nRole: HR]

    C --> D[Role and User Setup Complete]

    %% Styles
    style A fill:#1E3A8A,color:#ffffff,stroke:#0F172A,stroke-width:2px
    style B fill:#0F766E,color:#ffffff,stroke:#134E4A,stroke-width:2px
    style C fill:#2563EB,color:#ffffff,stroke:#1E40AF,stroke-width:2px
    style D fill:#059669,color:#ffffff,stroke:#065F46,stroke-width:2px
```

---

# RBAC 2 — Giving Permission to a Role
# RBAC2 — ロールへの権限付与

## Step 1 — Admin Assigns Permission
## ステップ1 — 管理者が権限を付与する

```
Actor    : Admin
Action   : Navigate to Permission Page
Action   : Assign "Create New User" function to Role HR
Example  : HR Role can now create users
```

## Step 2 — HR_1 Uses Granted Permission
## ステップ2 — HR_1 が付与された権限を使用する

```
Actor    : HR_1
Interface: Web App
Action   : Navigate to Create User Page
Action   : Create a new user with Role DRIVER
Example  : DRIVER 2 created
Note     : DRIVER 2 can now follow the same Point Flow as DRIVER 1
```

---

## Flow Diagram — Permission and Usage
## フロー図 — 権限付与と使用

```mermaid
flowchart TD

    A[Admin Login - Web]
    --> B[Go to Permission Page]

    B --> C[Assign Create User Function\nto Role HR]

    C --> D[Permission Saved]

    D --> E[HR_1 Login - Web]

    E --> F[Go to Create User Page\nPermission Granted by Admin]

    F --> G[HR_1 Creates DRIVER 2\nRole: DRIVER]

    G --> H[DRIVER 2 Created Successfully]

    H --> I[DRIVER 2 can now follow\nthe same Point Flow as DRIVER 1]

    %% Styles
    style A fill:#1E3A8A,color:#ffffff,stroke:#0F172A,stroke-width:2px
    style B fill:#2563EB,color:#ffffff,stroke:#1E40AF,stroke-width:2px
    style C fill:#D97706,color:#ffffff,stroke:#92400E,stroke-width:2px
    style D fill:#059669,color:#ffffff,stroke:#065F46,stroke-width:2px
    style E fill:#1E3A8A,color:#ffffff,stroke:#0F172A,stroke-width:2px
    style F fill:#2563EB,color:#ffffff,stroke:#1E40AF,stroke-width:2px
    style G fill:#0F766E,color:#ffffff,stroke:#134E4A,stroke-width:2px
    style H fill:#059669,color:#ffffff,stroke:#065F46,stroke-width:2px
    style I fill:#7C3AED,color:#ffffff,stroke:#4C1D95,stroke-width:2px
```

---

# Other Features
# その他の機能

---

# Feature 3 — Group Messaging
# 機能3 — グループメッセージング

## English

Admin can send group messages to one or more branches at once via the Web App.

## Japanese

管理者はWebアプリから一度に1つまたは複数のブランチにグループメッセージを送信できます。

---

## Notes
## 注意事項

- Only Admin can send group messages
  グループメッセージの送信は管理者のみが行えます

- Admin selects one or more target branches before sending
  送信前に管理者は対象ブランチを1つ以上選択します

- All users within the selected branches receive the message
  選択されたブランチ内のすべてのユーザーがメッセージを受信します

---

## Step 1 — Login as Admin (Web)
## ステップ1 — 管理者としてログイン（Web）

```
Actor     : Admin
Interface : Web App
```

## Step 2 — Send Group Message
## ステップ2 — グループメッセージを送信する

```
Actor    : Admin
Action   : Navigate to Send Message Page
Action   : Select target Branch(es)
Action   : Compose and Send Message
Result   : Message delivered to all users in selected branches
```

---

## Flow Diagram — Group Messaging
## フロー図 — グループメッセージング

```mermaid
flowchart TD

    A[Admin Login - Web]
    --> B[Go to Send Message Page]

    B --> C[Select Target Branch or Branches]

    C --> D[Compose Message]

    D --> E[Send Message]

    E --> F[Message Delivered to All Users\nin Selected Branches]

    %% Styles
    style A fill:#1E3A8A,color:#ffffff,stroke:#0F172A,stroke-width:2px
    style B fill:#2563EB,color:#ffffff,stroke:#1E40AF,stroke-width:2px
    style C fill:#D97706,color:#ffffff,stroke:#92400E,stroke-width:2px
    style D fill:#0F766E,color:#ffffff,stroke:#134E4A,stroke-width:2px
    style E fill:#7C3AED,color:#ffffff,stroke:#4C1D95,stroke-width:2px
    style F fill:#059669,color:#ffffff,stroke:#065F46,stroke-width:2px
```

---

# Complete System Sequence Diagram
# システム全体シーケンス図

```mermaid
sequenceDiagram

    participant Admin
    participant Manager as MANAGER 1
    participant Driver as DRIVER 1
    participant HR as HR_1
    participant System
    participant Database

    Note over Admin,Database: == SETUP PHASE ==

    Admin->>System: Login - Web
    Admin->>System: Create BRANCH 1
    System->>Database: INSERT branch_office, branch_info
    Database-->>System: Success
    System-->>Admin: Branch Created

    Admin->>System: Create MANAGER 1 (Role: BRANCH MANAGER, Branch: BRANCH 1)
    System->>Database: INSERT user
    Database-->>System: Success
    System-->>Admin: Manager Created

    Admin->>System: Create DRIVER 1 (Role: DRIVER, Branch: BRANCH 1)
    System->>Database: INSERT user
    Database-->>System: Success
    System-->>Admin: Driver Created

    Note over Admin,Database: == POINT FLOW PHASE ==

    Admin->>System: Credit Points to BRANCH 1
    System->>Database: INSERT cdt_pnt, UPDATE branch_info
    Database-->>System: Success
    System-->>Admin: Points Credited

    Manager->>System: Login - Web
    Manager->>System: Transfer Points to DRIVER 1
    System->>Database: INSERT giv_pnt, UPDATE branch_info, UPDATE driver_point
    Database-->>System: Success
    System-->>Manager: Transfer Complete

    Driver->>System: Login - Mobile App
    Driver->>System: Submit Withdrawal Request
    System->>Database: INSERT req_pnt
    Database-->>System: Request Created
    System-->>Driver: Request Submitted

    Manager->>System: Respond to Withdrawal Request
    System->>Database: UPDATE req_pnt
    Database-->>System: Updated
    System-->>Manager: Response Recorded

    Note over Admin,Database: == RBAC PHASE ==

    Admin->>System: Create Role HR
    System->>Database: INSERT role
    Database-->>System: Success

    Admin->>System: Create User HR_1 (Role: HR)
    System->>Database: INSERT user
    Database-->>System: Success

    Admin->>System: Assign Create User Permission to Role HR
    System->>Database: INSERT permission mapping
    Database-->>System: Success
    System-->>Admin: Permission Assigned

    HR->>System: Login - Web
    HR->>System: Create DRIVER 2 (Role: DRIVER)
    System->>Database: INSERT user
    Database-->>System: Success
    System-->>HR: DRIVER 2 Created

    Note over Admin,Database: == GROUP MESSAGING PHASE ==

    Admin->>System: Go to Send Message Page
    Admin->>System: Select Branches and Send Message
    System->>Database: INSERT message record
    Database-->>System: Success
    System-->>Admin: Message Sent to Selected Branches
```