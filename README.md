# Nexora Labs — Technical Assessment Platform

A full-stack technical assessment platform built with **Next.js (App Router)**, **Tailwind CSS**, **Prisma**, and **NextAuth**. The platform includes a public-facing landing page with contact/lead capture, a candidate assessment flow, and a secured admin dashboard for managing assessments, questions, candidates, and submissions.

---

## 1. Tech Stack

| Layer          | Technology                          |
|----------------|--------------------------------------|
| Framework      | Next.js 14+ (App Router)             |
| Styling        | Tailwind CSS                         |
| ORM / DB       | Prisma ORM + PostgreSQL              |
| Auth           | NextAuth.js (Credentials/Admin auth) |
| Language       | TypeScript                           |
| Deployment     | Vercel                               |

---

## 2. Project Structure

```
nexora-labs/
├── prisma/
│   ├── schema.prisma          # Assessment, Question, Candidate, Submission, AdminUser
│   └── migrations/
├── src/
│   ├── app/
│   │   ├── (public)/          # Landing page & marketing routes
│   │   ├── (admin)/           # Admin login & dashboard routes
│   │   ├── api/
│   │   │   ├── auth/[...nextauth]/route.ts
│   │   │   ├── contact/route.ts
│   │   │   └── assessments/route.ts
│   │   └── layout.tsx
│   ├── components/            # UI components (landing + admin)
│   ├── lib/                   # Prisma client, auth config, utils
│   └── types/
├── public/
├── .env.example
├── next.config.js
├── tailwind.config.ts
└── package.json
```

---

## 3. Prerequisites

- Node.js **v18.18+** (v20 LTS recommended)
- npm / pnpm / yarn
- A PostgreSQL database (local, [Neon](https://neon.tech), [Supabase](https://supabase.com), or [Vercel Postgres](https://vercel.com/storage/postgres))
- Git

---

## 4. Environment Variables

Create a `.env` file in the project root based on `.env.example`:

```env
# --- Database ---
DATABASE_URL="postgresql://<user>:<password>@<host>:5432/<db_name>?schema=public"

# --- NextAuth ---
NEXTAUTH_URL="http://localhost:3000"
NEXTAUTH_SECRET="generate-with-openssl-rand-base64-32"

# --- Admin Seed Credentials (used only for initial seeding) ---
ADMIN_EMAIL="admin@nexoralabs.com"
ADMIN_PASSWORD="ChangeMe123!"

# --- Contact Form / Email (optional) ---
SMTP_HOST=""
SMTP_PORT=""
SMTP_USER=""
SMTP_PASSWORD=""
CONTACT_RECEIVER_EMAIL=""

# --- App ---
NEXT_PUBLIC_APP_URL="http://localhost:3000"
```

**Generate a secure `NEXTAUTH_SECRET`:**
```bash
openssl rand -base64 32
```

> ⚠️ Never commit `.env` to version control. It is already included in `.gitignore`.

---

## 5. Local Installation & Setup

### Step 1 — Clone the repository
```bash
git clone https://github.com/<your-org>/nexora-labs.git
cd nexora-labs
```

### Step 2 — Install dependencies
```bash
npm install
```

### Step 3 — Configure environment variables
```bash
cp .env.example .env
# then edit .env with your actual values
```

### Step 4 — Generate Prisma Client
```bash
npx prisma generate
```

### Step 5 — Run database migrations
```bash
npx prisma migrate dev --name init
```
This creates the tables for `Assessment`, `Question`, `Candidate`, `Submission`, and `AdminUser`, and generates a migration history under `prisma/migrations/`.

### Step 6 — (Optional) Seed the database
If a seed script is configured in `package.json` (`prisma.seed`):
```bash
npx prisma db seed
```
This typically creates the default `AdminUser` from your `.env` credentials.

### Step 7 — Run the development server
```bash
npm run dev
```
Visit **http://localhost:3000** to view the landing page, and **http://localhost:3000/admin/login** for the admin dashboard.

---

## 6. Useful Prisma Commands

| Command | Purpose |
|---|---|
| `npx prisma studio` | Open a visual DB browser at `localhost:5555` |
| `npx prisma migrate dev` | Create & apply a new migration (dev) |
| `npx prisma migrate deploy` | Apply pending migrations (production) |
| `npx prisma db push` | Push schema changes without a migration file (prototyping only) |
| `npx prisma generate` | Regenerate the Prisma Client after schema changes |
| `npx prisma migrate reset` | ⚠️ Drops DB, reapplies all migrations + seed |

---

## 7. Build & Production Run (local test)

```bash
npm run build
npm run start
```

---

## 8. Testing Checklist Before Deployment

- [ ] All API routes (`/api/contact`, `/api/assessments`) tested with valid/invalid payloads
- [ ] NextAuth admin login flow verified (session + protected routes)
- [ ] Prisma schema matches production DB (`prisma migrate status`)
- [ ] Environment variables validated for production values (no localhost/test secrets)
- [ ] Landing page responsive across breakpoints
- [ ] Admin dashboard route protected via middleware/session check

---

## 9. Available Scripts

```json
"scripts": {
  "dev": "next dev",
  "build": "next build",
  "start": "next start",
  "lint": "next lint",
  "postinstall": "prisma generate"
}
```

---

## 10. License

Internal technical assessment project — Nexora Labs. All rights reserved.
