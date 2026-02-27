# Flux ERP - Modular Local-First System

## 🚀 Getting Started

This project is structured as a **Monorepo** with a NestJS backend and a React (Vite) frontend.

### Prerequisites

- Node.js (v18+)
- Docker (optional, for PostgreSQL)

---

### 📦 Quick Start (SQLite Mode)

The project is currently configured to use **SQLite** (`dev.db`) by default. This allows you to run the entire stack locally without needing Docker or a separate database server.

#### 1. Backend Setup

```bash
cd backend
npm install
npx prisma generate
npx prisma migrate dev --name init
npm run start:dev
```

The backend will start at `http://localhost:3000`.
- **Admin User**: `admin@fluxerp.com`
- **Password**: `adminpassword`

#### 2. Frontend Setup

```bash
cd frontend
npm install
npm run dev
```

The frontend will be available at `http://localhost:5173`.

---

### 🐘 Switching to PostgreSQL (Recommended for Production)

To switch from SQLite to PostgreSQL, follow these steps:

1.  **Start the Database Container**:
    Run the provided `docker-compose.yml` file from the root directory:
    ```bash
    docker-compose up -d
    ```

2.  **Update Environment Variables**:
    Modify `backend/.env`:
    ```env
    # Comment out SQLite
    # DATABASE_URL="file:./dev.db"

    # Uncomment PostgreSQL
    DATABASE_URL="postgresql://admin:password@localhost:5432/flux_erp?schema=public"
    ```

3.  **Update Prisma Schema**:
    Modify `backend/prisma/schema.prisma`:
    ```prisma
    datasource db {
      provider = "postgresql" // Change from "sqlite" to "postgresql"
      url      = env("DATABASE_URL")
    }

    // Note: SQLite does not support native Enums, so if you switch to Postgres,
    // you can uncomment the Enum definitions and update the model fields to use them
    // instead of String if you wish for stricter typing.
    // For now, keeping them as String works for both.
    ```

4.  **Run Migrations**:
    ```bash
    cd backend
    # Remove the existing migration folder if it contains SQLite specific SQL
    rm -rf prisma/migrations

    npx prisma migrate dev --name init_postgres
    ```

5.  **Restart Backend**:
    ```bash
    npm run start:dev
    ```

---

## 🏗️ Architecture

- **Backend**: NestJS (Modular)
- **Database**: Prisma ORM (SQLite / PostgreSQL)
- **Frontend**: React + Vite + Tailwind CSS
- **Authentication**: JWT + Local Strategy

## 🧩 Modules Implemented (Phase 1)

- **Auth**: Login, JWT generation.
- **Users**: Admin seeding, User management.
- **Products**: CRUD, Margin Calculation.
- **Categories**: Basic categorization, Default seeding.
- **Sales/Stock/Customers**: Placeholder modules (Architecture ready).
