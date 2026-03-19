# Participium 📋

<div align="center">
  <img src="docs/images/logo.jpg" alt="Participium Logo" width="200" height="200"/>
</div>

**Participium** is an integrated platform for managing and monitoring civic participation reports. It enables citizens to report issues, facilitating communication between public administrators and the community.

---

## 📑 Table of Contents

- [Getting Started](#-getting-started)
  - [Access Credentials](#access-credentials)
  - [User Roles & Permissions](#user-roles--permissions)
  - [Main Features](#main-features)
  - [Telegram Bot](#telegram-bot)
  - [Quick Start](#quick-start)
- [Technical Setup & Deployment](#-technical-setup--deployment)
- [For Developers](#-for-developers)

---

## 🎯 Getting Started

### Access Credentials

Use these credentials to access Participium based on your role:

| Role | Username | Email | Password | Access Level |
|------|----------|-------|----------|--------------|
| **Citizen** | mneri | mneri@team4.it | citizenTeam4 | ✅ Full |
| **Technical Officer** | mcurie | mcurie@team4.it | tOfficerTeam4 | ✅ Full |
| **Public Relations Officer** | arossi | arossi@team4.it | PrOfficerTeam4 | ✅ Full |
| **External Maintainer** | everdi | everdi@team4.it | extMaintWithTeam4 | ✅ Limited |
| **Administrator** | admin | - | adminTeam4 | ✅ Full |

### User Roles & Permissions

Each role in Participium has specific rights and responsibilities:

#### 👤 **Citizen**
- **What you can do:**
  - ✏️ Create new reports and issues
  - 👁️ View your reports and their status
  - 💬 Add notes to your reports
  - 📲 Receive notifications via Telegram (if connected)

#### 🛠️ **Technical Officer**
- **What you can do:**
  - ✅ Approve/Reject technical reports
  - 👁️ View all pending reports
  - 📝 Add technical notes
  - 📊 Manage report classification (category, location)

#### 📢 **Public Relations Officer**
- **What you can do:**
  - ✅ Approve/Reject reports for communication
  - 💌 Manage citizen communication
  - 📋 View all report statuses
  - 📝 Fill in decisions and reasoning

#### 🏢 **External Maintainer**
- **What you can do:**
  - 👁️ View only assigned reports
  - 📝 Add technical notes
  - ✅ Mark completion of actions

#### 🔑 **Administrator**
- **What you can do:**
  - 🔧 Configure users and roles
  - 📊 View global statistics
  - ⚙️ Manage system settings

### Main Features

#### 📝 Report Creation
1. Select the **category** of the issue (e.g., "Roads", "Lighting")
2. Choose the **location** on the map
3. Add **photos and description**
4. Submit — the report will be tracked automatically

#### 📊 Status Tracking
Each report has a clear status:
- 🟡 **Pending** - Awaiting review
- 🟢 **Approved** - Accepted, in progress
- 🔴 **Rejected** - Rejected with reason
- ✅ **Completed** - Resolved

#### 💬 Notes System
- Add private comments to reports
- See all actions taken
- Track complete history

### Telegram Bot

Connect your Participium account to **Telegram** to receive real-time notifications:

1. Open the Participium Telegram bot
2. Start the conversation (`/start`)
3. Link your account
4. You'll receive notifications when:
   - ✏️ You create a report
   - ✅ A report is approved
   - 🔴 A report is rejected
   - 💬 You receive a new comment

### Quick Start

1. **Clone the repository**:

```bash
git clone https://github.com/Skeitt/Participium.git
cd Participium
```

2. **Set up environment variables**:
   - Copy `.env.example` to `.env` in the root (or create `.env` manually) and fill in the required values (e.g. `BOT_TOKEN`, `DATABASE_URL`, etc.).
   - The Docker Compose file will automatically load variables from `.env` in the current directory.

3. **Start all services with Docker Compose**:

```bash
docker compose pull
docker compose up -d
```

To use a custom `.env` file (e.g., `.env.prod`):

```bash
docker compose --env-file .env.prod up -d
```

4. **Verify that the containers are running**:

```bash
docker ps
```

5. **Access the application**:
   - Backend: [http://localhost:3000](http://localhost:3000)
   - The Telegram bot will respond to messages if the token is valid.

### Available Services

| Service | Port | Description |
|---------|------|-------------|
| **participium** | 3000 | Web application |
| **telegram_bot** | - | Bot (isolated environment) |
| **db** | 5432 | Main Database |
| **test_db** | 5433 | Test Database |

### Main Environment Variables

```env
# Database
DATABASE_URL=postgresql://user:password@db:5432/participium
TEST_DATABASE_URL=postgresql://user:password@test_db:5433/participium_test

# Telegram Bot
BOT_TOKEN=your_telegram_bot_token_here
BOT_ADMIN_ID=your_admin_telegram_id

# Application
NEXTAUTH_SECRET=your-secret-key
NEXTAUTH_URL=http://localhost:3000
```

### Deploy with Docker Hub

To run Participium using pre-built images from Docker Hub:

```bash
docker run -d --name participium \
  --env-file .env \
  -p 3000:3000 \
  skeitt/participium-team-4:latest

docker run -d --name participium_bot \
  --env-file .env \
  skeitt/participium-team-4-bot:latest
```

> Make sure you have a `.env` file in the current directory with all required variables.

---

## 🛠️ Technical Setup & Deployment

### Technology Stack

**Frontend & Backend:**
- Next.js 14 (App Router)
- TypeScript
- Prisma ORM
- PostgreSQL

**Telegram Bot:**
- Node.js + Telegraf
- TypeScript

**Infrastructure:**
- Docker & Docker Compose
- CI/CD with GitHub Actions

### Project Structure

```
participium-team-4/
├── participium/                # Next.js Frontend + Backend API
│   ├── src/
│   │   ├── app/               # Next.js app router
│   │   ├── components/        # React components
│   │   ├── lib/
│   │   │   ├── controllers/   # HTTP controllers
│   │   │   ├── services/      # Business logic
│   │   │   ├── repositories/  # Data access
│   │   │   ├── dtos/          # Zod schemas
│   │   │   └── utils/         # Utility functions
│   │   └── types/             # TypeScript types
│   ├── prisma/
│   │   ├── schema.prisma      # Database schema
│   │   └── migrations/        # DB migrations
│   └── package.json
│
├── bot/                        # Telegram Bot
│   ├── bot.ts                 # Entry point
│   ├── handlers/              # Command handlers
│   ├── dtos/                  # DTOs
│   ├── utils/                 # Utilities
│   ├── package.json
│   └── jest.config.js
│
├── docker-compose.yml         # Service orchestration
└── README.md
```

### Database

**Managed by Prisma** with automatic migrations:

```bash
# Generate/apply migrations
npx prisma migrate dev --name add_feature

# Seed test data
node prisma/admin.ts
node prisma/citizen.ts

# View database
npx prisma studio
```

Data is persisted via Docker volumes (`pgdata` and `pgdata_test`).

---

## 👨‍💻 For Developers

### Clone and Local Setup

```bash
git clone https://github.com/Skeitt/Participium.git
cd Participium

# Install dependencies
cd participium && npm install && cd ..
cd bot && npm install && cd ..

# Setup environment
cp .env.example .env
# Edit .env with your values
```

### Development in Watch Mode

```bash
# Terminal 1: Backend + Frontend (Next.js)
cd participium
npm run dev

# Terminal 2: Bot (Telegram)
cd bot
npm run dev

# Terminal 3: Database with Prisma Studio
cd participium
npx prisma studio
```

### Testing

```bash
# Backend API tests
cd participium
npm run test

# Bot tests
cd bot
npm run test

# Test coverage
npm run test:coverage
```

### Production Build

```bash
# Build Next.js
cd participium
npm run build
npm run start

# Build Bot
cd bot
npm run build
npm start
```

### Build & Push Docker Images

For multi-platform build (amd64 + arm64):

```bash
# Setup builder
docker buildx create --use --name participium-builder

# Build backend
cd participium
docker buildx build --platform linux/amd64,linux/arm64 \
  -t your_username/participium:latest --push .

# Build bot
cd bot
docker buildx build --platform linux/amd64,linux/arm64 \
  -t your_username/participium-team-4-bot:latest --push .
```

> Replace `your_username` with your Docker Hub username.

---

**Last Updated:** March 2026  
**Repository:** [Skeitt/Participium](https://github.com/Skeitt/Participium)
